---
layout: post
title: Monitoring Distributed System Health with Ruby, Splunk, and Honeybadger
date: 2015-08-21 10:45
comments: true
headshot: https://trunkclub-avatars.s3.amazonaws.com/employee/thumb/thumb_072d3e4369a17dda5831031c1adfd9f7.jpg?03112016150216
author: Jeff Meyers
---
At Trunk Club, we care a lot about the stability and uptime of our platform.
Experimenting with different solutions to effectively monitor everything going
on in a distributed system is tough (but rewarding!) and we\'re trying to
improve all the time.

<!--more-->
We have a lot of metrics that can easily be measured by network traffic -
anything that happens synchronously is generally easy to diagnose. If an
HTTP call fails for some reason - be it
[network partitioning](https://en.wikipedia.org/wiki/Network_partition), an
error in the receiving service, or something totally different - we are alerted
in Honeybadger. Anything mission-critical we run on
[Celluloid](https://celluloid.io/) workers via [Sidekiq](http://sidekiq.org/)
so we have some insight into failures and tools to retry.

But for health metrics that aren’t baked into HTTP requests - things like
database records that are not explicitly exposed as a CRUD layer - how do you
keep track of those?

Here’s what we came up with.

All of our (40+) services can optionally implement a `/health` endpoint. That
health endpoint returns a payload of multiple health metrics, each of which has
a name and value. We have a `health_worker`, which is just a loop running in
Ruby:


{% highlight ruby %}
require \'bundler\'
Bundler.setup(:default)
require \'httparty\'
require \'honeybadger\'

HEALTH_ENDPOINTS = {
  \'foo\' => \'bar/health\'
}

CHECK_EVERY_SECONDS = ENV.fetch(\'CHECK_EVERY_SECONDS\').to_i
API_URL = ENV.fetch(\'API_URL\')

loop do
  HEALTH_ENDPOINTS.each do |service_name, endpoint|
    full_path = "#{API_URL}/#{endpoint}"
    response = HTTParty.get(full_path)

    if response.code != 200
      Honeybadger.notify({
        error_class: \'HealthEndpointUnavailable\',
        error_message: "Health endpoint at #{full_path} did not return 200.",
        context: { path: full_path }
      })
      next
    end

    metrics = response.fetch(\'metrics\', [])
    metrics.each do |metric|
      name = metric.fetch(\'name\', \'\')
      value = metric.fetch(\'value\', \'\')
      payload = {
        service: service_name,
        metric: name,
        value: value
      }
      log(\'measurement\', payload)
    end
  end

  sleep CHECK_EVERY_SECONDS
end
{% endhighlight %}

Every 60 seconds, the worker grabs the metric payloads from every registered
service and logs them. Those logs get collected into Splunk where we can
generate graphs of the data to see the heartbeat of Trunk Club technology. As
an added bonus, any time a `/health` endpoint is unavailable, our worker
complains to Honeybadger, so we have extra monitoring for uptime.

Reliability is important, but this is just one of the many approaches towards a
solution. Let us know how you\'ve tried to solve this issue in the comments.
