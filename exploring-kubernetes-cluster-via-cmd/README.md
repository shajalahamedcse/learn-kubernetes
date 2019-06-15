
Additional Information and Resources
We have been given a Kubernetes cluster to inspect. In order to better understand the layout and the structure of this cluster, we must run the appropriate commands. We will need to find the following information about this cluster in order to complete this hands-on lab:

List all the nodes in the cluster.
List all the pods within all the namespaces.
List all the namespaces.
See if there are any pods running in the default namespace.
Find the IP address of the API server running on the master node.
See if there are any deployments.
Find the label applied to the etcd pod on the master node.


Exploring the Kubernetes Cluster via the Command Line
We have been given a Kubernetes cluster to inspect. In order to better understand the layout and the structure of this cluster, we must run the appropriate commands.

Log in to the Kube Master server using the credentials on the lab page (either in your local terminal, using the Instant Terminal feature, or using the public IP), and work through the objectives listed.

List all the nodes in the cluster.
Use the following command to list the nodes in your cluster:

    kubectl get nodes
We should see three nodes: one master and two workers.

List all the pods in all namespaces.
Use the following command to list the pods in all namespaces:

    kubectl get pods --all-namespaces
List all the namespaces in the cluster.
Use the following command to list all the namespaces in the cluster:

    kubectl get namespaces
Here, we should see four namespaces: default, kube-public, kube-system, and web.

Check to see if there are any pods running in the default namespace.
Use the following command to list the pods in the default namespace:

    kubectl get pods
We should see that there aren't any pods in the default namespace.

Find the IP address of the API server running on the master node.
Use the following command to find the IP address of the API server:

    kubectl get pods --all-namespaces -o wide
See if there are any deployments in this cluster.
Use the following command to check for any deployments in the cluster:

    kubectl get deployments
We should see there aren't any deployments in the cluster.

Find the label applied to the etcd pod on the master node.
Use the following command to view the label on the etcd pod:

    kubectl get pods --all-namespaces --show-labels -o wide
Conclusion
That's it — congratulations on completing this lab!