---
description: >-
  In this step, we'll setup a domain to access our application, rather than
  clunky IP + Ports. Even if we didn't use Knative, we'd have to follow similar
  steps to setup a domain.
---

# Step 6: Setup Ingress for a Clean URL

### Setup Knative Ingress

First, let's make note of our Ingress subdomain provided by IBM Cloud Kubernetes Service. Run the following command for your cluster:

```text
# Replace XX with your cluster
$ ibmcloud ks cluster-get fossasia-kubeXX
...
Ingress Subdomain:      fossasia-kubeXX.sjc04.containers.appdomain.cloud   
...
```

Ingress is a Kubernetes service that balances network traffic workloads in your cluster by forwarding public or private requests to your apps. This Ingress Subdomain is an externally available and public URL providing access to your cluster. Make a note of the value next to "Ingress Subdomain". You'll want to open a separate notepad/note to save this.

Next, update the default URL for new Knative apps by editing the configuration:

```text
 kubectl edit cm config-domain --namespace knative-serving
```

Change `example.com` to your ingress subdomain, which should look something like: 

```text
fossasia-kubeXX.sjc04.containers.appdomain.cloud
```

Knative applications will now be assigned a route with this host, rather than `example.com`.

### Setup Kubernetes Ingress to Route Requests to Knative

Next, we'll need to tell Kubernetes to route every request coming in with the hostname that matches the subdomain above to our application. Create the following file in your current directory. Change the &lt;ingress\_subdomain&gt; portion to the Ingress Subdomain from above.

{% code-tabs %}
{% code-tabs-item title="ingress.yaml" %}
```yaml
 apiVersion: extensions/v1beta1
 kind: Ingress
 metadata:
     name: iks-knative-ingress
     namespace: istio-system
 spec:
     rules:
         - host: guestbook.default.<ingress_subdomain>
             http:
                 paths:
                     - path: /
                         backend:
                             serviceName: istio-ingressgateway
                             servicePort: 80
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Note that this file tells Kubernetes to route all requests hitting `/` to the istio-ingressgateway, which powers our Knative-based application. Deploy that ingress file to your cluster after making the change to the `host` field:

 `kubectl apply --filename ingress.yaml`

## You're done!

Now you just need to access your application using the subdomain, `guestbook.default.<ingress_subdomain>`. It should look something like this: `http://guestbook.default.fossasia-kube03.sjc04.containers.appdomain.cloud/`

Paste that into your browser, and access your Guestbook application!

## What's next?

Check out more samples and patterns like this on [IBM Developer](https://developer.ibm.com/components/kubernetes/)

Explore your [IBM Cloud Dashboard](https://cloud.ibm.com)

Learn more about Kubernetes and Knative:

{% embed url="https://www.youtube.com/watch?v=aSrqRSk43lY" %}

{% embed url="https://www.youtube.com/watch?v=69OfdJ5BIzs" %}



