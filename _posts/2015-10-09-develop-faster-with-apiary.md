---
layout: post
title: ! 'Develop faster with Apiary'
author: Jack Blandin
categories:
- javascript
---

When building a front-end application in a microservice architecture I've often found myself waiting for other developers to complete the back-end so that I know what endpoints I have to work with. Recently, I've been using [Apiary](https://apiary.io/) to reduce this waiting time.

### Apiary
Apiary is a tool used for designing, documenting, and testing APIs. In addition, it has a tool called a _mock server_ which is described on their [website](https://apiary.io/) as

> What wireframes are for UI design, a server mock is for API design

The mock-server allows the developer to stub out a payload to be returned after a specific endpoint is requested. This allows front-end applications to get back real data even though the back-end is not yet setup.

### API Blueprint

Let's say, for example, that we have two teams working on a blog-posting app. If both teams can agree on an API, then we can create a contract using Apiary's blueprint tool using [MSON markdown](https://docs.apiary.io/api_101/mson-tutorial/). Here is a sample blueprint for retrieving all blog posts:

    ## Group Blog Posts
    ## Blog Post Collection [/blog/posts]
    ### List All Blog Posts [GET]

    + Response 200 (application/json)
    [
      {
        "author": "John Smith",
        "date": "2015-10-4T08:40:51.620Z",
        "message": "Hello"
      },
      {
        "author": "John Smith",
        "date": "2015-10-5T08:40:51.620Z",
        "message": "World"
      }
    ]


### Mock-server

Now that the contract is made, it allows us to utilize the mock server. We can do this simply by changing the `API_BASE_URI` in our front-end app to the mock-server's url (obtained when setting up a new Apiary project) instead of our back-end server's url.

    // TODO: uncomment the follwing line once back-end is setup
    // const API_BASE_URL = 'http://example-back-end-url.com'

    const API_BASE_URL = 'http://example-apiary-private-url.com'

    function fetchBlogPosts() {
      xhr.get(`${API_BASE_URL}/blog/posts`)
    }

Now when we call `fetchBlogPosts()`,

    let blogPosts = null

    fetchBlogPosts().then(
      (data) => {
        blogPosts = data.payload
      },
      (error) => {
      throw error;
      })
    }

we get back the payload that we configured in Apiary:

    blogPosts: [
      {
        "author": "John Smith",
        "date": "2015-10-4T08:40:51.620Z",
        "message": "Hello"
      },
      {
        "author": "John Smith",
        "date": "2015-10-5T08:40:51.620Z",
        "message": "World"
      }
    ]

### How we've used it at Trunk Club
My team and I used this feature recently while developing a new React application. There were a few endpoints that did not exist yet in the service level, but we thought it was more important to get a rough draft of our application before setting up those endpoints. Our solution was to stub out these endpoints using the above strategy, and create the endpoints in the service-layer afterward. This allowed us a lot of flexibility with negotiating a contract between the front and back end.
