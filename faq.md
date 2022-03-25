---

copyright:
  years: 2022
lastupdated: "2022-03-18"

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
{:faq: data-hd-content-type='faq'}

# FAQs
{: #hpc-openshift-faqs}

## What locations are available for deploying VPC resources?
{: #locations-vpc-resources}
{: faq}

Available regions and zones for deploying VPC resources, and a mapping of those to city locations and data centers can be found in [Locations for resource deployment](/docs/overview?topic=overview-locations).

## What permissions do I need in order to create a cluster using the offering?
{: #permissions-cluster-offering}
{: faq}

Instructions for setting the appropriate permissions for {{site.data.keyword.cloud_notm}} services that are used by the offering to create a cluster can be found in [User access permissions](/docs/openshift?topic=openshift-access_reference#cluster_create_permissions). 

## How many worker nodes can I deploy in my Red Hat OpenShift cluster through this offering?
{: #worker-nodes}
{: faq}

Before you deploy a cluster, it is important to ensure that the cluster-related resource quota settings in your {{site.data.keyword.cloud_notm}} account are appropriate for the size of the cluster that you would like to create. For more information, see [Service and quota limitations](/docs/openshift?topic=openshift-openshift_limitations#tech_limits).

The maximum number of worker nodes that are supported for the `worker_nodes_per_zone` deployment value is 1,000 (see [Deployment values](/docs/hpc-openshift?topic=hpc-openshift-deployment-values)). However, the default quota, as noted in [Service and quota limitations](/docs/openshift?topic=openshift-openshift_limitations#tech_limits), is 500 worker nodes across all of the clusters in a region. If you need to deploy more nodes, [contact IBM Support](/docs/get-support?topic=get-support-using-avatar).

## Where are the Terraform files used by the Red Hat OpenShift offering located?
{: #terraform-file-location}
{: faq}

The Terraform-based templates can be found in this [GitHub repository](https://github.com/IBM-Cloud/hpc-cluster-openshift){: external}.
