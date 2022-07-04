# Microservices Demo Applications
[![Tigera][tigera.io-badge]][tigera.io] 

This repository brings **all-in-one-yaml** files to deploy some of the most popular microservices applications used for demo purposes:

> - [**Simple Development Environment**](/README.md#simple-development-environment)
> - [**Yet Another Online Bank**](/README.md#yet-another-on-line-bank-yaobank)
> - [**Online Boutique**](README.md#online-boutique)
> - [**Robot Shop**](README.md#robot-shop)

## Quickstart

You can deploy the application right away, even without cloning this repository:

**Simple Development Environment**
```bash
kubectl apply -f https://raw.githubusercontent.com/regismartins/demo-apps/main/development/development.yaml
```

**Yet Another Online Bank**
```bash
kubectl apply -f https://raw.githubusercontent.com/regismartins/demo-apps/main/yaobank/yaobank.yaml
```

**Online Boutique**
```bash
kubectl apply -f https://raw.githubusercontent.com/regismartins/demo-apps/main/onlineboutique/onlineboutique.yaml
```

**Robotshop**
```bash
kubectl apply -f https://raw.githubusercontent.com/regismartins/demo-apps/main/robotshop/robotshop.yaml
```

## Simple Development Environment

Simple development environment is a very basic deployment using only 2 microservices:
- sdkserver
- webserver

The simple development enviroment application will create the **development** namespace for its deployment.
The **sdkserver** is a pod created with the alpine image and the **webserver**, well, this is a web server and uses a custom image based on nginx:alpine. 
The **webserver service** is a LoadBalancer exposing the port 80 and will distribute the HTTP requests across the 2 webserver replicas created by the **webserver deployment**.
The **sdkserve** microservice will issue a HTTP GET request using wget to the **webserver service** every 3 seconds.

The diagram below shows the objects of the simple development environment.

![development](https://user-images.githubusercontent.com/104035488/177042841-b7688f6e-0758-4b9b-be13-e527bc4f499f.png)

To deploy this demo application follow the steps below:

1. **Have a kubernetes cluster up and running with sufficient capacity** to support this application. :)

2. **Clone this repository**

```bash
git clone https://github.com/regismartins/demo-apps
cd demo-apps/development
```

3. **Deploy the sample application to the cluster.**

```bash
kubectl apply -f development.yaml
```

4. **Wait for the pods to be ready** 

```bash
kubectl get -n development pods
```

After a few minutes, you shoudl see:

```bash
NAME                             READY   STATUS    RESTARTS   AGE
dev-webserver-5cfd564c58-k7j9j   1/1     Running   0          44m
dev-webserver-5cfd564c58-nl9mn   1/1     Running   0          44m
sdkserver                        1/1     Running   0          44m
```

5. **Access the webserver in a browser** using the webserver's EXTERNAL_IP.

```bash
kubectl get -n development service webserver | awk '{print $4}'
``` 

**Example output - do not copy**

```bash
EXTERNAL-IP
<your-ip>
```

**Note**-  you may see `<pending>` while the cloud provider provisions the load balancer. If this happens, wait a few minutes and re-run the command.

[Optional] **Clean up**:

```bash
kubectl delete -f development.yaml
```

And finally, in the webserver_image folder, you can find the Dockerfile and the index.html used to create the image for the webserver microservice.


<!-- ### Yet Another On-line Bank ### -->


## Yet Another On-line Bank (yaobank)

The "Yet Another Online Bank" (yaobank) application will consist of 3 microservices:
- customer (web gui)
- summary (business logic)
- database (data)

This is an example of a three-tier architecture software application. The yaobank organizes the application in three logical computing layers: the user interface tier (customer microservice), the data processing tier (summary microservice) and the data (database microservice) tiers.

All the Kubernetes resources (Deployments, Pods, Services, Service Accounts, etc.) for yaobank will all be created within the **yaobank namespace**. The **customer service** is a LoadBalancer exposing the port 80 and will forward the HTTP requests to the **customer** pod created by **customer deployment**.

The diagram below shows the objects of the yet another online bank environment.

![yaobank](https://user-images.githubusercontent.com/104035488/177184583-a2a73b00-a235-495a-aede-52f43e5fea94.png)

To deploy this demo application follow the steps below:

1. **Have a kubernetes cluster up and running with sufficient capacity** to support this application. :)

2. **Clone this repository**

```bash
git clone https://github.com/regismartins/demo-apps
cd demo-apps/yaobank
```

3. **Deploy the sample application to the cluster.**

```bash
kubectl apply -f yaobank.yaml
```

4. **Wait for the pods to be ready** 

```bash
kubectl get -n yaobank pods
```

After a few minutes, you shoudl see:

```bash
NAME                        READY   STATUS    RESTARTS   AGE
customer-69ff657cf-mjgdc    1/1     Running   0          31m
database-6c7977f4d5-8brpz   1/1     Running   0          31m
summary-789499469f-d5hsx    1/1     Running   0          31m
summary-789499469f-trt6n    1/1     Running   0          31m
```

5. **Access the webserver in a browser** using the webserver's EXTERNAL_IP.

```bash
kubectl get -n yaobank service customer | awk '{print $4}'
``` 

**Example output - do not copy**

```bash
EXTERNAL-IP
<your-ip>
```

**Note**-  you may see `<pending>` while the cloud provider provisions the load balancer. If this happens, wait a few minutes and re-run the command.

[Optional] **Clean up**:

```bash
kubectl delete -f yaobank.yaml
```

<!-- ### Online Boutique ### -->


## Online Boutique

**Online Boutique** is a cloud-native microservices demo application. Online Boutique consists of a 11-tier microservices application. The application is a web-based e-commerce app where users can browse items, add them to the cart, and purchase them.

You can find more information at the offical project [github repository](https://github.com/GoogleCloudPlatform/microservices-demo).

The diagram below shows the objects of the yet another online bank environment.

![onlineboutique](https://user-images.githubusercontent.com/104035488/177209387-800f9db2-eae3-47a5-95b4-40a0f7caf09f.png)

To deploy this demo application follow the steps below:

1. **Have a kubernetes cluster up and running with sufficient capacity** to support this application. :)

2. **Clone this repository**

```bash
git clone https://github.com/regismartins/demo-apps
cd demo-apps/onlineboutique
```

3. **Deploy the sample application to the cluster.**

```bash
kubectl apply -f onlineboutique.yaml
```

4. **Wait for the pods to be ready** 

```bash
kubectl get -n onlineboutique pods
```

After a few minutes, you shoudl see:

```bash
NAME                                     READY   STATUS    RESTARTS   AGE
adservice-8d6675769-x5t69                1/1     Running   0          21m
cartservice-848976c565-9ql72             1/1     Running   0          21m
checkoutservice-6898f55469-pj9x7         1/1     Running   0          21m
currencyservice-674f46f579-6xsqg         1/1     Running   0          21m
emailservice-5dbfd5fdb5-qfjmw            1/1     Running   0          21m
frontend-78dcf586d4-62sk4                1/1     Running   0          21m
loadgenerator-6549dbbb8b-hkx5s           1/1     Running   0          21m
paymentservice-59dbf5ff58-776v4          1/1     Running   0          21m
productcatalogservice-67dcbcbfcd-tmm2s   1/1     Running   0          21m
recommendationservice-55b469945b-s2khp   1/1     Running   0          21m
redis-cart-6f65887b5d-8hkgm              1/1     Running   0          21m
shippingservice-8669dfbcdb-kblbw         1/1     Running   0          21m
```

5. **Access the webserver in a browser** using the webserver's EXTERNAL_IP.

```bash
kubectl get -n onlineboutique service frontend-external | awk '{print $4}'
``` 

**Example output - do not copy**

```bash
EXTERNAL-IP
<your-ip>
```

**Note**-  you may see `<pending>` while the cloud provider provisions the load balancer. If this happens, wait a few minutes and re-run the command.

[Optional] **Clean up**:

```bash
kubectl delete -f onlineboutique.yaml
```

<!-- ### Robotshop ### -->


## Robot Shop

**Stan's Robot shop** is a sample microservice application you can use as a sandbox to test and learn containerised application orchestration and monitoring techniques. It is not intended to be a comprehensive reference example of how to write a microservices application, although you will better understand some of those concepts by playing with Stan's Robot Shop. To be clear, the error handling is patchy and there is not any security built into the application.

You can get more detailed information from [Steve Waterworth's blog post](https://www.instana.com/blog/stans-robot-shop-sample-microservice-application/) about this sample microservice application.

You can find more information at the offical project [github repository](https://github.com/instana/robot-shop).

The diagram below shows the objects of the yet another online bank environment.



To deploy this demo application follow the steps below:

1. **Have a kubernetes cluster up and running with sufficient capacity** to support this application. :)

2. **Clone this repository**

```bash
git clone https://github.com/regismartins/demo-apps
cd demo-apps/robotshop
```

3. **Deploy the sample application to the cluster.**

```bash
kubectl apply -f robotshop.yaml
```

4. **Wait for the pods to be ready** 

```bash
kubectl get -n robotshop pods
```

After a few minutes, you shoudl see:

```bash
NAME                         READY   STATUS    RESTARTS   AGE
cart-d68b689cc-xwzmt         1/1     Running   0          97s
catalogue-665bd584fd-htxtf   1/1     Running   0          97s
dispatch-765c9c7788-628x4    1/1     Running   0          96s
load-856c7d54bb-b5g2p        1/1     Running   0          93s
mongodb-7755d57bd6-gvn4n     1/1     Running   0          96s
mysql-fbfdb4f58-xm5ft        1/1     Running   0          96s
payment-6b99fb896b-9hfjb     1/1     Running   0          95s
rabbitmq-75d9cf4484-mjdht    1/1     Running   0          95s
ratings-856f484fc-x6cr4      1/1     Running   0          95s
redis-5f4f5c5f97-x5x8v       1/1     Running   0          95s
shipping-56ff6d5d66-hnfk7    1/1     Running   0          94s
user-5865575fd-mh5vs         1/1     Running   0          94s
web-7998c9d5c7-4bfb6         1/1     Running   0          94s
```

5. **Access the webserver in a browser** using the webserver's EXTERNAL_IP.

```bash
kubectl get -n robotshop service web | awk '{print $4}'
``` 

**Example output - do not copy**

```bash
EXTERNAL-IP
<your-ip>
```

**Note**-  you may see `<pending>` while the cloud provider provisions the load balancer. If this happens, wait a few minutes and re-run the command.

[Optional] **Clean up**:

```bash
kubectl delete -f robotshop.yaml
```

/README.md#microservices-demo-applications

---

[⬆️ Back to the top](/README.md#microservices-demo-applications) 

---


<!-- Links -->
[tigera.io-badge]: https://img.shields.io/badge/Powered%20by-Tigera-orange
[tigera.io]: https://www.tigera.io
