# Popular Microservices Demo Application using Terraform
[![Tigera][tigera.io-badge]][tigera.io] 

This repository brings an all-in-one-yaml files to deploy some of the most popular micro-services applications used for demo purposes.

## Simple Development Environment

Simple development environment is a very basic deployment using only 2 microservices:
- sdkserver
- webserver

The simple development enviroment application will create a the --development-- namespace for its the deployment.
The sdkserver is a pod created with the alpine image and the webserver, well, this is a web server and uses a custom image based on nginx:alpine. 
The webserver service is a LoadBalancer exposing the port 80 and will distribute the HTTP requests across the 2 webserver replicas created by the webserver deployment.
The sdkserver will issue a HTTP GET request using wget to the webserver service every 3 seconds.

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
kubectl get -n developmente service webserver | awk '{print $4}'
``` 

**Example output - do not copy**

EXTERNAL-IP
<your-ip>

**Note**-  you may see `<pending>` while the cloud provides provisions the load balancer. If this happens, wait a few minutes and re-run the command.

[Optional] **Clean up**:

```bash
kubectl delete -f development.yaml
```

And finally, in the webserver_image folder, you can find the Dockerfile and the index.html used to create the image for the webserver microservice.

## Yet Another On-line Bank (yaobank)



## Online Boutique



## Robot Shop




<!-- Links -->
[tigera.io-badge]: https://img.shields.io/badge/Powered%20by-Tigera-orange
[tigera.io]: https://www.tigera.io