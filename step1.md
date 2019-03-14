# Step 1: Pre-reqs and Setup

Before you begin the workshop, you need to install the required CLIs to create and manage your Kubernetes clusters in IBM Cloud Kubernetes Service and to deploy containerized apps to your cluster.

This lab includes the information for installing the following CLIs and plug-ins:

* IBM Cloud CLI \(`ibmcloud`\)
* IBM Cloud Container \(`ibmcloud ks`\)
* Kubernetes CLI \(`kubectl`\)

If you already have the CLIs and plug-ins, you can skip this step and proceed to the next one.

## Install the IBM Cloud command-line interface

1. As a prerequisite for the IBM Cloud Container Service plug-in, install the [IBM Cloud command-line interface](https://cloud.ibm.com/docs/cli?topic=cloud-cli-install-ibmcloud-cli#install-ibmcloud-cli). Once installed, you can access IBM Cloud from your command-line with the prefix `ibmcloud`.
2. Log in to the IBM Cloud CLI: `ibmcloud login -a api.ng.bluemix.net`.
3. Enter your IBM Cloud credentials when prompted.
4. When asked which account to use, target the `IBM` account:  
   `1. Sai Vennam's Account (d815248d6bd0cc354df42d43db45ce09)`

   `2. IBM (e2b54d0c3bbe4180b1ee63a0e2a7aba4) <-> 1840867  
   Enter a number> 2`

## Install the IBM Cloud Container Service plug-in

1. To create Kubernetes clusters and manage worker nodes, install the IBM Cloud Container Service plug-in: 

   ```text
   ibmcloud plugin install container-service
   ```

   **Note:** The prefix for running commands by using the IBM Cloud Container Service plug-in is `ibmcloud ks`.

2. To verify that the plug-in is installed properly, run the following command: `ibmcloud plugin list`

   The IBM Cloud Kubernetes Service plug-in is displayed in the results as `container-service`.

3. Set the proper region: `ibmcloud ks region-set us-south`

## Download the Kubernetes CLI

To view a local version of the Kubernetes dashboard and to deploy apps into your clusters, you will need to install the Kubernetes CLI that corresponds with your operating system:

* [OS X](https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/darwin/amd64/kubectl)
* [Linux](https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/linux/amd64/kubectl)
* [Windows](https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/windows/amd64/kubectl.exe)

**For Windows users:** Install the Kubernetes CLI in the same directory as the IBM Cloud CLI. This setup saves you some filepath changes when you run commands later. You can also move the `kubectl` executable to a folder already on the PATH env var, or add the folder to your PATH.

**For OS X and Linux users:**

1. Move the executable file to the `/usr/local/bin` directory using the command `mv /<path_to_file>/kubectl /usr/local/bin/kubectl` .
2. Make sure that `/usr/local/bin` is listed in your PATH system variable.

   ```text
   $echo $PATH
   /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
   ```

3. Convert the binary file to an executable: `chmod +x /usr/local/bin/kubectl`

Verify it works by running `kubectl -h`

