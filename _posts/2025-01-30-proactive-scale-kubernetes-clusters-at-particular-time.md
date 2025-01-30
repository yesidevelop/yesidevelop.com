---
layout: post
title:  "Proactive Scaling: Anticipating Demand for Smoother User Experiences"
date:   2025-01-29 06:40:10 +0500
categories: eks autoscaling cronjob
author: Ali Mehdi
---
## Proactive Scaling: Anticipating Demand for Smoother User Experiences

We all know the drill: applications need to scale. Modern infrastructure, especially in the cloud, allows us to dynamically adjust resources based on demand. The standard approach is **reactive scaling**—if traffic surges, we spin up more servers (or pods in Kubernetes). This works, but what happens during peak times? Scaling takes time, and those precious seconds can lead to slow response times, frustrated users, and potentially lost revenue. What if we could **anticipate** these peaks and scale **before** they hit?

### **The Challenge**
In a previous role, I faced this exact problem. Our **e-commerce site** experienced predictable traffic surges from **12 PM to 9 PM** daily. While **Horizontal Pod Autoscaler (HPA)** and **Cluster Autoscaler** handled scaling, there was a noticeable delay, impacting performance. We needed a **proactive** approach.

### **The Solution: Scheduled Scaling with Kubernetes CronJobs**
We leveraged Kubernetes' API and **CronJobs** to schedule scaling **before** traffic spikes. Here’s how it works:

#### **1. Define the HPA**
A standard **Horizontal Pod Autoscaler (HPA)** definition ensures automatic scaling while setting a higher baseline to handle regular traffic.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: ecommerce-site
  namespace: default
spec:
  maxReplicas: 2000
  minReplicas: 200
  metrics:
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 60
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ecommerce-site
```

#### **2. Grant Permissions**
The scaling script needs permission to modify the HPA. We create a **Role, ServiceAccount, and RoleBinding**:

```yaml
# Role
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: scaling-role
  namespace: default
rules:
- apiGroups: ["*"]
  resourceNames: ["ecommerce-site"]
  resources: ["horizontalpodautoscalers"]
  verbs: ["patch"]

# Service Account
apiVersion: v1
kind: ServiceAccount
metadata:
  name: scaling-sa
  namespace: default

# Role Binding
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: scaling-rolebinding
  namespace: default
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: scaling-role
subjects:
- kind: ServiceAccount
  name: scaling-sa
  namespace: default
```

#### **3. Schedule Scaling with a CronJob**
We use a **CronJob** to modify `minReplicas` before peak hours.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: scaling-cronjob
  namespace: default
spec:
  schedule: '0 12 * * *' # Run daily at 12 PM
  jobTemplate:
    spec:
      template:
        spec:
          serviceAccountName: scaling-sa
          containers:
          - name: ubuntu
            image: ubuntu
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - |
              apt-get update -y && apt-get install curl -y && \
              KUBE_TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token) && \
              curl -k -X PATCH \
                -H "Authorization: Bearer $KUBE_TOKEN" \
                -H "Content-Type: application/merge-patch+json" \
                --data '{"spec": {"minReplicas": 500}}' \
                https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_PORT_443_TCP_PORT/apis/autoscaling/v1/namespaces/default/horizontalpodautoscalers/ecommerce-site
```

### **The Result**
This proactive scaling approach:
- Ensured a smooth user experience during peak hours.
- Reduced the load on the **Cluster Autoscaler**.
- Kept the application **responsive** without delays.

By **anticipating demand** and adjusting resources in advance, we built a **reliable, high-performance** system. 

