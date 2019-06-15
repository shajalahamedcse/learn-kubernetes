## Information and Resources

Part of the power of Kubernetes containers comes from their ephemeral nature. They can be easily created, destroyed, and replaced, and this makes them easy to manage. However, sometimes we need to maintain persistent data that survives beyond the life of a pod. In this lab, we will go through how to implement state persistence using PersistentVolumes and PersistentVolume claims. We will create persistent storage and consume it using a pod.
Suppose , Your company needs a small database server to support a new application. They have asked you to deploy a pod running a `MySQL container`, but they want the data to `persist` even if the pod is `deleted` or `replaced`. Therefore, the MySQL database pod requires persistent storage.

You will need to do the following:

#### Create a PersistentVolume:

    * The PersistentVolume should be named `mysql-pv`.
    * The volume needs a capacity of `1Gi`.
    * Use a `storageClassName` of `localdisk`.
    * Use the `accessMode` `ReadWriteOnce`.
    * Store the data locally on the node using a `hostPath` volume at the location `/mnt/data`.


Let's write the descriptor file for PersistentVolume. 

`mysql-pv.yaml`

    kind: PersistentVolume
    apiVersion: v1
    metadata:
        name: mysql-pv
    spec:
        storageClassName: localdisk
        capacity: 
            storage: 1Gi
        accessModes:
            - ReadWriteOnce
        hostPath:
            path: "/mnt/data"

Create the `PersistentVolume`.

    kubectl apply -f mysql-pv.yml


#### Create a PersistentVolumeClaim:

    * The PersistentVolumeClaim should be named `mysql-pv-claim`.
    * Set a resource request on the claim for `500Mi` of storage.
    * Use the same `storageClassName` and `accessModes` as the `PersistentVolume` so that this claim can bind to the `PersistentVolume`.

Let's Create a descriptor file with `vim mysql-pv-claim.yaml`

    kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
        name: mysql-pv-claim
    spec:
        storageClassName: localdisk
        accessModes:
            - ReadWriteOnce
        resources:
            requests:
                storage: 500Mi

Create the `PersistentVolumeClaim`:

    kubectl apply -f mysql-pv-claim.yml




#### Create a MySQL Pod configured to use the PersistentVolumeClaim:

    * The Pod should be named `mysql-pod`.
    * Use the image `mysql:5.6` .
    * Expose the containerPort `3306`.
    * Set an environment variable called `MYSQL_ROOT_PASSWORD` with the value `password`.
    * Add the `PersistentVolumeClaim` as a volume and mount it to the container at the path `/var/lib/mysql`.


Create a descriptor file with  `vi mysql-pod.yml`

Kind: Pod
apiVersion: v1
metadata: 
name: mysql-pod
spec:
containers:
- name: mysql
    image: mysql:5.6
    ports:
    - containerPort: 3306
    env:
    - name: MYSQL_ROOT_PASSWORD
    value: password
    volumeMounts:
    - name: mysql-storage
    mountPath: /var/lib/mysql

volumes:
- name: mysql-storage
    persistentVolumeClaim:
    claimName: mysql-pv-claim

Create the Pod:

    kubectl apply -f mysql-pod.yml

Check the status of the pod with `kubectl get pod mysql-pod`. After a few moments it should be in the RUNNING status.

You can also verify that the pod is interacting with the filesystem at `/mnt/data` on the node. Log in to the node from the Kube master like this, using the same as the password for the kube master:

    ssh cloud_user@10.0.1.102

Check the contents of the PersistentVolume directory:

    ls /mnt/data

You should see files and directories there related to MySQL. These were created by the pod, meaning that your `PersistentVolume` and `PersistentVolumeClaim` are working!