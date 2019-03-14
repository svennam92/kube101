# Step 7: Traffic Management with Knative

Earlier, we saw how we can use Kubernetes to rollout guestbook v2. Let's do the same with Knative, but take it a step further by taking advantage of canary deployments - rolling out a new service to a subset of users.

### Rollout Guestbook v2

This should look familiar. We're doing the same deploy but with `guestbook:v2`. We set `managed-route` to false as we'll setup our own routing.

```text
knctl deploy \
      --service guestbook \
      --image svennam92/guestbook:v2
      --managed-route=false
```

Access your application again \(something like `http://guestbook.default.fossasia-kube03.sjc04.containers.appdomain.cloud`\). You may have to do a cache-less reload, but you should see Guestbook v2.

### **Traffic Management**

You've now made two deployments, one for v1 and the other for v2. Knative takes snapshots of every deployment you make - you can check them out by running `knctl revision list`:

```text
$ knctl revision list
Revisions

Service    Name             Tags      Annotations  Conditions  Age  Traffic  
guestbook  guestbook-00002  latest    -            5 OK / 5    20s  100% -> guestbook.default.fossasia-kube1.sjc04.containers.appdomain.cloud  
~          guestbook-00001  previous  -            4 OK / 5    23m  -  

2 revisions

Succeeded
```

However, 100% of traffic is being routed to the new version. Let's change it so that 50% of traffic is routed to each of the two revisions. Run the following command:

```text
knctl rollout --route guestbook -p guestbook:latest=50% -p guestbook:previous=50%
```

### Traffic: Managed!

That's it! Your traffic is now being equally routed between the two versions. You can test this by running the following script \(but for your Ingress Subdomain\):

```text
while sleep 0.5; do curl -s http://guestbook.default.fossasia-kube1.sjc04.containers.appdomain.cloud/ | grep title ; done
```

In a real-world deployment, you'd have a much lower percent routing to the newer version. You'd gradually increase that until you've verified that your users are having a positive experience with the new version. Knative makes it really easy to manage routes, traffic and revisions -- all the tools that make operations engineers' lives easier.

