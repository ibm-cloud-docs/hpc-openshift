---

copyright:
  years: 2022
lastupdated: "2022-03-22"

keywords: 

subcollection: hpc-openshift

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:table: .aria-labeledby="caption"}

# Monitoring cluster health and application metrics
{: #monitor-cluster-health-application-metrics}

To enable {{site.data.keyword.monitoringlong}} for your {{site.data.keyword.redhat_openshift_full}} cluster, you need to ensure that the `enable_monitoring` deployment value is set to `true`. By doing so, the offering's automation deploys and configures an {{site.data.keyword.monitoringlong_notm}} instance that allows you to collect (1) cluster metrics to monitor the health and resource usage of your worker nodes and (2) custom application metrics to monitor the status of your workloads.
{: shortdesc}

After your cluster is provisioned, you can monitor your workloads by completing the following steps:

1. From your {{site.data.keyword.redhat_openshift_notm}} cluster page, click your name.
2. In the **Integrations > Monitoring** section, click **Launch**.
3. You will see a number of default templates in **Dashboards > Featured > Kubernetes**. 
4. Select **Workload Status & Performance**. A dashboard opens that displays statistics of your cluster and workloads.

For more information, see [Monitoring cluster health](/docs/openshift?topic=openshift-health-monitor).

