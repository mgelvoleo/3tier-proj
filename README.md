# How to create 3 tier proj


Step: 1 Create to instance one for Master and Node


Step: 2 Access the Master node

  step 2.1:

  Used Shell script
  
  https://github.com/mgelvoleo/installation-script/tree/main/kubernetes
  
  installed kubernetes steps by steps

  Note: You need to be super user for execute the scripts

  step 2.2 Switch to root
  
  ```
  sudo su
  ```

  step 2.3 open editor
  
  ```
  vi kubeadm.sh
  ```

  step 2.4 paste the script step 1 to 10
  

  step 2.5 execute the script

  ```
    sh kubeadm.sh
  ```

  step 2.6 run the or init the kube version

  ```
  kubeadm init --kubernetes-version=${KUBE_VERSION}
  ```

  ```
  sudo su
  ```

  ```
  kubectl get nodes
  ```


  ```
  kubectl create cm db-config --from-literal=MYSQL_DATABASE=sqldb
  ```

  ```
  kubectl create secret generic db-secret --from-literal=MYSQL_ROOT_PASSWORD=rootpassword
  ```


  ```
  kubectl run mysql-pod --image=mysql --dry-run=client -o yaml > mysql-pod.yaml
  ```
  

  ```
  vi mysql-pod.yaml
  ```


  ```
  apiVersion: v1
  kind: Pod
  metadata:
    createTimestamp: null
    labels:
      run: mysql-pod
    name: mysql-pod
  spec:
    containers:
    - image: mysql
      name: mysql-pod
      envFrom:
        - configMapRef:
          name: db-config
        - secretRef:
          name: db-secret
        resources: {}
        .......
  ```

  ```
  kubectl apply -f mysql-pod.yaml
  ```

  ```
  kubectl get all
  ```

  ```
  kubectl expose pod mysql-pod --port=3306 --target-port=3306 --name=mysql-svc
  kubectl get all
  ```

  After excute the kubectl get all get the IP addres to PMA_HOST
  ```
  kubectl create cm phpadmin-config --from-literal=PMA_HOST=10.109... --from-literal=PMA_PORT=3306
  ```


```
  kubectl create secret generic phpadmin-secret --from-literal=PMA_USER=root --from-literal=rootpasswd
```


```
  kubectl run mysql-pod --image=phpmyadmin --dry-run=client -o yaml > phpadmin-pod.yaml
```


```
  vi mysql-pod.yaml
```


```
  apiVersion: v1
  kind: Pod
  metadata:
    createTimestamp: null
    labels:
      run: phpadmin-pod
    name: phpadmin-pod
  spec:
    containers:
    - image: phpmyadmin
      name: phpadmin-pod
      envFrom: 
        - configMapRef:
          name: phpadmin-config
        - secretRef:
          name: phpadmin-secret
        resources: {}
        .......
```

```
  kubectl apply -f phpadmin-pod.yaml
```


```
kubectl expose pod phpadmin-pod --type=NodePort --port=8099 --target-port=80 --name=phpadmin-svc
```


```
kubectl get all
```

### Access the public url and with port after we expose the port


### Import the sqldb using the phpmyadmin by getting the db in the repo


### Clone the repo for starting build the apps

```
git clone  https/github.com....
```

### Change the configuration of index, the localhost, user, and passwd

### Get the ip address of mysql service
```
kubectl get all
```


## Start to build a images for your changes

```
docker build -t mgelvoleo/phpwebapp:latest .
docker images ls
```

## Push to the docker hub. Note make sure you need to login in you docker hub in order we can push
```
docker image push mgelvoleo/phpwebapp:latest
```

## Run the images of php

```
docker image ls
kubectl run pod php-app --image=mgelvoleo/phpwebapp
kubectl get all
kubectl expose pod php-app --type=NodePort --port=8088 --target-port=80 phpapp-svc
kubectl get all
```

## Try to view the site by getting the public address and the port




 






Step: 3 Access the Slave node 


    step 3.1:

    Used Shell script
    
    https://github.com/mgelvoleo/installation-script/tree/main/kubernetes
    
    installed kubernetes steps by steps

    Note: You need to be super user for execute the scripts

    Step 3.2 Switch to root
    ```
    sudo su
    ```

    Step 3.3 open editor
    ```
    vi kubeadm.sh
    ```

    Step 3.4 past the script step 1 to 10


     step 3.5 execute the script

    ```
        sh kubeadm.sh
    ```




