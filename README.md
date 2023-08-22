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




