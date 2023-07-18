![K8S](/images/kubernetes-ar21.svg)

## :control_knobs: _Admission Controllers_

* It helps us enforce better security meausres on how a K8S cluster is to be used.

 To view all admission controllers :
`kube-apiserver -h | grep enable-admission-plugins`


The kube-api yaml file is at location :
`/etc/kubernetes/manifests/kube-apiserver.yaml`

## :butterfly: Validating and Mutating Admission Controllers

* Here, controllers first validates and then if required mutates basically change the object definition which is to be created.



## :artist: Author

[Rishab Agarwal](mailto:agarwal.risha@northeastern.edu)
















