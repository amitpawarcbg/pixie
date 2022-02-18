# pixie

Pixie Overview
Pixie is an open source observability tool for Kubernetes applications. Pixie uses eBPF to automatically capture telemetry data without the need for manual instrumentation.

Developers can use Pixie to view the high-level state of their cluster (service maps, cluster resources, application traffic) and also drill-down into more detailed views (pod state, flame graphs, individual full body application requests).

Pixie was contributed by New Relic, Inc. to the Cloud Native Computing Foundation as a sandbox project in June 2021.

**Features**
Auto-telemetry: Pixie uses eBPF to automatically collect telemetry data such as full-body requests, resource and network metrics, application profiles, and more.

In-cluster edge compute: Pixie collects, stores and queries all telemetry data locally in the cluster. Pixie uses less than 5% of cluster CPU, and in most cases less than 2%.

Scriptability: PxL, Pixie’s flexible Pythonic query language, can be used across Pixie’s UI, CLI, and client APIs. Pixie provides a set of community scripts for common use cases.
