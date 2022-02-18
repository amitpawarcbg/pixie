# pixie

Pixie Overview
Pixie is an open source observability tool for Kubernetes applications. Pixie uses eBPF to automatically capture telemetry data without the need for manual instrumentation.

Developers can use Pixie to view the high-level state of their cluster (service maps, cluster resources, application traffic) and also drill-down into more detailed views (pod state, flame graphs, individual full body application requests).

Pixie was contributed by New Relic, Inc. to the Cloud Native Computing Foundation as a sandbox project in June 2021.

**Features**
Auto-telemetry: Pixie uses eBPF to automatically collect telemetry data such as full-body requests, resource and network metrics, application profiles, and more.

In-cluster edge compute: Pixie collects, stores and queries all telemetry data locally in the cluster. Pixie uses less than 5% of cluster CPU, and in most cases less than 2%.

Scriptability: PxL, Pixie’s flexible Pythonic query language, can be used across Pixie’s UI, CLI, and client APIs. Pixie provides a set of community scripts for common use cases.

**Data Sources**
Pixie uses eBPF to automatically instrument Kubernetes applications.

Pixie ships with a set of default data sources, which can also be extended by the user.

**Data Sources**
Pixie automatically collects the following data:

Protocol traces: Full-body messages between the pods of your applications. Tracing currently supports the following list of protocols. For more information, see the Request Tracing, Service Performance, and Database Query Profiling tutorials.

Resource metrics: CPU, memory and I/O metrics for your pods. For more information, see the Infra Health tutorial.

Network metrics: Network-layer and connection-level RX/TX statistics. For more information, see the Network Monitoring tutorial.

JVM metrics: JVM memory management metrics for Java applications.

Application CPU profiles: Sampled stack traces from your application. Pixie’s continuous profiler is always running to help identify application performance bottlenecks when you need it. Currently supports compiled languages (Go, Rust, C/C++). For more information, see the Continuous Application Profiling tutorial.

Pixie can also be configured by the user to collect dynamic logs from Go application code and to run custom BPFTrace scripts.

**Supported Protocols**
Pixie automatically traces the following protocols:

Protocol	Support	Notes
HTTP	    Supported	
HTTP2/gRPC	Partially Supported	Currently only for Golang apps with debug information.
DNS	      Supported	
NATS	    Supported	Requires a NATS build with debug information.
MySQL	    Supported	
PostgreSQL	Supported	
Cassandra	Supported	
Redis	    Supported	
Kafka	    Supported	
Additional protocols are under development
