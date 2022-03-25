---

copyright:
  years: 2022
lastupdated: "2022-03-17"

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
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Getting started with Red Hat OpenShift for HPC
{: #getting-started-tutorial}

{{site.data.keyword.redhat_openshift_full}} for HPC enables you to deploy [{{site.data.keyword.redhat_openshift_notm}} clusters on {{site.data.keyword.cloud}}](/docs/openshift?topic=openshift-getting-started) infrastructure for running HPC workloads. The deployment is performed by using Terraform and {{site.data.keyword.bplong_notm}} as automation frameworks. The following steps outline the high-level flow of events that are performed:

1. **Create a workspace** with the Terraform code from {{site.data.keyword.bplong_notm}}. This step defines the set of configuration properties that are used to perform the automation.
2. **Generate a plan** to confirm whether the configuration properties are valid, so that when you run the Terraform code, all of the resources are provisioned correctly. If the validation fails, fix the configuration properties and try again.
3. **Apply a plan** triggers the actual deployment of the {{site.data.keyword.cloud_notm}} resources to have an HPC cluster up and running by the time the deployment completes. If the deployment fails, identify the reason for failure, fix the problem, and try again. If a change is needed to the configuration properties, it might be better to generate a plan again.

Before you can deploy your {{site.data.keyword.redhat_openshift_notm}} cluster, you need to create or gather some information. To get started, complete the following steps.

## Check your permissions for IBM Cloud services
{: #check-permissions}
{: step}

You need to make sure that you have the appropriate permissions for the {{site.data.keyword.cloud_notm}} services that are used by the offering to create and manage cluster resources. For more information, see the following links:

* [Granting user permissions for VPC resources](/docs/vpc?topic=vpc-managing-user-permissions-for-vpc-resources)
* [Managing user access for Schematics](/docs/schematics?topic=schematics-access)
* [Assigning access to Secrets Manager](/docs/secrets-manager?topic=secrets-manager-assign-access)
* [User access permissions for OpenShift](/docs/openshift?topic=openshift-access_reference)

## Generate API key
{: #generate-api-key}
{: step}

Generate an API key for your {{site.data.keyword.cloud_notm}} account where the {{site.data.keyword.redhat_openshift_notm}} cluster will be deployed. For more information, see [Managing user API keys](/docs/account?topic=account-userapikey).

## Create SSH key
{: #create-ssh-key}
{: step}

Create an SSH key in your {{site.data.keyword.cloud_notm}} account. This is your SSH key that you will use to access the {{site.data.keyword.redhat_openshift_notm}} cluster. Ensure that the SSH key is present in the same resource group and region where the cluster is being provisioned. The offering supports passing multiple, comma-separated SSH keys, if the cluster needs multiple SSH keys. For more information, see [Managing SSH keys](/docs/vpc?topic=vpc-managing-ssh-keys).

## Next steps
{: #getting-started-next-steps}
{: step}

After you've gathered the necessary input values to define your cluster configuration, you are ready to deploy your {{site.data.keyword.redhat_openshift_notm}} cluster. The {{site.data.keyword.redhat_openshift_notm}} cluster can be deployed on {{site.data.keyword.cloud_notm}} by using the {{site.data.keyword.bplong_notm}} UI, {{site.data.keyword.bpshort}} CLI, or the {{site.data.keyword.bpshort}} APIs. If you want to deploy your cluster by using the CLI or API, review the prerequisites for your interface of choice:

* [Setting up the {{site.data.keyword.bplong_notm}} CLI](/docs/hpc-openshift?topic=hpc-openshift-setting-up-cli)
* [Setting up the {{site.data.keyword.bplong_notm}} API](/docs/hpc-openshift?topic=hpc-openshift-setting-up-api)

After you've created and gathered your information and reviewed any additional prerequisites for your interface of choice, you are ready to begin [Creating a workspace](/docs/hpc-openshift?topic=hpc-openshift-creating-workspace).