---
layout: post
title: Using Alt's Data Sources
date: 2015-08-13 17:00
comments: true
headshot: https://trunkclub-avatars.s3.amazonaws.com/employee/thumb/thumb_34319424a794f9a0c6ed84297c9ebdf7.jpg?02232016104907
categories:
  - alt
  - react
  - flux
  - babel
author: Zach Perrault
---

The Trunk Club tech team recently committed to using [Alt](http://alt.js.org) [for a year](https://twitter.com/peterpme/status/626190340844687361). One of the more interesting features of Alt is [data sources](http://alt.js.org/docs/async).

Data sources provide several features around async requests:

* Checking if data to be fetched already exists in the store
* Automatic bindings to actions to handle loading, success, and errors
* Access to store data if needed to build the request
* Loading state which keeps track of all pending requests for a specific store

## Defining a data source

Data sources are just plain javascript objects used by Alt to build smart wrappers around asynchronous requests.

A data source can define how to fetch data locally or remotely, and also what actions should be fired when something is loading, loads successfully, or fails to load.

### Retrieving data from the server

{% gist zperrault/dc158e5126cbabad2bf0 todo-source.es6 %}

### Modifying data on the server

For things like creating new data or modifying existing data, where a request should always be sent to the server, simply omit the `local` function.

{% gist zperrault/dc158e5126cbabad2bf0 todo-source-post.es6 %}

## Handling responses in actions

The actions listed for `success` and `error` will get called with whatever the Promise resolved or rejected. Assuming the use of [axios](https://github.com/mzabriskie/axios), the parsed response body will reside on the `data` key of the payload.

Actions that are fired by sources are no different from any other [Alt action](http://alt.js.org/docs/actions/)

{% gist zperrault/dc158e5126cbabad2bf0 todo-actions.es6 %}

## Registering a data source

The objects used to define a data source are useless on their own. They must be registered on a store.

{% gist zperrault/dc158e5126cbabad2bf0 todo-store.es6 %}

## Using a data source

Once a source is registered on a store, the functions defined in the source can be called from the store object.

{% gist zperrault/dc158e5126cbabad2bf0 call-example.es6 %}

Additionally, an `isLoading` function is added to the store which returns true if there are any pending requests from the source. This can be used to disable inputs and prevent duplicate requests.

Here are some complete components that use the source defined above.

{% gist zperrault/dc158e5126cbabad2bf0 todo.es6 %}

{% gist zperrault/dc158e5126cbabad2bf0 todo-input.es6 %}

## Some gotchas

Data sources cannot replace traditional getters from your stores. If `remote` gets called, the promise returned from remote is returned from the outer store function. However if `local` returns a value, nothing is returned from the outer function but the parent store will emit a change event. Because of this, it is best to put calls to source functions in `componentDidMount` and call traditional getters in whatever callback is fired when stores emit changes.

The only exception to this is when using [`connectToStores`](https://github.com/goatslacker/alt/blob/master/src/utils/connectToStores.js). `connectToStores` exposes an additional lifecycle function, `componentDidConnect`, which is called when store connection has been made.

---
Alt's source code is fun to read so why don't you [peek under the hood...](https://github.com/goatslacker/alt/blob/f7a5290706c786f1d7201dff7d78f432c6bdd5e8/src/alt/store/StoreMixin.js#L26-L85)

Zach Perrault is a senior Computer Science major at Ohio University and Trunk Club tech intern. If you'd like to share any thoughts with him about this article or anything else, feel free to reach out on twitter: [@zperrault](http://twitter.com/zperrault).
