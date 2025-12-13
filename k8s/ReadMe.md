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
kubectl port-forward rc/{rc_name} {host_port}:{rc_port}
```

- Setting or rollout is not supported in Replication Controller. 

