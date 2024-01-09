![K8S](/images/kubernetes-ar21.svg)

>NOTE : When you use any of the above yaml files as given in the repository, you will have to run the below command to create any of the Kubernetes Objects :

` kubectl create -f file_name.yaml `


## Some Commands for Exam (Sheet): **[Click Here]( https://gist.github.com/iamavnish/28f361afb6ded81866df816c915157fe "Link")**


> Notes : monolith app broken down into smaller services(micro)

## :shushing_face:  _Secrets_
- Used to store sensitive information like passwords and keys.
- They are stored in an encoded format.
- TO use it we need to :
    - create secret
    - inject it into Pod
- Here the data needs to be **Encoded**, so it is done using ` echo -n 'root' | base64 ` command
- **Decode**
` echo -n 'decode_value' | base64 --decode `

Imperative way to create secrets :

```shell
  #get secrets
  kubectl get secrets

  # describe or get info about particular secret
  kubectl describe secret <secret-name> # This shows the attributes in secret but hides the values.

  kubectl get secret <secret-name> -o yaml # To view the values (encoded).

  # If you enter the pod where secret is injected, you can see decoded values.
  kubectl create secret generic <secret-name> --from-literal=<key1>=<value1> --from-literal=<key2>=<value2>
  kubectl create secret generic <secret-name> --from-file=<path to file> # colon or equals to as delimiter between keys and values


```



## :earth_asia:  _ConfigMaps_
- It is helpful inorder to pass the env variables to the required Pods.
- We need to create **Config Maps** either through imperative or declarative(yaml file) way.

Imperative way to create config map :

```shell
  #for single key value pairs
  kubectl create cm <cm-name> --from-literal=<key1>=<value1> --from-literal=<key2>=<value2>

  #to create from a file
  kubectl create cm <cm-name> --from-file=<path to file> # colon or equals to as delimiter between keys and values
  kubectl create cm <cm-name> --from-file=<directory>
```


## :computer:  _Service Accounts_
- Used by external applications to query or authenticate itself to k8s cluster inorder to use the APIs of k8s.
- As per latest version of k8s first we need to create service account as it will automatically not generate the secret object which contains token
- When we use service account inside the pod, the secret for that service account is mounted as volume inside the pod.

- We need to create token for that particular account by running the command

```shell

kubectl create sa <sa-name>

kubectl get sa

kubectl describe sa <sa-name>

# To fetch token from service account
kubectl describe sa <sa-name> # gives secret name
kubectl describe secret <secret-name> # gives token stored in secret
```

## :fairy_man:  _Jobs_
- Can use it to perform any type of jobs like batch processing, generating a report and them mailing it.
- Some points to remember about job in k8s
    - In case of pods, default value for restart property is Always and in case of jobs, default value for restart property is Never.
    -  Job has pod template. Job has 2 spec sections - one for job and one for pod (in order).
    - A pod created by a job must have its restartPolicy be **OnFailure or Never**. If the restartPolicy is OnFailure, a failed container will be re-run on the same pod. If the restartPolicy is Never, a failed container will be re-run on a new pod.

    -  Job properties to remember: `completions, backoffLimit, parallelism, activeDeadlineSeconds, restartPolicy.`
    -  By default, pods in a job are created one after the other (sequence). Second pod is created only after the first one is finished.


```shell
    kubectl create job busybox --image=busybox -- /bin/sh -c "echo hello;sleep 30;echo world"

    kubectl get jobs

    #once a job is created then it launches a pod to do the work
    #so you check the logs of that pod after we get the name of the pod from running the above the commands to get jobs
    kubectl logs busybox-qhcnx # pod under job

    kubectl delete job <job-name>
```


## :merman:  _Cron Jobs_
- In a cronjob, there are 2 templates - one for job and another for pod.
- In a cronjob, there are 3 spec sections - one for cronjob, one for job and one for pod (in order).
- Properties to remember: spec -> successfulJobHistoryLimit, spec -> failedJobHistoryLimit

```shell
# Create a cron job with image busybox that runs on a schedule and writes to standard output
kubectl create cj busybox --image=busybox --schedule="*/1 * * * *" -- /bin/sh -c "date; echo Hello from Kubernetes cluster"

```

