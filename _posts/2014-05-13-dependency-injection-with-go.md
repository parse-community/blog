---
id: 2327
title: Dependency Injection with Go
date: 2014-05-13T12:00:45+00:00
author: naitikshah
layout: post
guid: http://blog.parse.com/?p=2327
permalink: /learn/engineering/dependency-injection-with-go/
post_format:
  - basic
app_store_link_id:
  - ""
hide_from_index:
  - "0"
dsq_thread_id:
  - "3679500557"
categories:
  - Engineering
  - Ops
tags:
  - go
---
Dependency Injection (DI) is the pattern whereby the dependencies of a component are provided to it and made part of its state. The pattern is used to isolate components from the implementations of their dependencies. <a href="http://golang.org/" target="_blank">Go</a>, with its interfaces, eliminates many of the reasons for a classic DI system (in Java, etc). Our [inject](https://github.com/facebookgo/inject) package provides very little of what you'll find in a system like [Dagger](https://github.com/square/dagger) or [Guice,](https://code.google.com/p/google-guice/) and focuses on eliminating the need to manually allocate instances and wire up the object graph. This is both because many of those aspects are unnecessary, and also because we wanted to make injection simpler to understand in our Go codebase.

Our path to building [inject](https://github.com/facebookgo/inject) went through a few stages:

## Globals

It started with a unanimous, noble goal. We had global connection objects for services like Mongo, Memcache, and some others. Roughly, our code looked like this:

<pre class="brush: c; gutter: false">var MongoService mongo.Service

func InitMongoService(url string) {
  MongoService = ...
}

func GetApp(id uint64) *App {
  a := new(App)
  MongoService.Session().Find(..).One(a)
  return a
}</pre>

Typically the `main()` function would call the various init functions like `InitMongoService` with configuration based on flags/configuration files. At this point, functions like `GetApp` could use the service/connection. Of course we sometimes ran into cases where we forgot to initialize the global and so got into a `nil` pointer panic.

Though in production the globals were shared resources, having them had (at least) two downsides. First, code was harder to ponder because the dependencies of a component were unclear. Second, testing these components was made more difficult, and running tests in parallel was near impossible. While our tests are relatively quick, we wanted to ensure they stay that way, and being able to run them in parallel was an important step in that direction. With global connections, tests that hit the same data in a backend service could not be run in parallel.

## Eliminating Globals

To eliminate globals, we started with a common pattern. Our components now explicitly depended on say, a Mongo service, or a Memcache service. Roughly, our naive example above now looked something like this:

<pre class="brush: c; gutter: false">type AppLoader struct {
  MongoService mongo.Service
}

func (l *AppLoader) Get(id uint64) *App {
  a := new(App)
  l.MongoService.Session().Find(..).One(a)
  return a
}</pre>

Many functions referencing globals now became methods on a struct containing its dependencies.

## New Problems

The globals and functions went away, and instead we got a bunch of new structs that were created in `main()` and passed around. This was great, and it solved the problems we described. But... we had a very verbose looking `main()` now. It started looking like this:

<pre class="brush: c; gutter: false">func main() {
  mongoURL := flag.String(...)
  mongoService := mongo.NewService(mongoURL)
  cacheService := cache.NewService(...)
  appLoader := &AppLoader{
    MongoService: mongoService,
  }
  handlerOne := &HandlerOne{
    AppLoader: appLoader,
  }
  handlerTwo := &HandlerTwo{
    AppLoader:    appLoader,
    CacheService: cacheService,
  }
  rootHandler := &RootHandler{
    HandlerOne: handlerOne,
    HandlerTwo: handlerTwo,
  }
  ...
}</pre>

As we kept going down this path, our `main()` was dominated by a large number of struct literals which did two mundane things: allocating memory, and wiring up the object graph. We have several binaries that share libraries, so often we'd write this boring code more than once. A noticeable problem that kept occurring was that of `nil` pointer panics. We'd forget to pass the `CacheService` to `HandlerTwo` for example, and get a runtime panic. We tried constructor functions, but they started getting a bit out of hand, too, and still required a whole lot of manual nil checking as well as being verbose themselves. Our team was getting annoyed at having to set up the graph manually and making sure it worked correctly. Our tests set up their own object graph since they obviously didn't share code with `main()`, so problems in there were often not caught in tests. Tests also started to get pretty verbose. In short, we had traded one set of problems for another.

## Identifying the Mundane

Several of us had experience with Dependency Injection systems, and none of us would describe it as an experience of pure joy. So, when we first started discussing solving the new problem in terms of a DI system, there was a fair amount of push back.

We decided that, while we needed something along those lines, we needed to ensure that we avoid known complexities and made some ground rules:

<ol class="standard-list">
  <li>
    No code generation. Our development build step was just <code>go install</code>. We did not want to lose that and introduce additional steps. Related to this rule was no file scanning. We didn't want a system that was O(number of files) and wanted to guard against an increase in build times.
  </li>
  <li>
    No subgraphs. The notion of "subgraphs" was discussed to allow for injection to happen on a per-request basis. In short, a subgraph would be necessary to cleanly separate out objects with a "global" lifetime and objects with a "per-request" lifetime, and ensure we wouldn't mix the per-request objects across requests. We decided to just allow injection for "global" lifetime objects because that was our immediate problem.
  </li>
  <li>
    Avoid code execution. DI by nature makes code difficult to follow. We wanted to avoid custom code execution/hooks to make it easier to reason about.
  </li>
</ol>

Based on those rules, our goals became somewhat clear:

<ol class="standard-list">
  <li>
    Inject should allocate objects.
  </li>
  <li>
    Inject should wire up the object graph.
  </li>
  <li>
    Inject should run only once on application startup.
  </li>
</ol>

We've discussed supporting constructor functions, but have avoided adding support for them so far.

## Inject

The inject library is the result of this work and our solution. It uses struct tags to enable injection, allocates memory for concrete types, and supports injection for interface types as long as they're unambiguous. It also has some less often used features like named injection. Roughly, our naive example above now looks something like this:

<pre class="brush: c; gutter: false">type AppLoader struct {
  MongoService mongo.Service `inject:""`
}

func (l *AppLoader) Get(id uint64) *App {
  a := new(App)
  l.MongoService.Session().Find(..).One(a)
  return a
}</pre>

Nothing has changed here besides the addition of the inject tag on the `MongoService` field. There are a few different ways to utilize that tag, but this is the most common use and simply indicates a shared `mongo.Service` instance is expected. Similarly imagine our `HandlerOne`, `HandlerTwo` & `RootHandler` have inject tags on their fields.

The fun part is that our `main()` now looks like this:

<pre class="brush: c; gutter: false">func main() {
  mongoURL := flag.String(...)
  mongoService := mongo.NewService(mongoURL)
  cacheService := cache.NewService(...)
  var app RootHandler
  err := inject.Populate(mongoService, cacheService, &app)
  if err != nil {
    panic(err)
  }
  ...
}</pre>

Much shorter! Inject roughly goes through a process like this:

<ol class="standard-list">
  <li>
    Looks at each provided instance, eventually comes across the <code>app</code> instance of the <code>RootHandler</code> type.
  </li>
  <li>
    Looks at the fields of <code>RootHandler</code>, and sees <code>*HandlerOne</code> with the <code>inject</code> tag. It doesn't find an existing instance for <code>*HandlerOne</code>, so it creates one, and assigns it to the field.
  </li>
  <li>
    Goes through a similar process for the <code>HandlerOne</code> instance it just created. Finds the <code>AppLoader</code> field, similarly creates it.
  </li>
  <li>
    For the <code>AppLoader</code> instance, which requires the <code>mongo.Service</code> instance, it finds that we seeded it with an instance when we called <code>Populate</code>. It assigns it here.
  </li>
  <li>
    When it goes through the same process for <code>HandlerTwo</code>, it uses the <code>AppLoader</code> instance it created, so the two handlers share the instance.
  </li>
</ol>

Inject allocated the objects and wired up the graph for us. After that call to `Populate`, inject is no longer doing anything, and the rest of the application behaves the same as it did before.

## The Win

We got our more manageable `main()` back. We now manually create instances for only two cases: if the instance needs configuration from main, or if it is required for an interface type. Even then, we typically create partial instances and let inject complete them for us. Test code also became considerably smaller, and providing test implementations no longer requires knowing the object graph. This made tests more resilient to changes far away. Refactoring also became easier as pulling out logic did not require manually tweaking the object graphs being created in various `main()` functions we have.

Overall we're quite happy with the results and how our codebase has evolved since the introduction of inject.

## Resources

You can find the source for the library on Github:

<p style="padding-left: 30px;">
  <a href="https://github.com/facebookgo/inject">https://github.com/facebookgo/inject</a>
</p>

We've also documented it, though playing with it is the best way to learn:

<p style="padding-left: 30px;">
  <a href="https://godoc.org/github.com/facebookgo/inject">https://godoc.org/github.com/facebookgo/inject</a>
</p>

We love to get contributions, too! Just make sure the tests pass:

<p style="padding-left: 30px;">
  <a href="https://travis-ci.org/facebookgo/inject">https://travis-ci.org/facebookgo/inject</a>
</p>