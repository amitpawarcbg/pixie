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

**How Pixie uses eBPF**
Pixie uses eBPF to drive much of its data collection. This approach enables Pixie to efficiently collect data in a way that requires no user instrumentation (i.e. no code changes, no redeployments).

Pixie sets up eBPF probes to trigger on a number of kernel or user-space events. Each time a probe triggers, Pixie collects the data of interest from the event. This enables Pixie to automatically collect monitoring data such as various protocol messages.

The following sections further describe how Pixie uses eBPF in some of its key features.

**Protocol Tracing**
One of the main features of Pixie is its ability to automatically trace protocol messages.

When Pixie is deployed to the nodes in your cluster, it deploys eBPF kernel probes (kprobes) that are set up to trigger on Linux syscalls used for networking. Then, when your application makes network-related syscalls -- such as send() and recv() -- Pixie's eBPF probes snoop the data and send it to the Pixie Edge Module (PEM). In the PEM, the data is parsed according the detected protocol and stored for querying.


**Pixie Protocol Tracing with eBPF**
**Tracing TLS/SSL Connections**
Pixie's SSL tracing works in a similar way as its basic protocol tracing, with one main difference: Instead of using eBPF to snoop the data at send() and recv(), eBPF user-space probes (uprobes) are set up directly on the TLS library's API. This enables Pixie to capture the data before it is encrypted. Pixie supports several encryption libraries, including OpenSSL.


Pixie TLS/SSL Protocol Tracing with eBPF
**Application CPU Profiling**
Pixie's continuous profiler uses eBPF to periodically interrupt the CPU. During this process, the eBPF probe inspects the currently running program and collects a stack trace to record where the program was executing.

This approach to CPU profiling is called a sampling-based profiler. By only triggering at a very low frequency (approximately once every 10 ms), the overhead is negligible. The sampling rate, however, is sufficient to identify the applications that are using the CPU the most, and which parts of the code is typically being executed.


**Distributed bpftrace Scripts**
eBPF is also used in Pixie's distributed bpftrace script deployment. In this case, you can write your own custom BPF code, using the bpftrace format. Pixie orchestrates the deployment of the BPF code to all the nodes in your cluster and records the results in a structured way into its data tables.

**Dynamic Logging**
With Pixie's dynamic logging feature (currently supported for Golang), you specify a function in your deployed code that you'd like to inspect. Pixie then automatically generates the eBPF code for tracing the specified arguments and return values of the function. Once deployed, the auto-generated eBPF probe triggers every time the function is called, and the arguments and return values are recorded into a table which can be queried. This enables the application developer to effectively add logs to their running programs without ever recompiling or redeploying!