## :basecampy: _Services_
- Service Types:
    - Node port : This services helps to access internal pods from the port on the node(exposing port on node to outside world)
   ![NodePort](https://www.bogotobogo.com/DevOps/Docker/images/Docker-Kubeernetes-Pods-Services/Service-Port-NodePort-TargetPort.png)


    - ClusterIp : This is the default service type and we don't have to specify a service type. It exposes the service on an internal-cluster IP.
From inside (reachable only from within the cluster)
    - LoadBalancer : This service type exposes the service via cloud provider's LB

```shell
kubectl apply -f <svc.yaml>
kubectl get svc
kubectl describe svc <service-name> # to get to know about port, target port etc.
kubectl delete svc <service-name>

kubectl expose pod redis --port=6379 --name=redis-service --type=ClusterIP --dry-run=client -o yaml > svc.yaml
kubectl expose pod nginx --port=80 --target-port=8080 --name=nginx-service --type=NodePort --dry-run=client -o yaml > svc.yaml
kubectl expose deploy <deploy-name> --port=<>
```

> Note : **IP of service is known as Cluster-IP (internal ip). Service can be accessed by pods using Cluster-IP or service name.**

## :basecampy: _Pods_

- A Pod is such that it contain one or more containers inside it.
- Basically an object
-  You can check the configuration of any pod definition file using the below command :

``` kubectl get pod pod_name -o yaml ```
> Note : To copy the above configuration to a file so that in future you can use it create/edit some other pods with almost same configuration.


## :basecampy: _Ingress_
- Can think of it as an Application layer 7 load balancer.



## :control_knobs: _Admission Controllers_



* It helps us enforce better security meausres on how a K8S cluster is to be used.

 To view all admission controllers :
`kube-apiserver -h | grep enable-admission-plugins`


The kube-api yaml file is at location :
`/etc/kubernetes/manifests/kube-apiserver.yaml`



## :butterfly: _Validating and Mutating Admission Controllers_



* Here, controllers first validates and then if required mutates basically change the object definition which is to be created.



## :atom: _API Versions_



#### _Alpha_

- Alpha version might have bugs and is not that reliable.
- can be dropped
- Not enabled by default

#### _Beta_

- After end to end testing and major bugs are fixed then the Alpha version moves to Beta phase.
- enabled by default
- Can have minor bugs


#### _GA Stable_
-  yes ny default
- highly reliable


####  :x:  _API Deprecations_

- If you want to deprecate any resource in that particular API group you need cannot removw it from that group in that current release.
- You need to remove it from the next api version/release and that resource will continue to be present in previous version.
    - Command to check about any particular resource like its version and group :

    ` kubectl explain deployment `


####  :passport_control:  _Controllers_
- Manage, observe and mantain the state of the resources/objects in the k8s cluster.


####  :passport_control:  _Deployment Stratergies_

1. **Blue Green Stratergy**
- The Blue part is that where the application is the older one & Green contains the newer version of the app
- But 100% of traffic is routed to the older one i.e Blue one
- Once green (newer app) - all tests are done and passed then the traffic is routed to green one (newer app) and blue deployment is taken down
- Basically service file is updated to route traffic to Green by changing the selector labels.


2. **Canary Stratergy**  :stopwatch:
- Here, one primary deployment where the current version of application is running and at the same time canary deployment is there where only some small percent of traffic is routed here which is the newer version of application (current app's).
- So, once all tests are done on canary deployment then canary deployment is taken down and current primary version application (pods) are upgraded with newer version of application.


## :wheel_of_dharma: _Helm_
- Also called as Package Manager
-  Manages all **Objects** of an application like pods, deployments, services, etc. at one place.
- If we need to update some values of some objects we can just use values.yaml file and pass in necessary key and value pairs to it and apply a
`helm upgrade` command.

## :chart_with_upwards_trend: _Helm Charts_
- Together value.yaml file & template files of objects form the Helm Chart.

>NOTE : You can refer to already created charts by other people here : **[Artifact Hub](https://artifacthub.io/ "Artifact Hub")**

Some Helm commands :


```helm install [release-name] [chart-name] ```

## :mantelpiece_clock: _Multiple Schedulers_
- We can write our custom logic for `Scheduler`if we want some specific conditions to be met
- Steps to use the custom scheduler:
    1. Download the scheduler binary from k8s website
    2. Create a config file for scheduler where we will define the configuration for custom scheduler like the name, leader, etc
    3. Create a pod or deployment where we would point the config flag to the newly created custom scheduler in the above step
    4. Just create the above pod and check if Scheduler is created or not
    5. Now, create a pod and add `schedulerName` property and add the name of the created custom scheduler to it

```bash

# to check if pod is scheduler on that custom scheduler
kubectl get events -o wide

```

## :tornado: _Static Pods_
- No master node, only a node and `Kubelet` is present on it with a container runtime ed **Docker** & no **K8S Cluster**
- Kubelet knows hows to create Pods
- It can create Pods by periodically checking the folder /etc/kubernetes/manifests in the node where kubelet is running
- Basicaly why use Static Pods?
    1. Because you can setup your control plane components such as API server, controller manager, scheduler, etc (as they all are pods).
    2. So, Kubelet is to be installed on the `Master Node` & then just run the Pod manifests of all the components of control plane
    3. This is how the cluster is setup using the `kubeadm` tool.



## :artist: _Author_

[Rishab Agarwal](mailto:agarwal.risha@northeastern.edu)
















