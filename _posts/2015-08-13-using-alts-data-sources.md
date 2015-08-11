---
layout: post
title: Alt Data Sources By Example
author: Zach Perrault
date: 2015-08-13 12:00:00
comments: true
---


{% highlight javascript %}
// src/alt-instance.es6
import Alt from 'alt'
export default new Alt()
{% endhighlight %}

{% highlight javascript %}
// src/actions/data-actions.es6
import alt from 'alt-instance'

class DataActions {
  datumLoaded(payload) {
    this.dispatch(payload.data.datum)
  }

  failedToLoadDatum(payload) {
    this.dispatch(payload)
  }
}

export default alt.createActions(DataActions)
{% endhighlight %}

{% highlight javascript %}
// src/sources/data-source.es6
import axios from 'axios'
import DataActions from 'actions/data-actions'

const DataSource = {
  fetch: {
    local(state, id) {
      return state.data[id]
    },

    remote(state, id) {
      return axios.get(`/api/data/${id}`)
    },

    success: DataActions.datumLoaded,
    error: DataActions.failedToLoadDatum,
  },
}

export default DataSource
{% endhighlight %}

{% highlight javascript %}
// src/stores/data-store.es6
import alt from 'alt-instance'
import React from 'react'
import DataActions from 'actions/data-actions'
import DataSource from 'sources/data-source'

class DataStore {
  constructor() {
    this.state = {
      data: {}
    }

    this.bindListeners({
      onDatumLoaded: DataActions.DATUM_LOADED,
      onDatumFailedToLoaded: DataActions.DATUM_FAILED_TO_LOAD,
    })

    this.registerAsync(DataSource)
  }

  onDatumLoaded(datum) {
    this.setState((prevState) => {
      return React.addons.update(prevState, {
        data: {
          [datum.id]: {
            $set: datum,
          },
        },
      })
    })
  }

  onDatumFailedToLoaded(error) {
    console.error(error)
  }
}
{% endhighlight %}

{% highlight javascript %}
// src/components/data-display.es6
// <DataDisplay datumId={12} />
import React from 'react'
import connectToStores from 'alt/utils/connectToStores'

import DataStore from 'stores/data-store'

const DataDisplay = React.createClass({
  propTypes: {
    datumId: React.PropTypes.number.isRequired,
    datum: React.PropTypes.object,
  },

  statics: {
    getStores() {
      return [DataStore]
    },
    getPropsFromStores(props) {
      return DataStore.getState().data[props.datumId]
    },
  },

  componentDidConnect() {
    DataStore.fetch(this.props.datumId)
  },

  render() {
    return (
      <pre>
        {JSON.stringify(this.props.datum)}
      </pre>
    )
  },
})

export default connectToStores(DataDisplay)
{% endhighlight %}
