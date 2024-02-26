# A Practical Introduction to OpenTelemetry Tracing for Developers

In the old days: monitoring, people looking at screens, alerting => no automation, but not doable anymore because systems are distributed.

Observability: ability to collect data.
3 pillars:
- Metrics
- Logging
- Tracing

Metric:
- System: CPU, Memory
- Higher level: req/s, HTTP status code, orders/s

Logging:
- What to log?
- Logging format?
- Where to log
- Logs aggregation

Centralized logging systems:
- Get the log: scrape vs send
- Parse: structured vs unstructured

Tracing: set of technique and tools to follow a request through multiple systems.
Zipkin, OpenTracing, Jaeger

W3C Trace Context
- Trace: follows path of a request in multiple components
- Span: bound to a single component, linked to another span in a parent-child relationship

OpenTelemetry defines format & protocols. Send OTel data to a collector.

2 approaches depending on the stack
- Auto instrumentation via runtime (eg JVM)
    - Low-hanging fruit
    - No coupling
- Manual instrumentation (library dependency + API)

Entrypoint generates the first req ID
Apache APISIX

Manual instrumentation: added annotations to functions eg `@WithSpan(name)` and `@SpanAttribute(name)`

Async instrumentation: filter requests, connect to queue and send the message to queue

Demo repository: https://github.com/nfrankel/opentelemetry-tracing