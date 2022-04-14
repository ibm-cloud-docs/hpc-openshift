---

copyright: 
  years: 2022
lastupdated: "2022-04-14"

keywords: 
content-type: tutorial
services: vpc, loadbalancer-service, monitoring, log-analysis
account-plan: paid
completion-time: 60m
subcollection: hpc-openshift

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}
{:step: data-tutorial-type='step'}

# Setting up a Red Hat OpenShift for HPC cluster
{: #setting-up-red-hat-openshift-hpc-cluster}
{: toc-content-type="tutorial"}
{: toc-services="vpc, loadbalancer-service, monitoring, log-analysis"} 
{: toc-completion-time="60m"}

## Objective
{: #objective}

* Deploy a {{site.data.keyword.redhat_openshift_full}} for HPC cluster with your choice of configuration properties

## Architecture overview and NFS file system setup 
{: #architecture-overview-nfs-file-system-setup}

The {{site.data.keyword.redhat_openshift_notm}} for HPC cluster consists of a login (bastion) node, a storage node where the block storage volume is attached, one or more instances of your {{site.data.keyword.redhat_openshift_notm}} master, and a number of worker nodes. 

* The login node serves as a jump host and it is the only node that has a public IP address. The NFS node has only a private IP address and the only way to reach it is through the login node.
* By default, every cluster in {{site.data.keyword.redhat_openshift_notm}} on {{site.data.keyword.cloud}} is set up with multiple {{site.data.keyword.redhat_openshift_notm}} master instances to ensure the availability and accessibility of your cluster resources, even if one or more of these masters become unavailable. Worker nodes are created across multiple availability zones in a region.
* Every cluster in {{site.data.keyword.openshiftlong_notm}} is controlled by a dedicated {{site.data.keyword.redhat_openshift_notm}} master that is managed by {{site.data.keyword.IBM_notm}} in an {{site.data.keyword.IBM_notm}}-owned {{site.data.keyword.cloud_notm}} infrastructure account. The {{site.data.keyword.redhat_openshift_notm}} master, including all of the master components, compute, networking, and storage resources, is continuously monitored by {{site.data.keyword.IBM_notm}} Site Reliability Engineers (SREs). Since [{{site.data.keyword.openshiftlong_notm}}](https://www.ibm.com/cloud/openshift){: external} is a managed service, you cannot SSH directly into the master and worker nodes.
* The storage node is configured as an NFS server and the block storage is mounted to `/data`, which is exported to share with {{site.data.keyword.redhat_openshift_notm}} cluster worker nodes. If you want to use the NFS storage inside the pods, then you need to create persistent volumes and persistent volume claims and attach them into the pods. For more information, see [Configuring persistent volumes and persistent volume claims](/docs/hpc-openshift?topic=hpc-openshift-configure-pv-and-pvc).

The image that is used by the login node and the storage node is based on RHEL 8.4 by default. In the login node, client packages like `oc`, `kubectl`, and `ibmcloud` are installed for performing {{site.data.keyword.redhat_openshift_notm}} operations.

If your HPC workload needs more libraries, you need to install them on this machine so that they can be used when you assemble your application for deployment to the cluster.
{: note}

## Create SSH key
{: #create-ssh-key}
{: step}

Complete the following steps to create your SSH key:

1. Generate an SSH key on your system by running the following command:

    ```
    ssh-keygen -t rsa
    ```
    {: pre}

2. Copy and save all of the content from `.ssh/id_rsa.pub`. 

## Add SSH key to the VPC infrastructure
{: #add-ssh-key-vpc-infrastructure}
{: step}

1. Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/){: external} by using your unique credentials.
2. From the dashboard, click **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > SSH keys**.
3. Click **Create**.
4. Enter the SSH key name (for example, hpcc-ssh-key), select the resource group, add tags, and select the region.
5. Copy and paste the public key into the _Public key_ field (the contents that you saved from `.ssh/id_rsa.pub`).
6. Click **Add SSH key**.

## Create API key
{: #create-api-key}
{: step}

Complete the following steps to create your API key: 

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM) > API keys**.
2. Click **Create an {{site.data.keyword.cloud_notm}} API key**.
3. Enter a name and description for your API key.
4. Click **Create**.
5. Click **Show** to display the API key, **Copy** to copy and save it for later, or click **Download**.

## Create and configure a Red Hat OpenShift for HPC cluster from the IBM Cloud Schematics UI
{: #create-configure-cluster}
{: step}

Complete the following steps to create your {{site.data.keyword.redhat_openshift_notm}} cluster.

### Creating a workspace using the UI
{: #create-workspace-ui}

1. Go the [{{site.data.keyword.bpshort}}, {{site.data.keyword.cloud_notm}}'s deployment manager](https://cloud.ibm.com/schematics/overview){: external}.
2. In the navigation menu, select **Workspaces**, and then select **Create workspace**.
3. In the _Specify template_ section:
    * Provide the {{site.data.keyword.redhat_openshift_notm}} repository URL that is provided by {{site.data.keyword.cloud_notm}} in this [public GitHub repository](https://github.com/IBM-Cloud/hpc-cluster-openshift){: external}.
    * Select the Terraform version 1.0.11, then click **Next**.
4. In the _Workspace details_ section:
    * Specify the name for your {{site.data.keyword.bpshort}} workspace.
    * Define any tags that you want to associate with the resources that are provisioned through the offering. The tags can later be used to query the resources in the {{site.data.keyword.cloud_notm}} console.
    * Select a resource group.
    * Select a location. The location determines where the workspace actions are run.
    * Provide a description (optional) of the {{site.data.keyword.bpshort}} workspace. 
    * Click **Next**, and then click **Create**. The {{site.data.keyword.bpshort}} workspace is created with the name that you specified. 
5. Go to **Schematics Workspace Settings**, and in the _Variable_ section, click "burger icons" to update the following parameters:
    * Update the `api_key` value with the API key value. Mark it as sensitive to hide the API key in the {{site.data.keyword.cloud_notm}} console.
    * Update the `ssh_key_name` with your {{site.data.keyword.cloud_notm}} SSH key name, such as "openshift-ssh-key", that is created in a specific region in {{site.data.keyword.cloud_notm}}. The SSH key should be configured in the same account that is being used against the API key.
    * Update the `region` value where the cluster is deployed. The selected region should match the region where the SSH key is configured.
    * Update the `cluster_prefix` value to the specific cluster prefix for your {{site.data.keyword.redhat_openshift_notm}} cluster.
    * Update the `worker_nodes_per_zone` value according to your requirement.

Review the following table for more information about cluster deployment parameters:

| Parameter | Description |
| --------- | ----------- |
| `api_key` | This is the API key for the {{site.data.keyword.cloud_notm}} account in which the {{site.data.keyword.redhat_openshift_notm}} cluster needs to be deployed. For more information on how to create an API key, see [Managing user API keys](/docs/account?topic=account-userapikey). |
| `ssh_key_name` | Comma-separated list of names of the SSH key configured in your {{site.data.keyword.cloud_notm}} account that is used to establish a connection to the {{site.data.keyword.redhat_openshift_notm}} login node. Ensure that the SSH key is present in the same resource group and region where the cluster is being provisioned. If you do not have an [SSH key](/docs/vpc?topic=vpc-ssh-keys) in your {{site.data.keyword.cloud_notm}} account, create one by using the SSH keys instructions. |
| `region` | Enter a region from the following list in which the cluster will be deployed: us-south, us-east, eu-gb, eu-de, jp-tok, au-syd, jp-osa, br-sao, ca-tor. For more information, see [Region and data center location for resource deployment](/docs/overview?topic=overview-locations). |
| `cluster_prefix` | Prefix that is used to name the {{site.data.keyword.cloud_notm}} resources that are provisioned to build the {{site.data.keyword.redhat_openshift_notm}} cluster. You cannot create more than one instance of the {{site.data.keyword.redhat_openshift_notm}} cluster with the same name. Make sure that the name is unique. Enter a prefix name, for example, 'my-openshift'. The length of the prefix should be fewer than 13 characters. |
| `worker_nodes_per_zone` | The number of worker nodes per zone in the default worker pool. Minimum is one node, and 1,000 is the maximum for each zone. For more information, see [Planning your worker node setup](/docs/openshift?topic=openshift-planning_worker_nodes).
{: caption="Table 1. Cluster deployment parameters" caption-side="top"}

### Generating a plan using the UI
{: #generate-plan-ui}

1. In the {{site.data.keyword.cloud_notm}} console, after the workspace is created, you can review the properties and the variables that are associated with that workspace by using the _Settings_ tab. Make sure to update the following required parameters: `api_key`, `region`, and `ssh_key_name`. In addition, review the `cluster_prefix` and `worker_nodes_per_zone` values to make sure that they are correct according to your specifications and requirements.
2. After you review all of the values, make any applicable changes, click **Generate plan**.
3. When you click **Generate plan**, a new log is generated that can be viewed in the Jobs tab by clicking **Jobs**.
4. Review the log file for any errors, fix the properties, and regenerate the plan by clicking **Generate plan** again.

### Applying a plan using the UI
{: #apply-plan-ui}

1. After you generate a plan in the {{site.data.keyword.cloud_notm}} console, click **Apply plan**. This action generates a new log that can be viewed in the Jobs tab.
2. Review the log file for any errors, fix the errors, and then click **Apply plan** again. 
3. After you successfully apply a plan, you can review all of the resources that are deployed in this workspace by clicking the _Resources_ tab.
4. Make sure to make a note of the SSH command that's printed at the end of the **Apply plan** log file (you'll use this SSH command to access your cluster).

## Access the Red Hat OpenShift for HPC cluster
{: #access-cluster}
{: step}

You have different options to connect with the {{site.data.keyword.redhat_openshift_notm}} cluster either from the {{site.data.keyword.cloud_notm}} console or the login node.

### Access the cluster from the IBM Cloud console
{: #access-cluster-console}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > OpenShift > Clusters**, and then select your cluster.
2. Click _OpenShift web console_ for the user interface view.

### Access the cluster from the login node with a token
{: #access-cluster-login-node-token}

1. You first need to SSH into the login server node. To get your SSH login command, go to the **Schematics > Workspaces** page in the {{site.data.keyword.cloud_notm}} console, and then select your workspace. You can find your SSH login command in the _Apply plan_ log file in the **Jobs** section of your workspace. The following command is an example:

    ```
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null root@169.63.100.221
    ```
    {: codeblock}

2. In the {{site.data.keyword.cloud_notm}} console, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > OpenShift > Clusters**, and then select your cluster. 
3. Click _OpenShift web console_.
4. Go to **IAM#your-IBMid > Copy login command**, and then click **Display Token**.
5. Copy the command in the **Log in with this token** section and run the command in the login server. The following command is an example:

    ```
    oc login --token=sha256~81NTZ5iEbzWtraVyBX7ZAX9qanowKHce_tIZe65KnSs --server=https://c108-e.eu-gb.containers.cloud.ibm.com:30637
    ```
    {: codeblock}

### Access the cluster from the login node with Red Hat OpenShift config file
{: #access-cluster-login-node-config-file}

1. You first need to SSH into the login server node. To get your SSH login command, go to the **Schematics > Workspaces** page in the {{site.data.keyword.cloud_notm}} console, and then select your workspace. You can find your SSH login command in the _Apply plan_ log file in the **Jobs** section of your workspace. The following command is an example:

    ```
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null root@169.63.100.221
    ```
    {: codeblock}

2. Install the {{site.data.keyword.cloud_notm}} `container-service` plug-in by running the following command:

    ```
    ibmcloud plugin install container-service
    ```
    {: pre}

3. Connect to your {{site.data.keyword.cloud_notm}} account with your API key by running the following command:

    ```
    ibmcloud login --apikey <your-API-key> -r <region-name>
    ```
    {: pre}

4. Get your cluster ID by running the following command:

    ```
    ibmcloud oc cluster ls | grep "your-cluster-prefix-name" | {'print $2'}
    ```
    {: pre}

5. Copy the cluster ID from the previous step into the following command to generate the config file:

    ```
    ibmcloud oc cluster config -c <your-cluster-ID> --admin
    ```
    {: pre}

5. Verify your connection by running the following command:

    ```
    oc get nodes
    ```
    {: pre}

## Validate NFS server
{: #validate-nfs-server}
{: step}

Complete the following steps to validate that the NFS storage is set up and exported correctly:

1. Log in to the storage node by using SSH. To get your SSH login command, go to the **Schematics > Workspaces** page in the {{site.data.keyword.cloud_notm}} console, and then select your workspace. You can find your SSH login command in the _Apply plan_ log file in the **Jobs** section of your workspace. The following command is an example:

    ```
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J root@169.63.100.221 root@10.241.4.12
    ```
    {: codeblock}

2. The following command and example output shows that the data volume, either `/dev/vde` or `/dev/vdd` is mounted to `/data` on the storage node:

    **Command:**

    ```
    df -k | grep data
    ```
    {: pre}

    **Example output:**

    ```
    /dev/vde       104806400  763756 104042644   1% /data
    ```
    {: screen}

3. The following command and example output shows that `/data` is exported as an NFS-shared directory:

    **Command:**

    ```
    exportfs -v
    ```
    {: pre}

    **Example output:**

    ```
    /data          10.248.0.0/16(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,no_root_squash,no_all_squash)
    ```
    {: screen}

4. In this example, at the NFS client end, the `/data` directory in the NFS server is mounted to the local directory, `/data`. The following command and example output shows that the local directory, `/data`, is mounted to the remote `/data` directory on the NFS server: 

    **Command:**

    ```
    df -k | grep data
    ```
    {: pre}

    **Example output:**

    ```
    /dev/vde       104806400  763756 104042644   1% /data
    ```
    {: screen}

## Add or reduce worker nodes in your HPC cluster
{: #add-reduce-worker-nodes-cluster}
{: step}

You can add or reduce the number of worker nodes in your cluster by resizing an existing worker pool, regardless of whether the worker pool is in one zone or spread across multiple zones. For more information, see [Adding worker nodes and zones to clusters](/docs/openshift?topic=openshift-add_workers).

## Update clusters, worker nodes, and cluster components
{: #update-clusters-worker-nodes-cluster-components}
{: step}

You can install updates to keep your {{site.data.keyword.openshiftlong_notm}} cluster up to date. For more information, see [Updating clusters, worker nodes, and cluster components](/docs/openshift?topic=openshift-update).