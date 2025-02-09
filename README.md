# Microservices Deployment Project with Kubernetes and Helm

## Overview
This project showcases a comprehensive deployment pipeline for Google's Online Boutique, a cloud-native microservices demo application. The deployment architecture is implemented across three progressive phases, leveraging Kubernetes (Linode LKE), Helm, and Helmfile.

> 🛍️ **Demo Application**: This project deploys [Google's Online Boutique](https://github.com/GoogleCloudPlatform/microservices-demo), a cloud-native microservices application consisting of 11 services that together form a fully-functional e-commerce platform.


![Diagram](https://github.com/Princeton45/microservices-helm-deployment1/blob/main/images/diagram.jpg)

## Phase 1: Kubernetes Deployment with Production & Security Best Practices

![Microservices](https://github.com/Princeton45/microservices-helm-deployment1/blob/main/images/Microservices.png)

### What I Built
- Created Deployment & Service Configurations for all of the Microservices

<details>
<summary>Click to expand YAML</summary>

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: emailservice
spec:
  selector:
    matchLabels:
      app: emailservice
  template:
    metadata:
      labels:
        app: emailservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/emailservice:v0.8.0
        ports:
        - containerPort: 8080
        env:
        - name: PORT
          value: "8080"
        livenessProbe:
          grpc:
            port: 8080
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 8080
          periodSeconds: 5
        resources:
          requests: 
            cpu: 100m
            memory: 64Mi
          limits:
            cpu: 200m
            memory: 128Mi
---
apiVersion: v1
kind: Service
metadata:
  name: emailservice
spec:
  type: ClusterIP
  selector:
    app: emailservice
  ports:
  - protocol: TCP
    port: 5000
    targetPort: 8080

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recommendationservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: recommendationservice
  template:
    metadata:
      labels:
        app: recommendationservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/recommendationservice:v0.8.0
        ports:
        - containerPort: 8080
        env:
        - name: PORT
          value: "8080"
        - name: PRODUCT_CATALOG_SERVICE_ADDR
          value: "productcatalogservice:3550"
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 8080
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 8080
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: recommendationservice
spec:
  type: ClusterIP
  selector:
    app: recommendationservice
  ports:
  - protocol: TCP
    port: 8080
    targetPort: 8080

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: productcatalogservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: productcatalogservice
  template:
    metadata:
      labels:
        app: productcatalogservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/productcatalogservice:v0.8.0
        ports:
        - containerPort: 3550
        env:
        - name: PORT
          value: "3550"
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 3550
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 3550
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: productcatalogservice
spec:
  type: ClusterIP
  selector:
    app: productcatalogservice
  ports:
  - protocol: TCP
    port: 3550
    targetPort: 3550

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: paymentservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: paymentservice
  template:
    metadata:
      labels:
        app: paymentservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/paymentservice:v0.8.0
        ports:
        - containerPort: 50051
        env:
        - name: PORT
          value: "50051"
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: paymentservice
spec:
  type: ClusterIP
  selector:
    app: paymentservice
  ports:
  - protocol: TCP
    port: 50051
    targetPort: 50051

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: currencyservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: currencyservice
  template:
    metadata:
      labels:
        app: currencyservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/currencyservice:v0.8.0
        ports:
        - containerPort: 7000
        env:
        - name: PORT
          value: "7000"
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 7000
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 7000
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: currencyservice
spec:
  type: ClusterIP
  selector:
    app: currencyservice
  ports:
  - protocol: TCP
    port: 7000
    targetPort: 7000

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: shippingservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: shippingservice
  template:
    metadata:
      labels:
        app: shippingservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/shippingservice:v0.8.0
        ports:
        - containerPort: 50051
        env:
        - name: PORT
          value: "50051"
        livenessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 50051
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: shippingservice
spec:
  type: ClusterIP
  selector:
    app: shippingservice
  ports:
  - protocol: TCP
    port: 50051
    targetPort: 50051

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: adservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: adservice
  template:
    metadata:
      labels:
        app: adservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/adservice:v0.8.0
        ports:
        - containerPort: 9555
        env:
        - name: PORT
          value: "9555"
        livenessProbe:
          grpc:
            port: 9555
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 9555
          periodSeconds: 5
        resources:
          requests: 
            cpu: 200m
            memory: 180Mi
          limits:
            cpu: 300m
            memory: 300Mi

---
apiVersion: v1
kind: Service
metadata:
  name: adservice
spec:
  type: ClusterIP
  selector:
    app: adservice
  ports:
  - protocol: TCP
    port: 9555
    targetPort: 9555

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: cartservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: cartservice
  template:
    metadata:
      labels:
        app: cartservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/cartservice:v0.8.0
        ports:
        - containerPort: 7070
        env:
        - name: PORT
          value: "7070"
        - name: REDIS_ADDR
          value: "redis-cart:6379"
        - name: DISABLE_PROFILER
          value: "1"
        livenessProbe:
          grpc:
            port: 7070
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 7070
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: cartservice
spec:
  type: ClusterIP
  selector:
    app: cartservice
  ports:
  - protocol: TCP
    port: 7070
    targetPort: 7070

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-cart
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis-cart
  template:
    metadata:
      labels:
        app: redis-cart
    spec:
      containers:
      - name: redis
        image: redis:alpine
        ports:
        - containerPort: 6379
        livenessProbe:
          initialDelaySeconds: 5
          tcpSocket:
            port: 6379
          periodSeconds: 5
        readinessProbe:
          initialDelaySeconds: 5
          tcpSocket:
            port: 6379
          periodSeconds: 5
        resources:
          requests: 
            cpu: 70m
            memory: 200Mi
          limits:
            cpu: 125m
            memory: 300Mi
        volumeMounts:
        - name: redis-data
          mountPath: /data
      volumes:
      - name: redis-data
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: redis-cart
spec:
  type: ClusterIP
  selector:
    app: redis-cart
  ports:
  - protocol: TCP
    port: 6379
    targetPort: 6379

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: checkoutservice
spec:
  replicas: 2
  selector:
    matchLabels:
      app: checkoutservice
  template:
    metadata:
      labels:
        app: checkoutservice
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/checkoutservice:v0.8.0
        ports:
        - containerPort: 5050
        env:
        - name: PORT
          value: "5050"
        - name: PRODUCT_CATALOG_SERVICE_ADDR
          value: "productcatalogservice:3550"
        - name: SHIPPING_SERVICE_ADDR
          value: "shippingservice:50051"
        - name: PAYMENT_SERVICE_ADDR
          value: "paymentservice:50051"
        - name: EMAIL_SERVICE_ADDR
          value: "emailservice:5000"
        - name: CURRENCY_SERVICE_ADDR
          value: "currencyservice:7000"
        - name: CART_SERVICE_ADDR
          value: "cartservice:7070"
        livenessProbe:
          grpc:
            port: 5050
          periodSeconds: 5
        readinessProbe:
          grpc:
            port: 5050
          periodSeconds: 5
      
---
apiVersion: v1
kind: Service
metadata:
  name: checkoutservice
spec:
  type: ClusterIP
  selector:
    app: checkoutservice
  ports:
  - protocol: TCP
    port: 5050
    targetPort: 5050

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: service
        image: gcr.io/google-samples/microservices-demo/frontend:v0.8.0
        ports:
        - containerPort: 8080
        env:
        - name: PORT
          value: "8080"
        - name: PRODUCT_CATALOG_SERVICE_ADDR
          value: "productcatalogservice:3550"
        - name: CURRENCY_SERVICE_ADDR
          value: "currencyservice:7000"
        - name: CART_SERVICE_ADDR
          value: "cartservice:7070"
        - name: RECOMMENDATION_SERVICE_ADDR
          value: "recommendationservice:8080"
        - name: SHIPPING_SERVICE_ADDR
          value: "shippingservice:50051"
        - name: CHECKOUT_SERVICE_ADDR
          value: "checkoutservice:5050"
        - name: AD_SERVICE_ADDR
          value: "adservice:9555"
        livenessProbe:
          httpGet:
            path: "/_healthz"
            port: 8080
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: "/_healthz"
            port: 8080
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
```
</details>

- Deployed microservices on Linode Kubernetes Engine (LKE)

![Linode](https://github.com/Princeton45/microservices-helm-deployment1/blob/main/images/linode.png)

![Kubernetes-resources](https://github.com/Princeton45/microservices-helm-deployment1/blob/main/images/kubernetes_resources.png)

Now, if I use any of the worker nodes Public IP through port 30117, it will bring me to the online ecommerce website.

![ecom](https://github.com/Princeton45/microservices-helm-deployment1/blob/main/images/ecom.png)

- I applied the following production-grade security configurations & best practices:

    - Making sure that your images contain a version, so it's not always pulling the latest version to avoid issues with untested versions.
    - Configuring liveness probes b/c a pod itself can state it is running and healthy, but the container/application inside could be in an unhealthy state. With the liveness probes, Kubernetes can now know if the application within the Pod is running successfully or not. Kubernetes will automatically restart the app. if there are issues.
    - Configuring readiness probes for each container to let Kubernetes know the application is ready before making the pod ready.
    - Assigning resource requests and limits for the containers.
    - NodePort Service opens the nodePort on each worker node which increases the attack surface. Instead, only use internal services (ClusterIP) and have one entrypoint into the cluster (ideally the entrypoint should be sitting outside of the cluster in a separate server). In this case I used a LoadBalancer service instead of NodePort. I could've also used Ingress as well.
    - Always have more than 1 replica for Deployments.
    - Always use more than 1 worker node in the cluster.
    - Use namespaces to isolate Kubernetes resources.
    - Ensuring that the Images used are free of vulnerabilities. This can be incorporated through vulnerability scans in a build pipeline
    - No root access for containers


## Phase 2: Helm Chart Implementation

### What I Built
- Developed a unified Helm chart for all microservices
- Implemented reusable configurations for Deployments and Services
- Created standardized templates for consistent deployments

```
📁 charts/
├── microservice/
│   ├── templates/
│   ├── .helmignore
│   ├── Chart.yaml
│   └── values.yaml
└── redis/
    ├── templates/
    ├── .helmignore
    ├── Chart.yaml
    └── values.yaml

📁 values/
├── ad-service-values.yaml
├── cart-service-values.yaml
├── checkout-service-values.yaml
├── currency-service-values.yaml
├── email-service-values.yaml
├── frontend-values.yaml
├── payment-service-values.yaml
├── productcatalog-service-values.yaml
├── recommendation-service-values.yaml
├── redis-values.yaml
└── shipping-service-values.yaml
```

The `values.yaml` in the chart folder is used to specify the default values for the charts when it's being deployed.

The values files in the `values` folder is used to override the default values specified in the `values.yaml` file in the charts folder (for e.g, if the default value for replicas is 1 but this time you want to have 5 replicas).

### Technologies Used
- Kubernetes
- Helm

## Phase 3: Helmfile Deployment

### What I Built
- Streamlined deployment process using Helmfile
- Automated multi-service deployments
- Implemented environment-specific configurations

[Deployment Flow Diagram]

### Technologies Used
- Kubernetes
- Helm
- Helmfile

