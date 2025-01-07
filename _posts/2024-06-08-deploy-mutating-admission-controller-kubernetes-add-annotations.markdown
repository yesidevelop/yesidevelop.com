---
layout: post
title:  "Add Annotations to New Pods with a Kubernetes Mutating Admission Controller"
date:   2024-06-08 01:48:12 +0500
categories: devops k8s kubernetes
author: Ali Mehdi
---
# Introduction

Ever wondered when a new kubernetes resource is created (say namespace), how there are some additional resources also created automatically. For example if you create a new namespace, there is a `default` service account which is already created inside newly created namespace. The `default` service account is actually created by Service Account Admission Controller.

So what is Admission Controller?

Kubernetes docs say

> An admission controller is a piece of code that intercepts requests to the Kubernetes API server prior to persistence of the object, but after the request is authenticated and authorized.

So it means that they are used to modify or verify the resources before storing into ETCD

Kubernetes is pluggable architecture, which means that we can create our own (kind of) or use external plugins to attach with kubernetes. Like ingress controllers are implemented outside kubernetes core and deployed inside kubernetes. Similarly we can create our own Admission Controllers.

There are few areas where we will need to create Admission Controllers 

- Lets assume that our requirement is that the every pod created in specific namespace have unique names. To assure the uniqueness we can create our own admission controller to verify the consistency
- If we need to append any annotations to set of pods
- If we need to create additional resources based on annotations
- If we need to inject sidecar containers based on annotations

and list goes on

For simplicity, Lets implement a solution which will create a annotation to every newly pod created. But before implementing lets further drill down what are the types of admission controllers

## Source Code

If you are just curious of the source code. Here is the GitHub repo

<a href="https://github.com/yesidevelop/admission-controller" target="_blank">GitHub Repo</a>


## Types of Admission Controllers:

- In-Tree Admission Controllers (These are like built in admission controllers, like I showed for service accounts above)
- Out of Tree Admission Controllers: These are developed outside kubernetes source code (By community or externally)


## Admission Webhooks

Admission Webhooks are HTTP callbacks, they receive the requests, process them, and return the response (These sends requests externally or within kubernetes and send response as decision)

Further, there are two types of Admissions Webhooks

- Mutating Admission Webhooks
- Validating Admission Webhooks

**Mutating Admission Webhooks** are those which change the object before it is applied in kubenretes

**Validating Admission Webhooks** are those which either accept or reject the object passed to kubernetes, these dont change the object definition etc


## Admission Controllers Flow:

![Admission Controllers](/assets/images/admission-controllers.png)

I am not going to get into the detail of above diagram, but the only thing to see is that mutating webhook is called before validating webhook. This ensures that whatever is stored in ETCD is validated just before the storing procedure

## Creating the Mutating Admission Controller:

There are multiple ways to create webhooks. You can create a webhook externally or internally. But essential idea is same. i.e. create an `https` endpoint which will accept the requests from resources and in response will send the mutated data.

This approach is language agnostic, but community is shifted towards Go lang and internal implementation of Mutating Admission Controller. It is highly perofmant, and also it is native implementation of kubernetes,

So lets get start it

## Prerequisite

- I assume that you have a kubernetes cluster deployed, either locally (like Kind) or in cloud like (EKS)
- Little Understanding of Golang is preferred but if you are just going through this blog then you will get almost everything if you are familiar with any programming language

## Create and HTTPS server for Webhook


<script src="https://gist.github.com/yesidevelop/9fb2f20c36726c0c9e42cbe7ee563e64.js"></script>

## Lets go through the code

I will not go into the detail of each line of code, but give you the basic idea of how it is working

If you see function `main` It is the starting point of application

{% highlight go linenos %}
func main() {

	var tlsKey, tlsCert string
	flag.StringVar(&tlsKey, "tlsKey", "/etc/certs/tls.key", "TLS key")
	flag.StringVar(&tlsCert, "tlsCert", "/etc/certs/tls.crt", "TLS certificate")
	flag.Parse()


	router := mux.NewRouter()
	router.HandleFunc("/mutate", mutate)
	fmt.Printf("Server listening on port %d\n", port)
	http.ListenAndServeTLS(":8443", tlsCert, tlsKey, router)
	
}
{% endhighlight %}

