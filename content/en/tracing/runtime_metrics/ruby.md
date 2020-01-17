---
title: Ruby Runtime Metrics
kind: documentation
further_reading:
- link: "tracing/connect_logs_and_traces"
  text: "Connect your Logs and Traces together"
- link: "tracing/manual_instrumentation"
  text: "Manually instrument your application to create traces."
- link: "tracing/opentracing"
  text: "Implement Opentracing across your applications."
- link: "tracing/visualization/"
  text: "Explore your services, resources, and traces"
---

<div class="alert alert-warning">
This feature is currently in private beta. <a href="https://docs.datadoghq.com/help/">Reach out to support</a> to turn on this feature for your account.
</div>

## Automatic Configuration

Runtime metrics collection uses the [`dogstatsd-ruby`][1] gem to send metrics via DogStatsD to the Agent. To collect runtime metrics, you must add this gem to your Ruby application, and make sure that [DogStatsD is enabled for the Agent][2].

Metrics collection is disabled by default. You can enable it by setting the `DD_RUNTIME_METRICS_ENABLED` environment variable to `true`, or by setting the following configuration in your Ruby application:

```ruby
# config/initializers/datadog.rb
require 'datadog/statsd'
require 'ddtrace'

Datadog.configure do |c|
  # To enable runtime metrics collection, set `true`. Defaults to `false`
  # You can also set DD_RUNTIME_METRICS_ENABLED=true to configure this.
  c.runtime_metrics_enabled = true

  # Optionally, you can configure the DogStatsD instance used for sending runtime metrics.
  # DogStatsD is automatically configured with default settings if `dogstatsd-ruby` is available.
  # You can configure with host and port of Datadog agent; defaults to 'localhost:8125'.
  c.runtime_metrics statsd: Datadog::Statsd.new
end
```

Runtime metrics can be viewed in correlation with your Ruby services. See the [Service page][3] in Datadog.

By default, runtime metrics from your application are sent to the Datadog Agent thanks to DogStatsD over port `8125`. Make sure that [DogStatsD is enabled for the Agent][2].
If you are running the Agent as a container, ensure that `DD_DOGSTATSD_NON_LOCAL_TRAFFIC` [is set to true][4], and that port `8125` is open on the Agent.
In Kubernetes, [bind the DogstatsD port to a host port][5]; in ECS, [set the appropriate flags in your task definition][6].


## Data Collected

The following metrics are collected by default after enabling Runtime metrics.

{{< get-metrics-from-git "ruby" >}}

Along with displaying these metrics in your APM Service Page, Datadog provides a [default Ruby Runtime Dashboard][7] with the `service` and `runtime-id` tags that are applied to these metrics.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}
[1]: https://rubygems.org/gems/dogstatsd-ruby
[2]: /developers/metrics/dogstatsd_metrics_submission/#setup
[3]: https://app.datadoghq.com/apm/service
[4]: /agent/docker/#dogstatsd-custom-metrics
[5]: /agent/kubernetes/dogstatsd/#bind-the-dogstatsd-port-to-a-host-port
[6]: /integrations/amazon_ecs/?tab=python#create-an-ecs-task
[7]: https://app.datadoghq.com/dash/integration/30268/ruby-runtime-metrics
