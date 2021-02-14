---
layout: post
title: How to start contributing to Parse Server
date: 2021-02-14 15:00 +0100
comments: true
author: dblythy
categories: [Learn, Tutorial, Community, NodeJS]
---

The most amazing feature of Parse Server is that it's accessible for developers of all skill levels.

Personally, I started playing around with Objective-C in 2012.

Parse was a way for me to build a complete online app, without having the in-depth knowledge of _how_ to build networking, storage, user systems, etc.

The more I built with Parse, the more I learnt JavaScript, which has fortunately allowed me to contribute to Parse Server.

Although it might seem daunting, contributing to our great open-source project is encouraged to all developers, and in this blog post, I'm going to give you an insight as to how.

<!-- more -->

## Cloning Parse Server

Ok. I like to use GitHub desktop as it's a visual git tool and sometimes I find git to be confusing. The first step after installing is to make a copy (or "clone") of the Parse Server repo locally.

- Press _File_ > _Clone Repository_, select 'url' and type [https://github.com/parse-community/parse-server](https://github.com/parse-community/parse-server)

<img width="960" alt="Cloning Parse Server" src="https://user-images.githubusercontent.com/4974769/102356603-8676a700-4001-11eb-8c8a-a9a2be910b57.png">

Next, select "Clone".

Ok, great! Now we have a local copy of Parse Server.

## Creating a Branch

In order to add code to Parse Server, we need to create a "Branch". This allows git to track any differences from our "Branch" vs the "master" branch, so we can hopefully merge our changes with the main project in the future.

First, set current repository to the clone of Parse Server, and then press the branch.

<img width="960" alt="Creating a Branch" src="https://user-images.githubusercontent.com/4974769/102357283-495ee480-4002-11eb-9c95-d84982e135b2.png">

Next, select "New Branch. Name your branch appropriately according to the feature you are working on.

<img width="960" alt="Creating a Branch" src="https://user-images.githubusercontent.com/4974769/102357356-52e84c80-4002-11eb-8066-5285e3fb8791.png">

For this blog post, I am working on adding `request.context` to an `afterFind` trigger, so I will name this branch `afterFind-context`.

## Diving in

Ok, now for the fun part! Next, select "Open the repository in your external editor". I am using Visual Studio Code.

You will notice there are a lot of files here. Don't be alarmed, we _try_ to keep things as intuitive and organized as possible.

Let's open a new terminal and run `npm run watch` so that changes to the src folder take effect.

<img width="960" alt="npm run watch" src="https://user-images.githubusercontent.com/4974769/102359017-60063b00-4004-11eb-861e-7ac457d44413.png">

<img width="960" alt="Screen Shot 2020-12-17 at 1 08 04 am" src="https://user-images.githubusercontent.com/4974769/102359009-5d0b4a80-4004-11eb-92ed-d2c1c62f34ec.png">


### Spec Files

The first thing I like to do for an issue like this, is write a failing test case so I can be clear as to what I am fixing.

If you're not familiar with test cases, they are essentially javascript files that automatically test and ensure that logic prevails. Parse Server uses Jasmine tests to ensure that new features do not affect existing functionality.

You will find all the Parse Server testing under `/spec`.

The issue I am fixing is related to Cloud Code, so the spec file I am adding to is `CloudCode.spec.js`.

<img width="960" alt="Screen Shot 2020-12-17 at 1 16 15 am" src="https://user-images.githubusercontent.com/4974769/102359907-795bb700-4005-11eb-93fe-a37ac966eb2a.png">

Here I have added my new test. You will notice that the code here is the same as you'd use in Parse Cloud code, with a few small changes.

- `it` is used to tell Jasmine that this is a new test. `fit` is so that Jasmine will only run this test whilst I debug.
- `expect` is Jasmine method used to ensure expected outcomes of logic. In is example, an error will be thrown if `request.context.a` is not `a`.

To run this test, click _Terminal_ > _Split Terminal_ and in the new terminal window, type `npm test spec/CloudCode.spec.js`. This might take a moment.

Here are the results:

<img width="960" alt="Screen Shot 2020-12-17 at 1 26 31 am" src="https://user-images.githubusercontent.com/4974769/102361054-e754ae00-4006-11eb-87d6-6c07d0b6e729.png">

As expected, the test failed with "Error: Cannot read property 'a' of undefined". This confirms that this is indeed a bug, or a feature that is yet to be extended. So let's get to work!

## Isolating and Fixing

It can be daunting at first to work out where the changes need to be made. The Parse Core team and other contributors are always happy to give tips and pointers and would rather your time not go wasted, so always feel free to ask in the [community forum](https://community.parseplatform.org). The [notable files](https://docs.parseplatform.org/parse-server/guide/#notable-files) is also a good resource too. It's unrealistic for us to expect _anyone_ new to be fully across the code base, so there is no shame in asking for pointers.

The file I am working on to start is `Triggers.js`. Here's the process I went to find that:

- `Parse.Cloud.js` is where the cloud functions are handled, and the afterFind section has the code: `triggers.Types.afterFind`
- Searching the project (control / command F) for `triggers.Types.afterFind` reveals `RestQuery.js`
- `RestQuery.js` has a function `RestQuery.prototype.runAfterFindTrigger` that calls the trigger, `triggers.maybeRunAfterFindTrigger`, which is located in `Triggers.js`

Looking at the other functions, they all have `context` passed to them, but `maybeRunAfterFindTrigger` does not. Let's try apply the existing logic to `maybeRunAfterFindTrigger`

No `context`:
<img width="960" alt="Screen Shot 2020-12-17 at 1 44 33 am" src="https://user-images.githubusercontent.com/4974769/102363348-71057b00-4009-11eb-8b73-6868bd1b3815.png">

`context`:
<img width="960" alt="Screen Shot 2020-12-17 at 1 45 17 am" src="https://user-images.githubusercontent.com/4974769/102363445-867aa500-4009-11eb-9d74-1388a551430d.png">

So, let's add `context` in, as it is in the other functions:

<img width="960" alt="Screen Shot 2020-12-17 at 1 46 59 am" src="https://user-images.githubusercontent.com/4974769/102363696-c3469c00-4009-11eb-8413-318b4e6db547.png">

We also need to add the afterFind type to triggers with context:

<img width="960" alt="Screen Shot 2020-12-17 at 2 32 31 am" src="https://user-images.githubusercontent.com/4974769/102369592-3c48f200-4010-11eb-8541-e82dfc4eee4c.png">

Cool! However, when I searched `maybeRunAfterFindTrigger`, I noticed that no context element was passed from RestQuery. So we'll add that:

<img width="960" alt="Screen Shot 2020-12-17 at 1 49 28 am" src="https://user-images.githubusercontent.com/4974769/102363990-1b7d9e00-400a-11eb-95b5-03c5fb9bc3df.png">

Next, `context` is never passed to `RestQuery.js`, so it won't know what `this.context` is. Let's add that too, with a similar pattern to `RestWrite.js`:

<img width="960" alt="Screen Shot 2020-12-17 at 1 51 45 am" src="https://user-images.githubusercontent.com/4974769/102364345-6f888280-400a-11eb-9882-c945db735ecc.png">

Ok great. Now we've got to pass the final context parameter to `RestQuery.js`, and then we should be good to run our test again. The context argument needs to be added to `new RestQuery(...` in `rest.js`

<img width="960" alt="Screen Shot 2020-12-17 at 2 00 57 am" src="https://user-images.githubusercontent.com/4974769/102368856-7239a680-400f-11eb-91fb-b813be492c74.png">


## Re-running the tests

I'm _pretty sure_ I've added context everywhere it's needed. I can always come back and continue editing the files, or temporarily add `console.log` in a few places to work out what's going on.

Let's run `npm test spec/CloudCode.spec.js` and see if the test passes:

### Result:
```
Started
.


Ran 1 of 130 specs
1 spec, 0 failures
```

Awesome, this means that the test passed and `request.context` is now available in `afterFind` triggers! 

## Preparing for Merge

In order to merge with the master repo, let's check a few things:

- Replace `fit` in tests with `it`. It's important that all new code added by the new features are captured by tests, so write more tests if required.
- Run `npm test` to make sure the new feature doesn't break any other feature
- Run `npm run prettier` to clean up code style
- Run `npm run lint-fix` to fix any lint issues
- Run `npm run lint` to identify any remaining lint issues
- Add our changes to the changelog

So, we're all good to go! Let's jump back into GitHub desktop.

## Publishing your Branch

<img width="960" alt="Screen Shot 2020-12-17 at 2 38 12 am" src="https://user-images.githubusercontent.com/4974769/102370204-ea549c00-4010-11eb-9993-d3d8b5edf2ac.png">

Here you can review all the changes you've made compared to the master branch. In order to upload these changes, type in a summary, and press _Commit_.

- If you see "You do not have write access to Parse-Server. Want to create a fork?", press _Create a Fork_, and then _Fork this Repository_

The summary of the first commit is what will show under GitHub file structures, so I try to give a little explanation here.

Today I'll use:

**Fix: context for afterFind**

Next, hit _Publish Branch_.

Now, your branch is available on GitHub. You can install this fork and test it on your own Parse Server using `npm install github:myUsername/parse-server#my-awesome-feature`.

GitHub Desktop will now say "Create a pull request from your current branch". Press _Create Pull Request_. This will open GitHub in your browser to create a pull request (aka PR) that will add your changes to the master repo.

All you have to do from here, is fill out the form, and hit _Create Pull Request_. The core Parse Team will review your changes, provide feedback, and eventually merge the changes with the master.

[You can see my PR for this feature here.](https://github.com/parse-community/parse-server/pull/7078)

**And that's all! You're on the way to becoming a Parse Contributor!**

Remember, if you're willing to contribute to our amazing project, the Core Team and many other contributors (such as myself 🤩) will be more than happy to help you learn. We are after all, a community.

Thanks for reading!
