## Description

As a Kubernetes Administrator, you will come across broken pods. Being able to identify the issue and quickly fix the pods is essential to maintaining uptime for your applications running in Kubernetes. In this hands-on lab, you will be presented with a number of broken pods. You must identify the problem and take the quickest route to resolve the problem in order to get your cluster back up and running.

### Additional Information and Resources

You have been given access to a three-node cluster. Within that cluster, there are a number of failing pods. You must discover why they aren’t running and repair them as quickly as possible to get them back up and running. Once you’ve applied the fix, verify the pods in the cluster are running and can operate sufficiently. Perform the following tasks in order to complete this hands-on lab:

 * Identify the broken pods in your cluster.
 * Find out why they are broken.
 * Repair them as quickly as you can.
 * After applying the fix, verify the pods are running.
 * Access the pod directly, install 'wget' utility
 * Access the pod directly to ensure the health of the pod.


#### Identify the broken pods.

Use the following command to see what’s in the cluster:

    kubectl get all --all-namespaces

#### Find out why the pods are broken.

Use the following command to inspect the pod and view the events:

    kubectl describe pod <pod_name>

#### Repair the broken pods.

Use the following command to repair the broken pods in the most efficient manner:

    kubectl edit deploy nginx -n web

#### Ensure pod health by accessing the pod directly.

Use the following command to access the pod directly via its container port:

    wget -qO- <pod_ip_address>:80



Repairing Failed Pods in Kubernetes
In this hands-on lab, we will be presented with a number of broken pods. We must identify the problem and take the quickest route to resolve the problem in order to get our cluster back up and running.

Log in to the Kube Master server using the credentials on the lab page (either in your local terminal, using the Instant Terminal feature, or using the public IP), and work through the objectives listed.

Identify the broken pods.
Use the following command to see what’s in the cluster:

    kubectl get all --all-namespaces

To make this a little easier to read, you could run the following command to view services, pods, and deployments:

    kubectl get svc,po,deploy -n web

Find out why the pods are broken.

Use the following command to inspect the broken pods in a namesspace:

     kubectl describe pods -n web

Use the following command to inspect one of the broken pods and view the events:

    kubectl describe pod <pod_name>

Repair the broken pods.


Executing command on the container

We can execute commands directly on the container once the Pod is up and running. For this, we use the exec command and use the name of the Pod as a parameter. Let’s list the environment variables:

    

Again, worth mentioning that the name of the container itself can be omitted since we only have a single container in the Pod.

Next let’s start a bash session in the Pod’s container:

    kubectl exec -ti $POD_NAME bash

Use the following command to repair the broken pods in the most efficient manner:

    kubectl edit deploy nginx -n web

Where it says image: nginx:191, change it to image: nginx. Save and exit.

Ensure pod health by accessing the pod directly.
Use the following command to access the pod directly via its container port:

TO get a pod Ip:

    kubectl describe pod $POD | awk '/IP/{print $2}'
    wget -qO- <pod_ip_address>:8080
    
Conclusion
Congratulations on completing this lab!