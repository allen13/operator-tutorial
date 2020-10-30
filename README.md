kubernetes operator tutorial
----------------------------

Topics

* Kubernetes Operators
* Prometheus
* Alertmanager
* Prometheus Operator
* Cluster Monitoring Operator
* Create a PrometheusRule
* Operator Lifecycle Manager
* Operator SDK


goal
----

The goal of this tutorial is to explore operators by observing a real world operator in action. By the end of this tutorial you should be able to understand all the steps that take place when a PrometheusRule gets created.

kubernetes operators
--------------------

The Operator pattern aims to capture the key aim of a human operator who is managing a service or set of services. Human operators who look after specific applications and services have deep knowledge of how the system ought to behave, how to deploy it, and how to react if there are problems.

People who run workloads on Kubernetes often like to use automation to take care of repeatable tasks. The Operator pattern captures how you can write code to automate a task beyond what Kubernetes itself provides.

Links:

* [Overview](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)
* [Pros](https://thenewstack.io/why-kubernetes-operators-will-unleash-your-developers-by-reducing-complexity/)
* [Cons](https://thenewstack.io/kubernetes-when-to-use-and-when-to-avoid-the-operator-pattern/)

crds
----

Custom resources are extensions of the Kubernetes API.

Links:

* [Overview](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
* [Defining a schema](https://kubernetes.io/blog/2019/06/20/crd-structural-schema/)

prometheus
----------

Prometheus is a metrics and monitoring system that fits particularly well into the kubernetes environment

* [Overview](https://prometheus.io/docs/introduction/overview/)
* [Data Model](https://prometheus.io/docs/concepts/data_model/)
* [Metric Types](https://prometheus.io/docs/concepts/metric_types/)

After reading a basic overview log into Prometheus and go through all of the tabs

You can find the Openshift Prometheus link on the Administrator->Monitoring->Metrics page

alertmanager
------------

Alertmanager manages and forwards alerts that come from prometheus. It is not responsible for generating alerts.

* [Overview](https://prometheus.io/docs/alerting/latest/overview/)

After reading a basic overview log into Alertmanager and go through all of the tabs and alerts

You can find the Openshift Alertmanager link on the Administrator->Monitoring->Alerting page

prometheus-operator
-------------------

Prometheus Operator allows easy automation of Prometheus inside of the kubernetes environment using CRDs. Important CRDs include:

* Prometheus
* Alertmanager
* ServiceMonitor
* PodMonitor
* PrometheusRule

Links:

* [Intro with good diagram](https://devops.college/prometheus-operator-how-to-monitor-an-external-service-3cb6ac8d5acb)
* [Overview](https://github.com/prometheus-operator/prometheus-operator/blob/master/README.md)
* [API Docs](https://github.com/prometheus-operator/prometheus-operator/blob/master/Documentation/api.md)

cluster monitoring operator
---------------------------

The Cluster Monitoring Operator manages and updates the Prometheus-based cluster monitoring stack deployed on top of OpenShift.

It contains the following components:

* Prometheus Operator
* Prometheus
* Alertmanager cluster for cluster and application level alerting
* kube-state-metrics
* node_exporter

Links:

* [Overview](https://github.com/openshift/cluster-monitoring-operator/blob/master/README.md)


create a PrometheusRule
-----------------------

1. Rename the alert to a unique name (e.g. your name) so that you will be able to find it in Prometheus when searching. `alert: Hello{name}`

2. Search for an interesting metric in prometheus to create an alert on. Use the template contained in this project. Try to find a more interesting expr than just `vector(0)`.

3. Apply the PrometheusRule. Look for it in the Prometheus alert tab. The rule will not appear. You will need to investigate why this is happening, as you can clearly see other PrometheusRules are being loaded.

Hints:

* Review the prometheus operator deployment in openshift-monitoring and review the --namespaces argument to the container. [namespaces cmd line arg](https://github.com/prometheus-operator/prometheus-operator/blob/master/cmd/operator/main.go#L176)
    
    * If you attempt to change the values here directly cluster monitoring operator will set them back automatically during a reconcile step

* Review the cluster-monitoring-operator for how it generates the namespaces value.
    
    * [Namespace Selector](https://github.com/openshift/cluster-monitoring-operator/blob/1cdc504549d439ec5111414ffe7acc8a6e8a513a/cmd/operator/main.go#L90)
    * [Implementation](https://github.com/openshift/cluster-monitoring-operator/blob/922578d7d8a33f39b43b577e74c469b4374e90bd/pkg/manifests/manifests.go#L1878)

* Skip to the end for the final answer


operator-lifecycle-manager
--------------------------

The Operator Lifecycle Manager (OLM) helps users install, update, and manage the lifecycle of all Operators and their associated services running across their clusters. It is part of the Operator Framework, an open source toolkit designed to manage Kubernetes native applications (Operators) in an effective, automated, and scalable way.

Links:

* [OLM](https://docs.okd.io/latest/operators/understanding_olm/olm-understanding-olm.html)


operator-sdk
------------

The Operator SDK provides the tools to build, test, and package Operators. Initially, the SDK facilitates the marriage of an application’s business logic (for example, how to scale, upgrade, or backup) with the Kubernetes API to execute those operations. Over time, the SDK can allow engineers to make applications smarter and have the user experience of cloud services. Leading practices and code patterns that are shared across Operators are included in the SDK to help prevent reinventing the wheel.

The Operator SDK is a framework that uses the controller-runtime library to make writing operators easier by providing:

    High level APIs and abstractions to write the operational logic more intuitively
    Tools for scaffolding and code generation to bootstrap a new project fast
    Extensions to cover common Operator use cases


* [operator-sdk home](https://sdk.operatorframework.io/)
* [operator-sdk guide](https://sdk.operatorframework.io/docs/building-operators/golang/quickstart/)
* [operator-sdk samples](https://github.com/operator-framework/operator-sdk-samples)


PrometheusRule answer
---------------------

Edit your namespace

    oc edit ns my-namespace

Add label

    labels:
      openshift.io/cluster-monitoring: "true"

Check the prometheus-operator deployment again to see if your namespace is in `--namespaces`
Check prometheus to see that your rule showed up
