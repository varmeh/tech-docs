# Kubernetes: A Quick Primer

- [Kubernetes: A Quick Primer](#kubernetes-a-quick-primer)
  - [What is Kubernetes?](#what-is-kubernetes)
  - [Key Concepts](#key-concepts)
    - [1. Cluster](#1-cluster)
    - [2. Node](#2-node)
    - [3. Pod](#3-pod)
    - [4. Service](#4-service)
    - [5. Deployment](#5-deployment)
    - [6. ConfigMap \& Secret](#6-configmap--secret)
    - [7. Volume](#7-volume)
    - [8. Namespace](#8-namespace)
    - [9. ReplicaSet](#9-replicaset)
    - [10. StatefulSet](#10-statefulset)
    - [11. DaemonSet](#11-daemonset)
    - [12. Ingress](#12-ingress)
    - [13. Horizontal Pod Autoscaler (HPA)](#13-horizontal-pod-autoscaler-hpa)
  - [Key Components](#key-components)
    - [1. Control Plane Components](#1-control-plane-components)
    - [2. Node Components](#2-node-components)
  - [Ksy Concepts vs Components](#ksy-concepts-vs-components)
  - [K8s using Docker Desktop](#k8s-using-docker-desktop)
    - [What is it?](#what-is-it)
    - [Enabling Kubernetes](#enabling-kubernetes)
    - [How It Works Under the Hood](#how-it-works-under-the-hood)
    - [Mac HyperKit Hypervisor](#mac-hyperkit-hypervisor)
  - [K8s Commands](#k8s-commands)

## What is Kubernetes?

- Kubernetes, or `K8s`, is an open-source orchestration system for automating deployment, scaling, and management of containerized applications.
- It manages clusters of servers and runs containers across them, balancing the load.

## [Key Concepts](https://kubernetes.io/docs/concepts/overview/)

### 1. Cluster

- A set of machines called nodes that run containerized applications.
- Contains both control plane nodes (master) and worker nodes.

### 2. Node

- A physical or virtual machine that runs the containers.
- Managed by the control plane.

### 3. Pod

- Smallest deployable unit in Kubernetes.
- Contains one or more containers.

### 4. Service

- Exposes pods to the network, either internally within the cluster or externally.
- Handles load balancing across multiple pods.

### 5. Deployment

- Defines the desired state of an application.
- Manages the scaling and updating of pods.

### 6. ConfigMap & Secret

- `ConfigMap`: Manages configuration data as key-value pairs.
- `Secret`: Manages sensitive information, like passwords.

### 7. Volume

- Provides storage to containers within a pod.
- Can be persistent across pod restarts.

### 8. Namespace

- Logical separation within a cluster.
- Allows for organization and isolation of resources.

### 9. ReplicaSet

- Ensures the specified number of replicas for a pod are running.
- Managed by deployments.

### 10. StatefulSet

- Manages stateful applications, maintaining unique identities and states across pods.

### 11. DaemonSet

- Ensures a copy of a pod runs on specific or all nodes in the cluster.

### 12. Ingress

- Manages external access to services within the cluster.
- Provides routing and load balancing.

### 13. Horizontal Pod Autoscaler (HPA)

- Automatically scales the number of pods based on observed metrics like CPU usage.

## [Key Components](https://kubernetes.io/docs/concepts/overview/components/)

### 1. Control Plane Components

- **kube-apiserver:** Exposes the Kubernetes API.
- **etcd:** Consistent and highly-available key-value store for all cluster data.
- **kube-scheduler:** Schedules pods to run on nodes.
- **kube-controller-manager:** Runs controllers for nodes, replicas, endpoints, etc.
- **cloud-controller-manager:** Runs controllers specific to the underlying cloud provider.

### 2. Node Components

- **kubelet:** Ensures that containers are running in a pod.
- **kube-proxy:** Manages network rules and enables communication to and from your pods.
- **Container Runtime:** Software for running containers (e.g., Docker, containerd).

## Ksy Concepts vs Components

- The `key concepts` are the fundamental building blocks you work with when defining and interacting with your applications in Kubernetes
- The `key components` are the underlying mechanisms that make Kubernetes work as a system
- Both are crucial to understanding how to effectively work with Kubernetes

## K8s using Docker Desktop

### What is it?

- Docker Desktop for Mac includes a standalone Kubernetes server that runs on your Mac.

### Enabling Kubernetes

- **Access Preferences:** Go to the Preferences/Settings menu in Docker Desktop.
- **Enable Kubernetes:** There's an option to enable Kubernetes, which will install the Kubernetes components necessary to run a single-node cluster locally.
- **Configure (Optional):** You can customize the Kubernetes version and other settings if needed.
- **Apply & Restart:** Click the "Apply & Restart" button, and Docker Desktop will set up a local Kubernetes cluster for you.

### How It Works Under the Hood

- **Kubernetes Inside Docker:** Docker Desktop runs the Kubernetes control plane and worker components inside Docker containers.
- **Shared Resources:** It shares system resources with Docker, so you don't need a separate VM for Kubernetes.
- **Integration with Docker CLI:** It integrates with your Docker CLI, allowing you to use `kubectl` commands just as you would with a remote cluster.
- **Local Registry:** Supports a local registry, allowing for easier image sharing and testing.
- **Networking:** It sets up networking so that you can communicate with your cluster using `localhost`.

### Mac HyperKit Hypervisor

- **Hypervisor.framework:**
  - Docker Desktop uses macOS's Hypervisor.framework (*through HyperKit*) to run a lightweight Linux VM
  - This is where your Docker containers (and Kubernetes) are actually running
  - It is a lightweight, native macOS hypervisor based on xhyve. It's bundled with Docker Desktop

- **Port Forwarding:** Docker Desktop handles port forwarding from the VM to your macOS system. When a service in a container inside the VM listens on a port, Docker Desktop forwards traffic from that port on `localhost` to the corresponding port on the VM.
- **Kubernetes Services:** When you expose a Kubernetes service on a particular port, it's accessible at that port on `localhost` in your Mac’s browser or other networking tools.

## K8s Commands

| Command                        | Description                                                  | Example                                           |
|--------------------------------|--------------------------------------------------------------|---------------------------------------------------|
| `kubectl get <resource>`       | List resources like pods, services, etc.                      | `kubectl get pods`                                |
| `kubectl describe <resource>`  | Show detailed information about a resource.                   | `kubectl describe pod my-pod`                     |
| `kubectl create <resource>`    | Create a resource from a file or stdin.                       | `kubectl create -f my-deployment.yaml`            |
| `kubectl delete <resource>`    | Delete resources by filenames, stdin, or resource names.      | `kubectl delete pod my-pod`                       |
| `kubectl apply <resource>`     | Apply changes to resources from a file or stdin.              | `kubectl apply -f my-deployment.yaml`             |
| `kubectl exec`                 | Execute a command in a container.                             | `kubectl exec my-pod -- ls /`                     |
| `kubectl logs`                 | Print the logs for a container in a pod.                      | `kubectl logs my-pod`                             |
| `kubectl scale`                | Scale the replicas of a deployment or replicaset.              | `kubectl scale --replicas=5 deployment/my-app`    |
| `kubectl port-forward`         | Forward one or more local ports to a pod.                     | `kubectl port-forward my-pod 8080:8080`           |
| `kubectl rollout`              | Manage the rollout of a resource.                              | `kubectl rollout undo deployment/my-deployment`   |
| `kubectl label`                | Update or add labels to a resource.                            | `kubectl label pods my-pod status=unhealthy`      |
| `kubectl annotate`             | Add or update annotations to a resource.                       | `kubectl annotate pods my-pod description="test"` |
| `kubectl config`               | Modify kubeconfig files.                                       | `kubectl config set-context my-context`           |
| `kubectl cluster-info`         | Display cluster info.                                          | `kubectl cluster-info`                            |
| `kubectl top`                  | Display resource usage (CPU/Memory) for nodes or pods.         | `kubectl top nodes`                               |

These commands cover most of the everyday tasks you would perform with Kubernetes, from managing resources to viewing logs and scaling applications. Remember to replace `<resource>` with the actual resource type and name you want to interact with!
