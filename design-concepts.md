---
description: Details our design decisions
---

# Design Concepts

### Database

_**Graph Database**_

We narrowed down on Dgraph as the Graph database. Dgraph being open source, NoSQL and providing inbuilt support for running complicated Joins giving results in a JSON format, adds additional value. It helps us store and display complex relationships between all the Kubernetes entities. For additional information on Dgraph, please visit [https://dgraph.io/](https://dgraph.io/)

Dgraph must be deployed on the Cutlass Kubernetes cluster in HA mode. We run 3 Dgraph Zero instances and 3 Dgraph Server instances. Dgraph Zero controls the Dgraph cluster. It automatically moves data between different Dgraph Server instances based on the size of the data served by each Server instance. The Dgraph setup is done as part of the [Installation](installation.md) section.

The Dgraph Schema is loaded as part of [Installation](installation.md) section as well. The "term" index is used to support term matches. The "trigram" index is used to support substring match based on the regex match capability it supports. 

The workload would normally be heavy on the writes and low on the reads depending as the number of events that are received from the cluster would be higher that the number of queries executed against it. For Performance and SLAs please refer to the Section on [SLAs](slas.md)

#### _Controller_

The controller is designed as a Kubernetes custom controller. The controller must reside in the cluster that it is collecting events from.

The controller consists of Informers which List and then Watch for events from the cluster where it runs. The native Kubernetes events cache offered by the client-go library is used, since repeatedly retrieving information from the Kube API server can become expensive. We only cares about events when the object has been created, modified or deleted. When one of these events is triggered on any of the Kubernetes entities which we handle, it is added to the controllers WorkQueue. The controller then picks them up from the WorkQueue, validates them and makes an API call to the Rest Service to store them in the database.

Adding a new controller is a simple process. For details on it, please refer to [Contribute to Cutlass]()

#### _Rest Service_

The Rest service offers several APIs that interact with the underlying database and provide results. The Rest Service must be deployed on the Cutlass Kubernetes cluster. For details on the Rest API please refer to the [Rest APIs](rest-apis.md) section.

It has CRUD APIs for handling entities. These are not externally exposed. Additionally, it has an API for validating entity metadata that comes in from the Controller. And APIs for querying data from the database.

#### _Web Application_

The Web Application has several views offering support for real time queries, graph queries and historical queries.

For novice users, we offer a one term substring query, that provided the added flexilbility for users. Users just provide the word to look for and don't need provide additional detail about the Kubernetes entitiy or attribute they are looking for. Based on the word entered, we provide all the matches and offer the user the option to drill down further to their entity of interest. For details on usage , please refer to the section [Cutlass UI]()

For advanced users, we offer a key, value based search query. The user can search based on attributes and the values they are looking for. For details on usage , please refer to the section [Cutlass UI]()



\_\_



