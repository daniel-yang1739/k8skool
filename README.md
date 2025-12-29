# K8s Learning Notes

## Setup

### Install Minikube
```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

### Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

### Shell Completion & Alias
```bash
source <(kubectl completion zsh)
alias k=kubectl
compdef __start_kubectl k
```

## Minikube

```bash
minikube start                      # Start cluster
minikube start --driver='docker'    # Start with docker driver
minikube stop                       # Stop cluster
minikube delete                     # Delete cluster
minikube dashboard                  # Open web dashboard
minikube tunnel                     # Create tunnel for LoadBalancer
minikube ssh                        # SSH into minikube VM
docker exec -it minikube bash       # SSH into minikube node
```

## Cluster & Context

```bash
k get nodes                         # List nodes
k config get-contexts               # List contexts
k config set-context --current --namespace=<ns>  # Switch namespace
```

## Namespace

```bash
k get ns                            # List namespaces
k create ns <name>                  # Create namespace
k create ns <name> --dry-run=client -o yaml > ns.yaml  # Generate YAML
```

## Pods

```bash
k get po -A                         # All pods, all namespaces
k get pods                          # Pods in current namespace
k run nginx --image=nginx           # Create pod imperatively
k run nginx --image=nginx --dry-run=client -o yaml > pod.yaml  # Generate YAML
k describe pod <name>               # Pod details
k delete pod <name>                 # Delete pod
k get po -o wide                    # Show more info (IP, Node)
k exec -it <pod> -- bash            # Interactive shell in pod
watch -n 1 "kubectl get pods"       # Watch pods
```

## Deployment

```bash
k get deployments.apps              # List deployments
k create deploy <name> --image=<img> --replicas=3  # Create deployment
k create deploy <name> --image=<img> --replicas=10 --dry-run=client -o yaml > deploy.yaml
k describe deployments.apps <name>  # Deployment details
k edit deployments.apps <name>      # Edit deployment
k delete deployments.apps <name>    # Delete deployment
```

## ReplicaSet

```bash
k get replicasets.apps              # List replicasets
k describe replicasets.apps <name>  # ReplicaSet details

## Storage

```bash
k get pv                            # List persistent volumes
k get pvc                           # List persistent volume claims
k delete pvc <name>                 # Delete a PVC
k get sc                            # List storage classes
```
```

## Apply & Delete

```bash
k apply -f <file.yaml>              # Apply manifest
k delete -f <file.yaml>             # Delete from manifest
```

## Deployment Strategies

### RollingUpdate (default)
```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxUnavailable: 1
    maxSurge: 1
```

### Recreate
```yaml
strategy:
  type: Recreate
```

## Project Structure

```
├── deploy/
│   ├── deploy-error.yaml    # Deployment with intentional error
│   ├── deploy-recreate.yaml # Deployment (Recreate strategy)
│   ├── deploy.yaml          # Deployment (RollingUpdate strategy)
│   ├── frontend.yaml        # Frontend resources
│   ├── nginx-doc.yaml       # Nginx documentation
│   ├── nginx-storage.yaml   # Nginx with persistent storage
│   └── nginx.yaml           # Basic Nginx pod
└── mealie/
    ├── deployment.yaml      # Mealie deployment
    ├── namespace.yaml       # Mealie namespace
    ├── nginx-mealie.yaml    # Nginx configuration for Mealie
    ├── service.yaml         # Mealie service
    └── storage.yaml         # Mealie storage configuration
```
