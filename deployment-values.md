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
{:table: .aria-labeledby="caption"}

# Deployment values
{: #deployment-values}

The following deployment values can be used to configure {{site.data.keyword.redhat_openshift_full}} cluster instance on {{site.data.keyword.cloud}}:

| Value | Description | Is it required? | Default value |
| ----- | ----------- | --------------- | ------------ |
| `api_key` | The API key for the {{site.data.keyword.cloud_notm}} account where the {{site.data.keyword.redhat_openshift_notm}} cluster needs to be deployed. For more information on how to create an API key, see [Managing user API keys](/docs/account?topic=account-userapikey). | Yes | None |
| `cluster_prefix` | Prefix that is used to name the {{site.data.keyword.cloud_notm}} resources that are provisioned to build the {{site.data.keyword.redhat_openshift_notm}} cluster. You cannot create more than one instance of the {{site.data.keyword.redhat_openshift_notm}} cluster with the same name. Make sure that the name is unique. Enter a prefix name, for example, `my-openshift`. The length of the prefix should be fewer than 13 characters. | No | hpcc-oc |
| `enable_logdna` | Specify whether you would like to set up a logging configuration for the {{site.data.keyword.redhat_openshift_notm}} cluster. For more information, see [Forwarding cluster and app logs to {{site.data.keyword.la_full_notm}}](/docs/openshift?topic=openshift-health#openshift_logging). | No | true |
| `enable_monitoring` | Specify whether you would like to set up a monitoring configuration for the {{site.data.keyword.redhat_openshift_notm}} cluster. For more information, see [Monitoring cluster health](/docs/openshift?topic=openshift-health-monitor). | No | true |
| `kube_version` | The {{site.data.keyword.redhat_openshift_notm}} version that you want to set up in your cluster. Available versions: 4.9_openshift and 4.8_openshift. For more information, see [Version information and update actions](/docs/openshift?topic=openshift-openshift_versions). | No | 4.9_openshift | 
| `login_node_instance_type` | Specify the virtual server instance profile type name to be used to create the login (bastion) node for the {{site.data.keyword.redhat_openshift_notm}} cluster. For more information, see [Instance Profiles](/docs/vpc?topic=vpc-profiles). | No | bx2-2x8 | 
| `mount_path` | Specify the mount path name for the exported directory on the NFS storage node. For example, if the name provided is `data`, the exported directory will be `/data`. | No | `data` |
| `region` | Enter a region from the following list in which the cluster will be deployed: us-south, us-east, eu-gb, eu-de, jp-tok, au-syd, jp-osa, br-sao, ca-tor. For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations). | Yes | None |
| `resource_group` | Resource group name from your {{site.data.keyword.cloud_notm}} account where the VPC resources should be deployed. For more information, see [Managing resource groups](/docs/account?topic=account-rgs). | No | Default |
| `ssh_key_name` | Comma-separated list of names of the SSH key configured in your {{site.data.keyword.cloud_notm}} account that is used to establish a connection to the {{site.data.keyword.redhat_openshift_notm}} login node. Ensure that the SSH key is present in the same resource group and region where the cluster is being provisioned. If you do not have an SSH key in your {{site.data.keyword.cloud_notm}} account, create one by using the [SSH keys](/docs/vpc?topic=vpc-ssh-keys) instructions. | Yes | None |
| `storage_node_instance_type` | Specify the virtual server instance profile type to be used to create the NFS storage node for the {{site.data.keyword.redhat_openshift_notm}} cluster. The storage node is used to create an NFS file system instance that is exported and can be mounted for sharing data among the {{site.data.keyword.redhat_openshift_notm}} cluster nodes. For more information, see [Instance Profiles](/vpc?topic=vpc-profiles). | No | bx2-2x8 |
| `volume_capacity` | Size in GB for the block storage that will be used to build the NFS file system instance on the storage node. Enter a value in the range 10 - 16,000. For more information, see [Block storage capacity and performance](/docs/vpc?topic=vpc-capacity-performance&interface=ui#block-storage-vpc-capacity). | No | 100 |
| `volume_iops` | Number to represent the IOPS (Input Output Per Second) configuration for block storage to be used for the NFS storage node (valid only for `volume_profile=custom`, dependent on `volume_capacity`. Enter a value in the range 100 - 48,000). For more information, see [Custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles&interface=ui#custom). | No | 100 |
| `volume_profile` | Name of the block storage volume type to be used for the NFS storage node. For more information, see [Block storage profiles](/docs/vpc?topic=vpc-block-storage-profiles). | No | general-purpose |
| `worker_nodes_per_zone` | The number of worker nodes per zone in the default worker pool. Minimum is one node, and 1,000 is the maximum for each zone. For more information, see [Planning your worker node setup](/docs/openshift?topic=openshift-planning_worker_nodes). | No | 1 |
| `worker_pool_flavor` | The flavor of the {{site.data.keyword.redhat_openshift_notm}} worker nodes that you want to create and attach to the cluster. For more information, see [Available flavors for VMs](/docs/openshift?topic=openshift-planning_worker_nodes#vm-table). | No | bx2.4x16 |
{: caption="Table 1. Deployment values" caption-side="top"} 

