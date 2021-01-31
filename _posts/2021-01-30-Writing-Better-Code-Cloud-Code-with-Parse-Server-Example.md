---
layout: post
title: Writing Better Cloud Code with Parse Server Example
date: 2021-01-30 15:00 +0100
comments: true
author: dblythy
categories: [Learn,JavaScript,GitHub]
---

Parse Server is a great, quick way to create an app backend without requiring years of knowledge and time.

There are a few additional steps you can do to ensure that your code is the best it can be, and be assured that your Parse Server always runs as smoothly as possible, even as your Cloud Code continues to grow.

<!-- more -->

Today, I'm going to introduce you to the newest features in the [Parse Server Example](https://github.com/parse-community/parse-server-example) project, which will help you write better code, specifically linting, testing, and coverage.

# Linting

Having well written code can save you a lot of time in the long run. There is nothing worse than looking back at old code you've written and needing to spend hours to wrap your head around it. 

I would recommend writing code as cleanly and simply as you can, making use of:

## Using const or let instead of var

In previous versions of our Parse javascript docs, we had many examples of javascript with `var`.

Using `var` can result in variables becoming accidently overridden, causing unwanted bugs, such as:

```javascript
Parse.Cloud.beforeSave('function', function(request) {
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
Parse.Cloud.beforeSave('function', function(request) {
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
  console.log(object, user);
});
```

ES6 shorthand syntax:

```
Parse.Cloud.beforeSave('function', ({object,user}) => {
  console.log(object, user);
});
```

## For of loops

With ES6, you can write loops using `for (const object of objects)` rather than `for (var i=0; i<objects.length; i++)`

## Promise.all

Unless your code requires series excecution (e.g one task after an other), you should try to leverage `Promise.all` in your cloud functions. This will allow your cloud code to resolve much faster.

Poorly Optimized:

```javascript
Parse.Cloud.define('function', ({user, params:{name, type}}) => {
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
Parse.Cloud.define('function', ({user, params:{name, type}}) => {
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

These are only a few examples of how to write better cloud code. On the latest [Parse Server Example](https://github.com/parse-community/parse-server-example) project, you can now run `npm run lint` , which will run over your cloud code and check for any mistakes. You can also run `npm run lint-fix` to fix any easy errors, such as indentation.

# Testing

As your Parse Server project gets larger and more complex, it can be challenging to add new features without previous ones becoming affected.

Automated testing is a good way to be assured that your functions are running as expected.

On the latest [Parse Server Example](https://github.com/parse-community/parse-server-example) project, you can create tests by creating `*.spec.js` files in `/spec`. 

This will boot up a mock database, start a Parse Server based off your `index.js`. You don't have to worry about tests impacting your database.

We have written some example tests to show you how it works.

Now, run `npm run test` and `jasmine` (the testing framework) will check to make sure the tests run as expected. 

Some helpful tips:
- If you create a test using `fit()` or `fdescribe()`, jasmine will focus on those tests only, and others will be ignored.
- You can specify a file to test by using `npm run test /spec/testFile.spec.js`
- You won't be able to access any live data through the tests. You'll have to save mock data using `beforeAll()` if you want to test queries.

# Coverage

Now you've learnt testing, it's important to make sure as much of your cloud functions are covered by tests, so you can be sure that they are all running as expected.

After you've written and tested a few tests, you can run `npm run coverage`. This will create a new folder `/coverage`. Navigate to `/coverage/lcov-report` and open `index.html`, and you'll be able to visually see what bits of your code you need to write tests for, and which sections are covered.

# Tying this all together

Great, now you can be assured that your Parse Server's code is working as expected, and is clean as it can be.

If you want to go one step further, you can use GitHub actions to check your servers linting and tests prior to commit.

You'll have to create a fork of the Parse Server example repo though - we recommend creating it privately so that your data is protected.

The script that GitHub uses to check your Parse Server is located at `.github/workflows/ci.yml`. You can also create other commands here, such as packaging up and deploying to your production server. Be sure to use GitHub secrets for any sensitive keys!

Thanks for reading, we hope this helps you build better Parse Servers!