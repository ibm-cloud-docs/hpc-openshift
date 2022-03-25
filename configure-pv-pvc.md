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

# Configuring persistent volumes and persistent volume claims
{: #configure-pv-and-pvc}

## Before you begin
{: #before-you-begin}

Before you begin, it is important to understand the Kubernetes concepts of a persistent volume (PV) and a persistent volume claim (PVC) and how they work together in a cluster. Review [Understanding Kubernetes storage basics](/docs/openshift?topic=openshift-kube_concepts) for more information.

## Creating the PV and PVC
{: #creating-pv-pvc}

Complete the following steps to create the PV and PVC on the NFS storage that was provisioned for your cluster:

1. In the login node, create a file (pvc.yml) and populate it with the following:

    ```
    ---
    ## Create a PV with your NFS shared folder.
    apiVersion: v1
    kind: PersistentVolume
    metadata:
    name: nfs-pv-hpcc
    spec:
    capacity:
        storage: 100Gi
    volumeMode: Filesystem
    accessModes:
        - ReadWriteMany
    persistentVolumeReclaimPolicy: Recycle
    storageClassName: nfs-sc-hpcc
    mountOptions:
        - hard
        - nfsvers=4.1
    nfs:
        path: /data            ## Shared folder of NFS server
        server: 163.68.86.201  ## IP address of the NFS server

    ---
    ## Create a PVC from PV.
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
    name: nfs-pvc-hpcc
    namespace: hpcc # Choose your own Namespace 
    spec:
    accessModes:
        - ReadWriteMany
    storageClassName: nfs-sc-hpcc
    resources:
        requests:
        storage: 100Gi
    ```
    {: codeblock}

2. Before you create the PV and PVC, you need to update the following values in the pvc.yml file that you create:
    * Replace the server IP address with the NFS storage node IP address.
    * If you used a mount path name other than the default `/data` when you created the cluster, replace the path name with the one that you specified. 
    * Replace storage capacity based on the size of the NFS storage volume that you created. 
3. After you create and update the pvc.yml file, run the following command to create the PV and PVC:

    ```
    oc apply -f pvc.yml
    ```
    {: pre}

4. Check the status of the PV and PVC by running the following command:

    ```
    oc get pv
    ```
    {: pre}

    ```
    oc get pvc
    ```
    {: pre}

## Validating the PV and PVC
{: #validate-pv-pvc}

Complete the following steps to validate the PV and PVC with a sample application:

1. Create a nginx.yml file with the following content:

    ```
    apiVersion: v1
    kind: Pod
    metadata:
    name: nginx-pv-pod
    spec:
    volumes:   ## Volume declaration
        - name: nginx-pv-storage
        persistentVolumeClaim:
            claimName: nfs-pvc-hpcc
    containers:
        - name: nginx
        image: nginx
        ports:
            - containerPort: 80
            name: "nginx-server"
        volumeMounts:
            - mountPath: "/usr/share/nginx/html"
            name: nginx-pv-storage
    ```
    {: codeblock}

2. Create a pod by running the following command:

    ```
    oc apply -f nginx.yml
    ```
    {: pre}

3. Check the status of the pod by running the following command:

    ```
    oc get pods
    ```
    {: pre}

4. Validate that the PVC is attached to the pod by running the following command:

    ```
    oc exec -it <pod name> bash
    ```
    {: pre}

5. Inside the pod, run `df -h` to see the mount path.

If your NFS storage is 100 GB, you can create a PV with 100 GB. You cannot create a PVC with a size larger than the PV size (less than or equal to 100 GB is possible).
{: note}
