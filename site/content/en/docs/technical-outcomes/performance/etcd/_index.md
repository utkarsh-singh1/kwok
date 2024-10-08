---
title: "Optimizing etcd Performance"
---

# Optimizing etcd performance

Some applications rely on the etcd component for distributed key-value storage. Since [etcd](https://etcd.io) is present in both
KWOK and non-KWOK clusters, various scenarios can be simulated to test etcd performance at scale.
[Kyverno](https://github.com/kyverno/kyverno), for example, is a practice of this.

## Leveraging KWOK for scalable etcd performance testing

The need for performance testing arose when Kyverno users faced issues with excessive admission reports generated by
the [Kubernetes admission controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) that
overwhelmed the etcd database, which stores these configuration data.

To predict the size a production cluster may consume, they advised users to conduct performance evaluations using KWOK in their cluster.
This setup involves creating fake virtual nodes and deployments with KWOK to host workloads and policies. Kyverno, along with other monitoring
tools like the metrics server, are also installed in the cluster to evaluate the impact of the policies on etcd storage.

Key steps in the testing process include:

- **Simulating a large cluster:** KWOK was installed inside a K3d-based cluster, simulating hundreds of nodes and deployments with little resource consumption.
- **etcd stress testing:** The hundreds of nodes and deployments generated many policy reports by the Kubernetes admission 
  controllers. This allowed for an in-depth analysis of Kyverno’s performance in terms of etcd storage usage.
- **Metrics collection:** Prometheus and other monitoring tools were integrated to gather data on etcd resource usage,
  enabling the team to evaluate the efficiency of Kyverno's operations.

You can find details on the specific metrics analyzed during the Kyverno tests, such as memory usage, CPU usage, and etcd
storage size, in the [scaling Kyverno documentation](https://kyverno.io/docs/installation/scaling/)

## Reference

- [How Kyverno uses KWOK for performance testing](https://github.com/kyverno/kyverno/tree/main/docs/perf-testing)
