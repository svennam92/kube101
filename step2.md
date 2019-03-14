# Step 2: Deploy Your First Application

Deploy an application to a managed Kubernetes cluster.

## 1. Enable kubectl to work with your cluster

As a pre-req, you should have obtained a cluster. This is a managed Kubernetes cluster hosted on an IBM organization, with access granted to your personal IBM Cloud account. Let's enable `kubectl`, the primary method of working with _any_ Kubernetes cluster, to communicate with this cluster.

Recall the name of the cluster you obtained in the [prereq steps](./#step-4-get-a-kubernetes-cluster). Run   
`# Replace XX with your cluster number!`  
`$ ibmcloud ks cluster-config fossasia-kubeXX`, and set the `KUBECONFIG` env var. You'll see output like this:

```text
The configuration for fossasia-kube03 was downloaded successfully.
Export environment variables to start using Kubernetes.

export KUBECONFIG=/Users/svennam/.bluemix/plugins/container-service/clusters/fossasia-kube03/kube-config-sjc04-fossasia-kube03.yml
```

**Copy/paste the output from** _**your terminal.**_ **Save this output in a notepad/note for future reference. When opening a new terminal, you will have to set this env var again.** If you're comfortable with it, you can also edit your `.bashrc` or similar file to include this env var every time you open a new terminal.

Once your client is configured, you are ready to deploy your first application, `guestbook`.

## **2**. Deploy your application

Now, you will deploy an application called `guestbook` that has already been built and uploaded to DockerHub under the name `svennam92/guestbook:v1`.

1. Start by running `guestbook`:

   `$ kubectl run guestbook --image=svennam92/guestbook:v1`

   This action will take a bit of time. To check the status of the running application, you can use `$ kubectl get pods --watch`. \(`Ctrl+C` to exit the watch process\)

   You should see output similar to the following:

   ```text
   $ kubectl get pods
   NAME                          READY     STATUS              RESTARTS   AGE
   guestbook-59bd679fdc-bxdg7    0/1       ContainerCreating   0          1m
   ```

   Eventually, the status should show up as `Running`.

   ```text
   $ kubectl get pods
   NAME                          READY     STATUS              RESTARTS   AGE
   guestbook-59bd679fdc-bxdg7    1/1       Running             0          1m
   ```

   The end result of the run command is a Pod containing our application container.   


   > Pro Tip: This step also created a `Deployment` resource. While Pods are ephemeral, Deployments continue to live on. That means even if you delete this pod \(`kubectl delete pod <pod_name>`\), Kubernetes will bring up another in its place because the Deployment enforces it!

   Once the status reads `Running`, we need to expose that deployment as a service so we can access it through the IP of the worker nodes. 

2. The `guestbook` application listens on port 3000. Expose it as a service, run: `$ kubectl expose deployment guestbook --type="NodePort" --port=3000 service "guestbook" exposed`
3. To find the port used on that worker node, examine your new service:

   ```text
   $ kubectl get service guestbook
   NAME        TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
   guestbook   NodePort   10.10.10.253   <none>        3000:31208/TCP   1m
   ```

   We can see that our `<nodeport>` is `31208`. We can see in the output the port mapping from 3000 inside the pod exposed to the cluster on port 31208. This port in the 31000 range is automatically chosen, and could be different for you.

4. `guestbook` is now running on your cluster, and exposed to the internet. We need to find out where it is accessible. The worker nodes running in the container service get external IP addresses. Run `$ ibmcloud cs workers <name-of-cluster>`, and note the public IP listed on the `<public-IP>` line.

   ```text
   $ ibmcloud ks workers fossasia-kubeXX
   OK
   ID                                                 Public IP        Private IP     Machine Type   State    Status   Zone    Version  
   kube-hou02-pa1e3ee39f549640aebea69a444f51fe55-w1   173.193.99.136   10.76.194.30   free           normal   Ready    hou02   1.5.6_1500*
   ```

   We can see that our `<public-IP>` is `173.193.99.136`.

5. Now that you have both the address and the port, you can now access the application in the web browser at `<public-IP>:<nodeport>`. In the example case this is `173.193.99.136:31208`.

Congratulations, you've now deployed an application to Kubernetes! In the next portion of the lab, you'll learn how to update a Pod and scale it up.

