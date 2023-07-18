![K8S](/images/kubernetes-ar21.svg)

>NOTE : When you use any of the above yaml files as given in the repository, you will have to run the below command to create any of the Kubernetes Objects :

` kubectl create -f file_name.yaml `


## :basecampy: _Pods_
---

- A Pod is such that it contain one or more containers inside it.
- Basically an object
-  You can check the configuration of any pod definition file using the below command :

``` kubectl get pod pod_name -o yaml ```
> Note : To copy the above configuration to a file so that in future you can use it create/edit some other pods with almost same configuration.
---


## :control_knobs: _Admission Controllers_
---

* It helps us enforce better security meausres on how a K8S cluster is to be used.

 To view all admission controllers :
`kube-apiserver -h | grep enable-admission-plugins`


The kube-api yaml file is at location :
`/etc/kubernetes/manifests/kube-apiserver.yaml`

---

## :butterfly: Validating and Mutating Admission Controllers

---

* Here, controllers first validates and then if required mutates basically change the object definition which is to be created.

---

## :atom: API Versions

---

#### _Alpha_
- Alpha version might have bugs and is not that reliable.
- can be dropped
- Not enabled by default

#### _Beta_
- After end to end testing and major bugs are fixed then the Alpha version moves to Beta phase.
- enabled by default
- Can have minor bugs

---


## :artist: Author

---

[Rishab Agarwal](mailto:agarwal.risha@northeastern.edu)


















