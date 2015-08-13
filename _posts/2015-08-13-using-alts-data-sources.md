---
layout: post
title: Using Alt's data sources
date: 2015-08-13 17:00
comments: true
categories:
  - alt
  - react
  - flux
  - babel
author: Zach Perrault
---



[Data sources](http://alt.js.org/docs/async/) provide several features around async requests:

* Checking if data to be fetched already exists in the store
* Automatic bindings to actions to handle loading, success, and errors
* Access to store data if needed to build the request
* Loading state which keeps track of all pending requests for a specific store

## Defining a data source

[Data sources](http://alt.js.org/docs/async/) are just plain javascript objects used by Alt to build smart wrappers around asynchronous requests.

A data source can define how to fetch data locally or remotely, and also what actions should be fired when something is loading, loads successfully, or fails to load.

### Retrieving data from the server
{% highlight javascript %}
// src/sources/todo-source.es6
import alt from 'alt-instance'
import api from 'utils/api'

import TodoActions from 'actions/todo-actions'

const TodoSource = {
  fetchById: {
    // `local` is called first and is passed the TodoStore's state
    // and any other params passed to it. if the returned
    // value != null, no request is sent to the server and the
    // TodoStore will emit a change event. Otherwise, `remote` is
    // called.
    local(state, id) {
      return state.todos[id] // will return `undefined` if there
                             // is no todo at that id
    },

    // `remote` is called if the return value of local == undefined
    // It will receive the same parameters as local would but should
    // return a promise.
    remote(state, id) {
      return api.get(`/todos/${id}`, null, null, {_context: {todoId: id}})
    },

    // shouldFetch allows you to force a call to remote even if
    // local returns a value != null. This should only be used if
    // you need more complicated logic around when to fetch and
    // when not to fetch (i.e. fetching if the data in the store
    // was fetched a long time ago.)
    // This is optional.
    shouldFetch(state, id) {
      const todo = state.todos[id]
      // sends request if todo doesn't exist in the store or if
      // the todo was fetched more than five minutes ago.
      // NOTE: This would require setting fetchedAt on the
      // todo object when the todo is fetched initially.
      if (!todo || (Date.now() - todo.fetchedAt  >= 300000)) {
        return true
      }
      return false
    },

    // loading specifies an optional action to fire once `remote`
    // has been called, this can be useful for doing things like
    // clearing out data in the store if it should be replaced once
    // the request completes.
    loading: TodoActions.fetchingTodo,

    // success is required and specifies what action should be
    // called if the promise returned by remote resolves. This
    // action will be passed the data resolved by the promise.
    success: TodoActions.todoFetched,

    // error is required and specifies what action should be
    // called if the promise returned by remote rejects. This
    // action will be passed the data rejected by the promise.
    error: TodoActions.fetchFailed,
  },
}
{% endhighlight %}

### Modifying data on the server

For things like creating new data or modifying existing data, where a request should always be sent to the server, simply omit the `local` function.

{% highlight javascript %}
const TodoSource = {
  createTodo: {
    remote(state, params) {
      api.post('/todos', params)
    },
    // no local(state, params) means this will always send a requse
    success: TodoActions.todoCreated,
    error: TodoActions.todoCreationFailed,
  },
}
{% endhighlight %}

## Handling responses in actions

The actions listed for `success` and `error` will get called with whatever the Promise resolved or rejected. Assuming the use of the trunkclub api module (a wrapper around [axios](https://github.com/mzabriskie/axios)) the parsed response body will reside on the `data` key of the payload.

Actions that are fired by sources are no different from any other [Alt action](actions.md)

{% highlight javascript %}
class TodoActions {
  todoFetched(payload) {
    const todo = payload.data.todo
    this.dispatch(todo)
  }
  ...
{% endhighlight %}

## Registering a data source

The objects used to define a data source are useless on their own. They must be registered on a store.

{% highlight javascript %}
// src/stores/todo-store.es6
import TodoSource from 'sources/todo-source'
class TodoStore {
  constructor() {
    this.state = {
      todos: {},
    }

    this.registerAsync(TodoSource)
  }
  ...
{% endhighlight %}

## Using a data source


Once a source is registered on a store, you can call the functions you defined from the store object.

{% highlight javascript %}
TodoStore.fetchById(this.props.todoId)
{% endhighlight %}

{% highlight javascript %}
TodoStore.createTodo({
  completed: true,
  text: 'Learn about Alt\'s data sources'
})
{% endhighlight %}

Here are some complete components that use the source we defined above.

{% highlight javascript %}
// src/components/todo.es6
import React from 'react'
import connectToStores from 'alt/utils/connectToStores'
import TodoStore from 'stores/todo-store'

const Todo = React.createClass({
  propTypes: {
    todoId: React.PropTypes.number.isRequired,
    todo: React.PropTypes.object,
  },
  statics: {
    getStores() {
      return [TodoStore]
    },
    getPropsFromStores(props) {
      return {
        todo: TodoStore.getById(props.todoId),
      }
    },
  },
  componentDidConnect() {
    TodoStore.fetchById(this.props.todoId)
  },
  render() {
    return (
      <li>
        <input type="checkbox" checked={this.props.todo.completed} />
        {this.props.todo.text}
      </li>
    )
  },
})

export default connectToStores(Todo)
{% endhighlight %}

{% highlight javascript %}
// src/components/todo-input.es6
import React from 'react'
import TodoStore from 'stores/todo-store'

const TodoInput = React.createClass({
  handleSubmit(event) {
    event.preventDefault()
    const text = findDOMNode(this.refs.todoText).value
    TodoStore.createTodo({
      completed: false,
      text,
    })
  },
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <input ref="todoText" type="text" />
      </form>
    )
  },
})

{% endhighlight %}

## Some gotchas

* Source functions cannot be expected to ever return a value
* When the `local` function returns a value, the parent store will emit a change event
  * If you're linked to a store using `connectToStores`, `getPropsFromStores` will get called again.
  * When using `connectToStores`, calls to source functions should go in `componentDidConnect`

---
[peek under the hood...](https://github.com/goatslacker/alt/blob/f7a5290706c786f1d7201dff7d78f432c6bdd5e8/src/alt/store/StoreMixin.js#L26-L85)
