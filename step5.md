# Step 5: Deploy with Knative

## Deploy Guestbook with Knative

Run the following command to Deploy guestbook using Knative.

```text
$ knctl deploy \
      --service guestbook \
      --image svennam92/guestbook:v1
```

After a few seconds, it should say `Succeeded`. You can now access the application directly using the External IP address. First, get the external IP address with:

```text
kubectl get svc -n istio-system istio-ingressgateway
```

You also need to know the domain name that Knative assigned to the Service we just deployed. Run the following command, and note the value for `domain`.

```text
kubectl get ksvc guestbook
```

You'll notice that the domain name is `guestbook.default.example.com`, but we don't actually own anything at `example.com`. You'll see how to update this to your own domain name later, but for now we can directly curl the external IP address for our cluster, and pass in a Host header:  
  
`curl -H 'Host: guestbook.default.example.com' {EXTERNAL_IP}`

You should see some HTML output. That's really all it takes to deploy an application with Knative! Next, let's fix our ingress so that we can access the application properly.

