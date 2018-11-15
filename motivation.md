---
description: What prompted us to develop Cutlass ?
---

# Motivation

In a Kubernetes ecosystem, with multiple clusters deployed, it is not only time consuming, but tedious for operators/developers to debug issues. Application deployment data are stored in various dispersed systems and difficult to navigate, audit and diagnose. Cutlass solves this by providing a holistic view and query interface.

Additionally, there is no graphical view of the topology, making it hard to visualize the system. Cutlass solves this by providing a near real-time graphical view of the system.

If your use case falls under the below or you have similar requirements, please check out the upcoming Sections.

**User Cases**

* Help with application diagnosing and troubleshooting

User can narrow down the list of possible suspect by checking Load balancer, node server, network device etc which has relationship with service when issue happen. User can query application resources by various criteria and relationship

What is the application deployment that has pod IP x.x.x.x?

Pods backing a dns domain [x.x.intuit.com](http://x.x.intuit.com/) ?

* Improved, reconciled data and usage insights for application compliance audits

For example, user can leverage data to check if application deployment meets minimum DR requirement, running on 443 HTTPS port etc across multiple k8s clusters.

* Support application deployment configuration

Store deployment metadata and spec used by orchestration engine to drive application to the desire state.







