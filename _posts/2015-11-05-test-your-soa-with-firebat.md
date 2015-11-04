---
layout: post
title: ! 'Test your SOA with Firebat'
author: Jeff Meyers
categories:
- SOA
- ruby
- OSS
---

Service-oriented architectures are notoriously difficult to test effectively.
Unit tests only cover an individual service, and if responses are stubbed and
that endpoint's contract changes, your on-call engineers aren't going to be
too thrilled.

So how can you write tests across your services that give you confidence to
deploy widespread changes? With [Firebat](https://github.com/trunkclub/firebat),
you can write flexible smoke tests to find problems in your workflows
early.

There are three main components of testing with Firebat:
* Service
* Flow
* Process

A service is a class that defines the contracts available for a particular
resource. This may be a shopping cart, an identity provider, or any one of your
services.

A flow is a set of service calls. Flows need not be service-specific. They can
span many services, but they define a specific workflow. Take, for instance,
what happens when a user selects a product to add to their cart, or bag, or
order. What happens behind the scenes? Likely the product is validated, maybe
cart information is validated, the availability of the product is verified,
and a reservation may be placed, depending on how the shopping experience is
orchestrated. A flow could run each of these steps sequentially and show you
what the output is.

A process is a set of flows. A process may be something like "shop an order,
ship it, then return it completely." This leverages multiple flows and chains
together the outputs of those flows to simulate user actions across multiple
systems.

These 3 concepts have helped us at Trunk Club to write out a suite of smoke
tests across our services, and to start catching problems before deploys.
Thorough documentation and example are available on the
[Github](https://github.com/trunkclub/firebat) page. We hope this helps you
wrangle your company's SOA smoke testing, and encourage pull requests and
feedback.

_Interested in what you've read on the Trunk Club blog? Want to join the best-
dressed tech team in Chicago? Check out our
[Engineering Page](https://www.trunkclub.com/engineering) to see open positions
and learn more._

_P.S. Are you super interested in testing SOA architectures effectively?
Join our Engineers in Test - [Engineering Careers](https://www.trunkclub.com/careers)_
