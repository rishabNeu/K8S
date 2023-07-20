![K8S](/images/kubernetes-ar21.svg)

>NOTE : When you use any of the above yaml files as given in the repository, you will have to run the below command to create any of the Kubernetes Objects :

` kubectl create -f file_name.yaml `


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

 

## :basecampy: _Pods_

- A Pod is such that it contain one or more containers inside it.
- Basically an object
-  You can check the configuration of any pod definition file using the below command :

``` kubectl get pod pod_name -o yaml ```
> Note : To copy the above configuration to a file so that in future you can use it create/edit some other pods with almost same configuration.



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






## :artist: _Author_

[Rishab Agarwal](mailto:agarwal.risha@northeastern.edu)


















