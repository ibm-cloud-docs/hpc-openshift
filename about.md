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

# About Red Hat OpenShift for HPC
{: #about-openshift-hpc}

{{site.data.keyword.redhat_openshift_full}} for HPC allows you to deploy [{{site.data.keyword.redhat_openshift_notm}} clusters on {{site.data.keyword.cloud}}](/docs/openshift?topic=openshift-getting-started) infrastructure that use the Kubernetes scheduler for scheduling of HPC workload jobs on cluster nodes. This offering uses open source, Terraform-based automation to provision and configure {{site.data.keyword.vpc_short}} resources that make up the cluster. It deploys the worker nodes in all three zones for the region that you select. With simple steps to define configuration properties and use automated deployment, you can build your own HPC cluster in minutes. In addition, the automation deploys an extra virtual server instance (shown as the Login Node in the architecture diagram) so you can easily build and assemble applications for your cluster right away. 
{: shortdesc}

{{site.data.keyword.redhat_openshift_notm}} for HPC enables three interfaces: UI, API, and CLI. The UI involves use of {{site.data.keyword.bplong_notm}} workspaces. To use the API and CLI interfaces, the Terraform-based automation code is available in this [public GitHub repository](https://github.com/IBM-Cloud/hpc-cluster-openshift){: external}.

The offering enables the initial {{site.data.keyword.redhat_openshift_notm}} for HPC cluster creation. Any updates that are needed post-deployment regarding {{site.data.keyword.redhat_openshift_notm}} configuration or setup should be performed by using {{site.data.keyword.redhat_openshift_notm}} tools and commands. If you use the {{site.data.keyword.bpshort}} interface to make changes to configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.redhat_openshift_notm}} cluster. Restoring it back to a working state might not be easy.
{: important}

## Architecture diagram
{: #architecture-diagram}

![Architecture diagram](images/hpccluster_openshift_architecture.svg){:caption="Figure 1. Architecture diagram of a {{site.data.keyword.redhat_openshift_notm}} for HPC cluster on {{site.data.keyword.cloud_notm}}" caption-side="bottom"}


