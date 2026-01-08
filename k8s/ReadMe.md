## Labels in k8s
- Labels are Key value pairs
- Labels can be thought of as object tags similar to tags in AWS
- Labels are used for categorization of k8s objects, i.e if a pod is labeled as env=prod, we can `filter` all pods which are tagged/labelled as env=prod
- In Replication Sets multi labels or complex label selections can be done, unlike replication controller support equality and singl label selection.
- Labels can be used on `Nodes` too, once Nodes are tagged, we can use `node selectors` to run pods only on selected nodes.


- Creating templated from kubectl
```bash
kubectl create deployment {deployment_name} --image={image_name} --dry-run=client -o yaml
```

- For local k8s cluster and pod creation to use local images load those images into minikube cluster 
```bash
minikube load image {image_name}:{tag_name}
```

- Minikube start cluster
```bash 
minikube start
```

- Create k8s cluster from k8s config
```bash
kubectl create -f {config_file}
```
- List or get all replication controllers
```bash 
kubectl get rc
```

- Temporary port forwarding
```bash
## port forwarding to a replication controller 
kubectl port-forward rc/{rc_name} {host_port}:{rc_port}

## for port-forwarding to a replication set within a deployment 
kubectl port-forward rs/{rs_name} {host_port}:{rs_port}
```

- Setting or rollout is not supported in Replication Controller. 

- Create a deployment config using command 
```bash 
kubectl create deployment {deployment_name} --image={image_name:tag} --dry-run=client -o yaml > {file_name}.yml
```
- Delete a deployment
```bash
kubectl delete deployment {deployment_name}
```


- Describe Pod state in k8s
```bash
kubectl get pods
kubectl describe pod {pod_name}
```

- Pod states
    - Failed [ atleast one container in the pod is in failed state ]
    - Unknown [ network issues or node on which pod has issues ]

- Pod Conditions [ 5 condtions ]
    - PodScheduled 
    - Ready 
    - Initialized
    - Unschedulable
    - ContainersReady

## Docker for K8s
- Docker images are not readily available to K8s 
- Docker images should be made available to K8s by pushing them to docker repository or by pointing local docker to k8s daemon.
- Check the latest tag within minikube or K8s cluster to double sure about the latest docker image is within k8s

## Replication Controller    
- Replication Controller maintains specified number of pods
- RC maintains total pods across nodes
- A RC config needs apiVersion, kind, metadata, spec
- Pods can be removed from ReplicationController by changing their labels.
- Replication controllers are designed to support rolling updates for services by replacing pods one by one.
- `ReplicationSet` is the next gen replication controller set based labeled selector.
- `ReplicationSet` is recommended to use with Deployments. 


## Deployment
- Deployment is high level API object.
- Deployment declaratively updates `pods` and `replicasets`
- Deployment describes the desired state. Deployment Controller changes the `actual state` to `desired state`.
- Deployment support replication sets, replication sets are next gen to replication controllers. replication sets support multiple label selection.
- With deployment we can 
    - create deployment
    - update deployment
    - do rolling updates
    - rollback
    - do canary deployments

- **useful deployment commands** 
    
    - For checking rollout status of the deployment
```bash
    kubectl rollout status deployment/{deployment_name}
```
    - For rolling back the deployment to a previous state 
```bash
    kubectl rollout undo deployment/{deployment_name}
```
    - For checking rollout history
```bash
    kubectl rollout history deployment/{deployment_name}
```        
    
### Deployments && Labels
- Deployments should match labels with pods, `matched labels are the critical link` between deployments and deployment's managed pods.
- 
```bash
kubectl get pods --show-labels
```
this command would match the labels of pods with deployments
- Emphermal Labels [ those that change often], should not be used as matchLabels in deployments, they can be used as necessary on the deployment's pod metadata labels
