---
description: What prompted us to develop K-Atlas ?
---

# Motivation

In a Kubernetes ecosystem, with multiple clusters deployed, it is not only time consuming, but tedious for operators/developers to debug issues. Application deployment data are stored in various dispersed systems and difficult to navigate, audit and diagnose. K-Atlas solves this by providing a holistic view and query interface.

Additionally, there is no graphical view of the topology, making it hard to visualize the system. K-Atlas solves this by providing a near real-time graphical view of the system.

If your use case falls under the below or you have similar requirements, please check out the upcoming Sections.

**User Cases**

* Help with application diagnosing and troubleshooting

User can narrow down the list of possible suspects by checking Load balancer, node server, network device etc which has relationship with service when the issue happened. User can query application resources by various criteria and relationship.

What is the application deployment that has pod IP x.x.x.x?

Pods backing a dns domain `service.domain.com` ?

* Improved, reconciled data and usage insights for application compliance audits

User can leverage the collected data to check if an application deployment meets the minimum DR requirements. For example, if it is running on HTTPS port 443, across multiple k8s clusters.

* Support application deployment configurations

Store the deployment metadata and k8s spec to drive an application to the desired state.







