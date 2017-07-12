---
id: 3551
title: 'Building a Killer WebRTC Video Chat App using PubNub &#038; Parse'
date: 2015-05-19T13:10:03+00:00
author: nancyxiao
layout: post
guid: http://blog.parse.com/?p=3551
permalink: /learn/building-a-killer-webrtc-video-chat-app-using-pubnub-parse/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3777832473"
categories:
  - Learn
  - Tutorial
---
_Find out how Parse can be handy in building real-time video chat applications (and more) with this guest post from PubNub by Shyam Purkayastha:_

WebRTC has made a huge impact on what we can do in the browser when it comes to communication and P2P sharing. With no plugins, no frameworks, and no applications to download, WebRTC is seamless for the end-user, and simple for the developer.

A few months ago, we built a [WebRTC video chat application](http://www.pubnub.com/blog/building-a-webrtc-video-and-voice-chat-application/) using PubNub that allows users to call each other and video/text chat over the browser. Thinking of ways to make the initial application even more powerful, our mind went straight to our friends over at Parse to add a layer of security to the app.

Today we're going to show you how we integrated [Parse Core](https://www.parse.com/products/core) to provide authenticated access to users. This will showcase how the PubNub and Parse SDKs complement each other to build a secure web application which can be easily deployed in the cloud without the need for any server-side programming.

**Tutorial Overview**
  
When it comes to chat applications, authentication is a staple feature. Users expect to have this additional layer of security, allowing them to log in and out of the application, and know that the person on the other end of the communication channel is who they say they are. Authentication powered by Parse fit the bill perfectly.

For this demonstration, we'll be adding the following features to the existing WebRTC application:

<ol class="standard-list">
  <li>
    Login page
  </li>
  <li>
    Signup option
  </li>
  <li>
    Logout option
  </li>
</ol>

We'll be using the following SDKs from PubNub and Parse:

<ol class="standard-list">
  <li>
    <a href="https://github.com/stephenlb/webrtc-sdk">PubNub WebRTC Javascript SDK</a>
  </li>
  <li>
    <a href="https://www.parse.com/docs/js_guide">Parse Javascript SDK</a>
  </li>
</ol>

**Login Page**
  
To provide secured access to the WebRTC application, we first need to display a login dialog before the application UI, which will ask the user to login using his or her username and password.

Here's how the Login dialog will look. This will be a modal dialog:
  
<img class="alignnone size-full wp-image-3554" src="{{ site.url }}/assets/wp-content/uploads/2015/05/login_image2.png" alt="Login Image" width="581" height="223" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/login_image2.png 581w, {{ site.url }}/assets/wp-content/uploads/2015/05/login_image2-300x115.png 300w" sizes="(max-width: 581px) 100vw, 581px" />
  
The login dialog will also act as a signup dialog when the user clicks the “Signup” button. In this case, an additional UI element pops up for the signup process.

<img class="alignnone size-full wp-image-3553" src="{{ site.url }}/assets/wp-content/uploads/2015/05/signup_image2.png" alt="Signup Image" width="577" height="259" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/signup_image2.png 577w, {{ site.url }}/assets/wp-content/uploads/2015/05/signup_image2-300x135.png 300w" sizes="(max-width: 577px) 100vw, 577px" />

**Parse Overview**
  
With the HTML structure in place, it's time for some Parse fun. Parse provides APIs for managing common application requirements in the cloud. Typically, every web-enabled application needs certain essential features like user management, authentication, data persistence, and access control. Parse provides the infrastructure for managing these routine chores and more, directly from the client application without needing any server-side programming or infrastructure.

All we need to do is create an account with Parse.com, register an app, and with that, Parse provides the API keys to manage users and data for that app. When the user clicks on the login button, the JavaScript kicks off an API call to Parse Cloud to authenticate the user.

Here's how the login code looks:

<pre class="line-numbers"><code class="language-csharp">//Handler for performing Parse Login Operation
    var performLogin = function(){
        //Step 1 - Set status message
        $('#status-section').html('Attempting to login as user ' + $('#user-text').val());
        //Step 2 - Perform login via Parse
        Parse.User.logIn($('#user-text').val(), $('#password-text').val(), {
            success: function(user) {
                $('#status-section').html('Authentication successful for user ' + user.getUsername() + ' , Loading PubNub WebRTC App');
                initWebRTC();
                $('#auth-overlay').fadeOut(2000);
                setTimeout(function(){
                    $('#auth-overlay').remove();
                },3000);
            },
            error: function(user, error) {
                $('#status-section').html('Authentication failed for user ' + user.getUsername() + ' ' + error.message);
            }
        });
    }</code></pre>

Assuming that the user has entered a valid username and password (with which he/she had signed up earlier), Parse Core will authenticate the user and return a success Promise object to the application. The application code will then disengage the modal dialog box making the WebRTC UI screen appear in the webpage.

All the previous functionality of the PubNub WebRTC application remains the same, except for the addition of the Logout button, as described below in the "Logout Option" section.

**Signup Option**
  
The signup dialog has an additional text field called "Retype Password" which requires the user to type the password again during signup to prevent spurious data entry.

The code for performing signup is similar to the code for login, except for the Parse API call.

<pre class="line-numbers"><code class="language-csharp">var performSignup = function(){
        //Step 1 - Create the Parse User Object
        var user = new Parse.User();
        user.set("username", $('#user-text').val());
        user.set("password", $('#password-text').val());
        //Step 2 - Set status message
        $('#status-section').html('Attempting to signup user ' + $('#user-text').val());
        //Step 3 - Perform signup via Parse
        user.signUp(null, {
            success: function(user) {
                $('#status-section').html('User ' + user.getUsername() + ' signed up successfully , Redirecting to Login page');
                //Logout the just signed up user
                Parse.User.logOut();
                setTimeout(function(){
                    window.location.reload();
                },3000);
            },
            error: function(user, error) {
                $('#status-section').html('Signup failed for User ' + user.getUsername() + ' , ' + ' ' + error.message);
            }
        });
}</code></pre>

**Logout Option**
  
After successful login, the WebRTC UI displays a “Logout” button to allow the user to log out from the application.

<img class="alignnone wp-image-3552 size-full" src="{{ site.url }}/assets/wp-content/uploads/2015/05/logout_image2.png" alt="Logout Image" width="500" height="278" srcset="{{ site.url }}/assets/wp-content/uploads/2015/05/logout_image2.png 500w, {{ site.url }}/assets/wp-content/uploads/2015/05/logout_image2-300x167.png 300w" sizes="(max-width: 500px) 100vw, 500px" />
  
**Conclusion**
  
With this, we now have an improved version of our PubNub WebRTC App with an additional layer of security and the ability to manage access control to the main application features. PubNub takes care of the communication, while Parse takes care of the user management and authentication.

The principles of this tutorial, and authentication powered by Parse, can be extended beyond chat to basically any realtime application where data is sent bidirectionally between two devices, or broadcast from one to many. Live-blogging platforms, multiplayer gaming, E-commerce, and even the Internet of Things all need user management authentication as an added layer of security.

Pairing Parse and PubNub enables you to build both fast _and_ secure realtime web, mobile, and IoT applications. You can access the complete [source code](https://github.com/shyampurk/pubnub-webrtc-parse) here.