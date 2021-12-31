# Harbor Docker Registry in Digital Ocean Kubernetes Cluster

Here are steps:

## 1. Creating Kuberenets Cluster

First I create a Kubernetes cluster by refering this atricle https://docs.digitalocean.com/products/kubernetes/quickstart/

After that I connected the cluster in my local machine so that I can start the Harbor installtion process

I ran the following command to get the nodes:

``` kubectl get nodes ```

![1.PNG](images/1.PNG)

## 2. Install Helm Chart

```helm repo add bitnami https://charts.bitnami.com/bitnami```

![2.PNG](images/2.PNG)
I used bitnami helm charts for the installation, but before taht I created ```harbor-values.yaml``` with following values:

```
externalURL: https://hub.prateekjain.dev
commonName: 'hub.prateekjain.dev'
```
Other values I kept as it is. 
```helm install harbor bitnami/harbor --values harbor-values.yaml -n harbor --create-namespace```


Now we are ready to install the chart. For that just run these commands:
![3.PNG](images/3.PNG)

## 3. Check the namespace, services and pods

After running the above command I waited for sometime and then ran the following command to check the objects

### Namespaces:

```kubectl get ns```

![4.PNG](images/4.PNG)

### Services:

```kubectl get svc```

![5.PNG](images/5.PNG)

### Pods:

```kubectl get pod```

![6.PNG](images/6.PNG)

# 4. Adding DNS record

For accessing the registry over domain, I have added the IP of loadbalancer for my domain i.e. https://hub.prateekjain.dev

# 5. Login to the Harbor

After the DNS propagation I went to my registry URL:

![7.PNG](images/7.PNG)

I ran following commnad for getting the default password

``` kubectl get secret --namespace harbor harbor-core-envvars -o jsonpath="{.data.HARBOR_ADMIN_PASSWORD}" | base64 --decode ```

![8.PNG](images/8.PNG)

# 6. Creating the Project

I created a project for pushing the docker images

![9.PNG](images/9.PNG)

# 7. Login to the registry

```sudo docker login hub.prateekjain.dev```

![10.PNG](images/10.PNG)

# 8. Tagging and pushing the image to registry

``` 
docker build -t do-challenge .
sudo docker tag do-challenge:latest hub.prateekjain.dev/do-challenge/do-challenge:latest
sudo docker push hub.prateekjain.dev/do-challenge/do-challenge:latest 
```

![11.PNG](images/11.PNG)