---
id: 2370
title: Building DryDock on Parse
date: 2014-06-19T22:42:35+00:00
author: nancyxiao
layout: post
guid: http://blog.parse.com/?p=2370
permalink: /learn/engineering/building-drydock-on-parse/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3678709062"
categories:
  - Engineering
---
At Venmo, we're always looking for ways to improve our tooling and processes for rapid deployment. At any given time we can have upwards of 4 or 5 independent features in-flight, and we need ways to get prototypes into the hands of our team on a daily basis.

TestFlight and HockeyApp both do a good job of assisting with build distribution, but sending links around with random builds has a few problems, worst of which is 'orphaned installs'. Team members can get stuck on the last build they were prompted to install, reducing the dogfood testing pool and preventing them from getting new features / fixes.

We decided that a good solution to this would be to create an internal installer app that would give team members instant, self-serve access to the latest internal builds, experiments, and also versions released to the App Store.

**Getting Started**

Creating the app didn't seem like a large challenge, but one key question at the outset was: Where do we store the builds, and how do we find new builds from the app?

First we considered writing a lightweight Sinatra application to manage and serve up available builds, but we didn't like that because it would just be one more thing to manage. We also wanted to make DryDock open-source and allow others to use it – having to set up a web service would have made using DryDock far less appealing.

Parse popped into our minds from a developer day they ran at Facebook's campus last year and so, we decided to take it for a spin and see if it would do what we were looking for. Our requirements for DryDock's backend were quite simple:

<ul class="standard-list">
  <li>
    Allow storing a list of builds with basic string attributes
  </li>
  <li>
    Visibility can be controlled simply (e.g. for private builds) <em>**future</em>
  </li>
  <li>
    Some interface for developers to manage builds
  </li>
  <li>
    Minimal changes to the iOS code should be needed for a developer to retrieve from their own private builds service
  </li>
  <li>
    As simple as possible to deploy & maintain
  </li>
</ul>

** DryDock doesn't currently support visibility but it's something that we want to build in the future.

Parse seemed to support everything that we were looking for, so we got started.

**Pod "Parse"**

We added the Parse pod in our new Podfile (if you don't know about Cocoapods, [Check it out](http://cocoapods.org/)) and Parse was ready to go.

Within about 20 minutes, we had a working prototype that would read a list of builds from Parse and display them.

There are two ways to approach modeling in Parse -- one is to subclass PFObject (the synchronizable object base-class in Parse) with your custom fields. The other is to simply use a PFObject as a dictionary, and it will create columns on the remote datastore to hold your data.

In our case, we only wanted 6-7 fields on an object and saw no need to subclass, so we just used PFObject as-is.

Populating our tableview looks like this...

<pre class="brush: c; gutter: false">PFQuery *query = [PFQuery queryWithClassName:VDDModelApp];   
[query whereKeyExists:VDDAppKeyName];    
[query findObjectsInBackgroundWithBlock:^(NSArray *objects, NSError *error) {         
    self.apps = objects;     
    [self.tableView reloadData]; 
}];
</pre>

Okay.. it's actually a little more complicated than that, but barely! We chose to break out all our model keys into constants to ensure consistency between calls and access.

[<img class="alignnone size-full wp-image-2371" src="{{ site.url }}/assets/wp-content/uploads/2014/06/model_keys.png" alt="Break out all model keys" width="834" height="315" />]({{ site.url }}/assets/wp-content/uploads/2014/06/model_keys.png)

Now our apps array property contains all the remote apps as PFObjects. You can then simply configure a cell by extracting the properties from the app for the current cell.

<pre class="brush: c; gutter: false">- (void)configureForApp:(PFObject *)app {
    self.app = app;      
    self.appNameLabel.text = app[VDDAppKeyName];
    self.appDescriptionLabel.text = app[VDDAppKeyDescription];
}
</pre>

Now we have a fully populated table view. It doesn't look very pretty though, so let's add the app icon.

[<img class="alignnone size-full wp-image-2372" src="{{ site.url }}/assets/wp-content/uploads/2014/06/add_app_icon.png" alt="Adding app icon to fully populated table view" width="834" height="313" />]({{ site.url }}/assets/wp-content/uploads/2014/06/add_app_icon.png)

Icons are stored as PFFile attachments to the object, so we have to download the file and then set an ImageView's image to the response.

<pre class="brush: c; gutter: false">PFFile *image = app[VDDAppKeyImage]; 
[image getDataInBackgroundWithBlock:^(NSData *data, NSError *error) {
    if (data && !error) {
        self.appIconView.image = [UIImage imageWithData:data];     
    }
}];
</pre>

Doesn't that look better...

[<img class="alignnone wp-image-2373 size-medium" src="{{ site.url }}/assets/wp-content/uploads/2014/06/post_icon-238x300.png" alt="Internal builds screen shot after adding the app icon" width="238" height="300" />]({{ site.url }}/assets/wp-content/uploads/2014/06/post_icon.png)

**Using DryDock for Your Own Builds**

Our goal with DryDock was not only to be able to use it at Venmo, but also to be able to share it and have other companies easily use DryDock for their own experimentation and build distribution.

Having to export and import all of the column names in order to create the columns for the Parse data browser seemed like a pain.  So, we decided to do an auto-creation of sample data in the app. If there are no apps the first time you run it, it will create a sample app…thereby creating the columns.

<pre class="brush: c; gutter: false">void createDemoObject() {
    NSData *imageData = UIImageJPEGRepresentation([UIImage imageNamed:@"VenmoIcon"], 1.0);
    PFFile *file = [PFFile fileWithName:@"image.png" data:imageData];      
    [file saveInBackgroundWithBlock:^(BOOL succeeded, NSError *error) {
        if (!succeeded) {
            return;
        }
        PFObject *testObject = [PFObject objectWithClassName:VDDModelApp];
        testObject[VDDAppKeyName] = @"Venmo";
        testObject[VDDAppKeyDescription] = @"Venmo Dogfood Builds";
        testObject[VDDAppKeyType] = @(1);
        testObject[VDDAppKeyVersionNumber] = @"1";
        testObject[VDDAppKeySharable] = @(YES);
        testObject[VDDAppKeyShareUrl] = @"http://someshareurl/";
        testObject[VDDAppKeyInstallUrl] = @"itms-service://someinstallurl/";  
        testObject[VDDAppKeyImage] = file;
        [testObject saveInBackgroundWithBlock:^(BOOL succeeded, NSError *error) {
       
        }];
    }]; 
}
</pre>

**Versioning**

One of the key problems that we wanted to solve with DryDock was stopping users of experimental builds from getting orphaned and left on the last experimental build they installed. In order to do this, we use DryDock along with [VENVersionTracker](https://github.com/venmo/VENVersionTracker) -- our version tracking library (which we're also migrating to Parse right now!) You can read more about how we use them together to manage builds in our [blog post](http://blog.venmo.com/hf2t3h4x98p5e13z82pl8j66ngcmry/2014/4/6/g7i4tdyfp59g6jhmbmmy1w6bx4icwb).

We hope that DryDock is useful and helps you to increase the rate at which you experiment internally! Feedback and pull-requests very welcome!

<p class="p1">
  <em>For more on this post and its original form by Chris Maddern, head over to the Venmo blog <a href="http://blog.venmo.com/hf2t3h4x98p5e13z82pl8j66ngcmry/2014/4/6/g7i4tdyfp59g6jhmbmmy1w6bx4icwb" target="_blank">here</a>.</em>
</p>