In my code Package [gorilla/mux](https://https://github.com/gorilla/mux)` is implemented, which is the powerful http router. 

From `line 3-7`, These are settings to add SSL certificates. I have used self signed SSL certificates for this purpose (In my Github repo I have shared how to generate the self signed certificates)

Line `9-12` are responsible for serving our app over web

Line `10` is the most important one here, which calls `mutate` function and server over `/mutate` path

Lets disect `mutate` function


## mutate() Function:

Mutate is a big function. So lets use small part of it in order to understand better


{% highlight go %}
func mutate(w http.ResponseWriter, r *http.Request) {
	var review admissionv1.AdmissionReview

	body, err := ioutil.ReadAll(r.Body)
{% endhighlight %}


First few lines of the function are used to get requests from arguments and then read the requests and stores in the `body` variable (Or throw error if there is some issue with body)

We are initializing `review` object which will eventually store the kubernetes data which will be coming through the requests


{% highlight go %}
if err := json.Unmarshal(body, &review); err != nil {
    http.Error(w, "could not decode request body", http.StatusBadRequest)
    return
}
{% endhighlight %}

**Unmarshaling** refers to the process of converting encoded data (typically JSON or YAML) into a Go struct or object.

Above lines are used to convert encoded body into kubernetes object `review` variable previously created

{% highlight go %}
func mutate(w http.ResponseWriter, r *http.Request) {
	pod := corev1.Pod{}
{% endhighlight %}


This creates an empty pod struct and assign it into `pod` variable

{% highlight go %}
if pod.Namespace != "default" {
// Skip pods not in the default namespace
review.Response = &admissionv1.AdmissionResponse{
    UID:     review.Request.UID,
    Allowed: true,
}
{% endhighlight %}


Code shows how we are annotating pods with only `default` namespace

If namespace is not `default` then we will not do anything, we will just pass the request

Else

{% highlight go %}
if err := addAnnotation(&pod); err != nil {
    http.Error(w, fmt.Sprintf("error adding annotation: %v", err), http.StatusInternalServerError)
    return
}
{% endhighlight %}

If it is `default` namespace then we will call addAnnotation function to add our annotations

And

{% highlight go %}
review.Response = &admissionv1.AdmissionResponse{
    UID:       review.Request.UID,
    Allowed:   true,
    Patch:     patchBytes,
    PatchType: func() *admissionv1.PatchType { pt := admissionv1.PatchTypeJSONPatch; return &pt }(),
}
{% endhighlight %}


patch it to the pod


Finally

{% highlight go %}
w.Header().Set("Content-Type", "application/json")
json.NewEncoder(w).Encode(review)
{% endhighlight %}

We again encode it and return the encoded json so that it will be patched


Lets Dockerize the above code and deploy it on kubernetes

## Dockerize the module

I am using Go version 1.22

Here is the Dockerfile

<script src="https://gist.github.com/yesidevelop/d265217f0800b00f5f8543e279103d6e.js"></script>

It basically installs the dependencies and builds it and finally calls the file which was build i.e. `./webhook`


I have used EKS (ECR for repository) for our purpose, but it should work on any of the kubernetes flavor, like Kind, GKE, AKS etc

Here are the build commands

{% highlight shell %}
docker build -t mutating-webhook .
docker tag mutating-webhook:latest <ecr-image-url>
docker push <ecr-image-url>
{% endhighlight %}


Once it will be pushed, we are ready to deploy


## Lets create certificates

{% highlight shell %}
openssl genrsa -out tls.key 2048
openssl req -new -key tls.key -out tls.csr -subj "/CN=my-webhook-server.default.svc"
openssl x509 -req -extfile <(printf "subjectAltName=DNS:my-webhook-server.default.svc") -in tls.csr -signkey tls.key -out tls.crt
{% endhighlight %}

I have used openssl to general tls certificates for my `default` namespace



## Kubernetes TLS Secret

Create a Kuberentes secret with the TLS certifcates we generated above

{% highlight shell %}
kubectl create secret tls my-webhook-server-tls \
    --cert "tls.crt" \
    --key "tls.key"
{% endhighlight %}


## Deploy the webhook

Here is the deployment yaml and it is exposed internally through the service as https


{% highlight yaml %}
# File: webhook.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-mutating-webhook
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-mutating-webhook
  template:
    metadata:
      labels:
        app: my-mutating-webhook
    spec:
      containers:
      - name: my-mutating-webhook
        image: <ecr-image-url>
        imagePullPolicy: Always
        ports:
        - containerPort: 8443
        volumeMounts:
        - name: tls-certs
          mountPath: /etc/certs
          readOnly: true
      volumes:
      - name: tls-certs
        secret:
          secretName: my-webhook-server-tls

---

apiVersion: v1
kind: Service
metadata:
  name: my-mutating-webhook
spec:
  selector:
    app: my-mutating-webhook
  ports:
  - protocol: TCP
    name: http
    port: 80
    targetPort: 8443
  - name: https
    port: 443
    targetPort: 8443

{% endhighlight %}


Replace `<ecr-image-url>` with your ECR url

The above yamls will create a pod which will be available on secured SSL endpoint through the service. Our Mutating Webhook will be used to call it to get the decision response


## Now question is: How will it add annotation with every pod in default namespace

When we deploy it will it automatically execute. Hmmm. No!!


## MutatingWebhookConfiguration for registering Mutating Webhook

For this we will need to generate `kind: MutatingWebhookConfiguration` for it

A `MutatingWebhookConfiguration` is a resource used to **register** webhooks that can modify (mutate) the objects sent to the Kubernetes API server.


{% highlight yaml %}
#File webhook-configuration.yaml
apiVersion: admissionregistration.k8s.io/v1
kind: MutatingWebhookConfiguration
metadata:
  name: my-mutating-webhook
webhooks:
  - name: mutating.webhook.example.com
    clientConfig:
      service:
        namespace: default
        name: my-mutating-webhook
        path: "/mutate"
      caBundle: "<ca-bundle-base64-encoded>"
    admissionReviewVersions:
      - v1
    rules:
      - operations: ["CREATE", "UPDATE"]
        apiGroups: [""]
        apiVersions: ["v1"]
        resources: ["pods"]
    sideEffects: None
    timeoutSeconds: 5
{% endhighlight %}

In above step, we will need the base64 encoded value of ca-bundle

`rules` show when it will be executed. In our case it will be executed in `create` / `update` request

Here is how you can get


{% highlight shell %}
cat tls.crt | base64 | tr -d '\n'
{% endhighlight %}

Copy the value generated from above command inside `<ca-bundle-base64-encoded>`


Once it is done, apply all the yamls, like

{% highlight shell %}
kubectl apply -f webhook.yaml
kubectl apply -f webhook-configuration.yaml
{% endhighlight %}


We are done! We just need to test if it is working or not

Now we just need to create pods in `default` namespace to see how it is behaving


## Lets test it out

Now lets create a deployment like following to create pods in `default` namespace

{% highlight yaml %}
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
{% endhighlight %}

Once you apply the above yaml, it will create the pods

To see their annotations you can write

{% highlight shell %}
kubectl edit po <pod-name>
{% endhighlight %}

Following is the snippet which I got from the deployment I did


{% highlight yaml %}
apiVersion: v1
kind: Pod
metadata:
  annotations:
    my-annotation: added-by-webhook
  creationTimestamp: "2024-06-08T09:36:04Z"
  generateName: nginx-deployment-7c79c4bf97-
  labels:
    app: nginx
    pod-template-hash: 7c79c4bf97
  name: nginx-deployment-7c79c4bf97-8r94m
  namespace: default
{% endhighlight %}


## Feedback:

We'd love to hear your thoughts and answer any questions. Mail me at alimehdi.official@gmail.com. Cheers!
