---
layout: post
title: Writing Better Cloud Code with Parse Server Example
date: 2021-02-17 15:00 +0100
comments: true
author: dblythy
categories: [Learn,JavaScript,GitHub,Tutorial]
---

Parse Server is a great, quick way to create an app backend without requiring years of knowledge and time.

There are a few additional steps you can do to ensure that your code is the best it can be, and be assured that your Parse Server always runs as smoothly as possible, even as your Cloud Code continues to grow.

<!-- more -->

Today, I'm going to introduce you to the newest features in the [Parse Server Example](https://github.com/parse-community/parse-server-example) project, which will help you write better code, specifically linting, testing, and coverage.

# Linting

Having well written code can save you a lot of time in the long run. There is nothing worse than looking back at old code you've written and needing to spend hours to wrap your head around it.

There have been _many_ improvements to JavaScript since the initial release of Parse Server. Here are a few of them:

## Using const or let instead of var

In previous versions of our Parse javascript docs, we had many examples of javascript with `var`.

Using `var` can result in variables becoming accidently overridden, causing unwanted bugs, such as:

```javascript
Parse.Cloud.beforeSave('test', function(request) {
  var object = request.object;
  var user = request.user;
  var query = new Parse.Query('Object');
  if (user.get('object')) {
    // accidently redefine 'object' variable
    var object = await query.first({useMasterKey:true});
    user.set('object',object);
  }
  // you might expect object to be request.object here, but now it is the object from the query
});
```
As of ES6, we have evolved all our example code away from var.

```javascript
Parse.Cloud.beforeSave('test', function(request) {
  const object = request.object;
  const user = request.user;
  const query = new Parse.Query('Object');
  if (user.get('object')) {
    // if you accidently redefine 'object' variable, you will get a syntax error using const.
    const secondObject = await query.first({useMasterKey:true});
    user.set('object',secondObject);
  }
  // object will always be request.object
});
```

## Object Literal Shorthand Syntax

With ES6, you can declare variables much clearer using shorthand syntax.

ES5:

```
Parse.Cloud.beforeSave('test', function(request) {
  var object = request.object;
  var user = request.user;
  request.log.info(object, user);
});
```

ES6 shorthand syntax:

```
Parse.Cloud.beforeSave('test', ({object,user,log}) => {
  log.info(object, user);
});
```

## For of loops

With ES6, you can write loops using `for (const object of objects)` rather than `for (var i=0; i<objects.length; i++)`

## Promise.all

Unless your code requires series excecution (i.e one task after an other), you should try to leverage `Promise.all` in your Cloud Functions. This will allow your Cloud Code to resolve much faster.

Poorly Optimized:

```javascript
Parse.Cloud.define('testFunction', ({user, params:{name, type}}) => {
  const nameQuery = new Parse.Query('Name');
  nameQuery.equalTo('name', name);
  const nameData = await nameQuery.first({useMasterKey:true});
  if (!nameData) {
    throw new Parse.Error(141, 'name does not exist');
  }
  const typeQuery = new Parse.Query('Type');
  typeQuery.equalTo('type',type);
  const typeData = await typeQuery.first({useMasterKey:true});
  if (!typeData) {
    throw new Parse.Error(141, 'type does not exist');
  }
  user.set('type', typeData);
  user.set('name', nameData);
  await user.save(null, {useMasterKey: true});
  return 'Type and name are valid'
},{
  requireUser:true,
  fields: ['name','type']
});
```

This code is poorly optimized as it resolves the query for `name`, and then the query for `type`. These should be resolved in parallel as they are not dependant on eachother.

Using Promise.all:
```javascript
Parse.Cloud.define('testFunction', ({user, params:{name, type}}) => {
  const nameQuery = new Parse.Query('Name');
  nameQuery.equalTo('name', name);
  const typeQuery = new Parse.Query('Type');
  typeQuery.equalTo('type',type);
  const [nameData, typeData] = await Promise.all([nameQuery.first({useMasterKey:true}),typeQuery.first({useMasterKey:true})]);
  if (!nameData) {
    throw new Parse.Error(141, 'name does not exist');
  }
  if (!typeData) {
    throw new Parse.Error(141, 'type does not exist');
  }
  user.set('type', type);
  user.set('name', name);
  await user.save(null, {useMasterKey: true});
  return 'Type and name are valid'
},{
  requireUser:true,
  fields: ['name','type']
});
```

These are only a few examples of how to write better Cloud Code. You can learn more about new ES6, ES7, ES8, and other ES release features [here](https://www.w3schools.com/js/js_es6.asp).

On the latest [Parse Server Example](https://github.com/parse-community/parse-server-example) project, you can now run `npm run prettier` to style your code, followed by `npm run lint` , which will run over your Cloud Code and check for quality issues. You can also run `npm run lint-fix` to fix any easy errors, such as indentation.

# Testing

As your Parse Server project gets larger and more complex, it can be challenging to add new features without previous ones becoming affected.

Automated testing is a good way to be assured that your functions are running as expected.

On the latest [Parse Server Example](https://github.com/parse-community/parse-server-example) project, you can add tests by creating `*.spec.js` files in `/spec`.

This will boot up a mock database, start a Parse Server based off your `index.js`. You don't have to worry about tests impacting your production database.

We have written some example tests to show you how it works.

Now, run `npm run test` and `jasmine` ([the testing framework](https://jasmine.github.io/)) will check to make sure the tests run as expected.

Some helpful tips:
- If you create a test using `fit()` or `fdescribe()`, jasmine will focus on those tests only, and others will be ignored.
- You can specify a file to test by using `npm run test /spec/testFile.spec.js`
- You won't be able to access any live data through the tests. You'll have to save mock data using `beforeAll()` if you want to test queries.

# Coverage

Now you've learnt testing, it's important to make sure as much of your Cloud Functions are covered by tests, so you can be sure that they are all running as expected.

After you've written and tested a few tests, you can run `npm run coverage`. This will create a new folder `/coverage`. Navigate to `/coverage/lcov-report` and open `index.html`, and you'll be able to visually see what bits of your code you need to write tests for, and which sections are covered. The more of your Cloud Code you cover, the more you can be assured that changes don't have any unforeseen consequences.

# Watching Cloud Code

Traditionally, you have to restart your Parse Server whenever you make changes to Cloud Code. Now, you can type `npm run watch`, and your Parse Server will auto-restart when any affected files are changed.

# New Scripts

* `npm run watch` will start your Parse Server and restart if you make any changes.
* `npm run lint` will check the linting of your Cloud Code, tests and `index.js`, as defined in `.eslintrc.json`.
* `npm run lint-fix` will attempt to fix the linting of your Cloud Code, tests and `index.js`.
* `npm run prettier` will help improve the formatting and layout of your Cloud Code, tests and `index.js`, as defined in `.prettierrc`.
* `npm run test` will run any tests that are written in `/spec`.
* `npm run coverage` will run tests and check coverage. Output is available in the `/coverage` folder.

# Tying this all together

Great, now you can be assured that your Parse Server's Cloud Code is working as expected, and is clean as it can be.

If you want to go one step further, you can use [GitHub Actions](https://github.com/features/actions) or another CI platform to check your codes' linting and tests prior to commit.

You'll have to create a clone the Parse Server example repo though - we recommend creating it privately so that your data is protected.

The script that GitHub uses to check your Parse Server is located at `.github/workflows/ci.yml`. You can also create other commands here, such as packaging up and deploying to your production server. Be sure to use GitHub secrets for any sensitive keys!

Thanks for reading, we hope this helps you build better Parse Servers!
