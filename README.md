# Microservices Demo Applications
[![Tigera][tigera.io-badge]][tigera.io] 

This repository brings **all-in-one-yaml** files to deploy some of the most popular microservices applications used for demo purposes:

> - [**Simple Development Environment**](README.md/#yet-another-on-line-bank-yaobank)
> - [**Yet Another Online Bank**](README.md/#yet-another-on-line-bank-yaobank)
> - [**Online Boutique**](README.md/#yet-another-on-line-bank-yaobank)
> - [**Robotshop**](README.md/#yet-another-on-line-bank-yaobank)

## Quickstart

You can deploy the application right away, without clonning this repository:

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

The diagram below shows the objects of the simple development environment.

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

<!-- ### Yet Another On-line Bank ### -->


## Online Boutique




<!-- ### Yet Another On-line Bank ### -->


## Robot Shop





<!-- Links -->
[tigera.io-badge]: https://img.shields.io/badge/Powered%20by-Tigera-orange
[tigera.io]: https://www.tigera.io
