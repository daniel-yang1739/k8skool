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
source <(minikube completion zsh)
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
├── daname/
│   └── namespace.yaml      # Namespace definition
└── deploy/
    ├── nginx.yaml          # Pod example
    ├── deploy.yaml         # Deployment (RollingUpdate)
    └── deploy-recreate.yaml # Deployment (Recreate)
```
