# K8s Commands

- [K8s Commands](#k8s-commands)
  - [Types of Commands](#types-of-commands)
  - [Imperative Commands](#imperative-commands)
  - [Declarative Commands](#declarative-commands)
  - [Comparison](#comparison)

## Types of Commands

- Kubernetes supports `imperative` and `declarative` approaches for managing resources

## Imperative Commands

- You're explicitly telling the cluster what to do, without describing the complete desired state.
- It's like giving step-by-step directions.

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

- These commands cover most of the everyday tasks you would perform with Kubernetes, from managing resources to viewing logs and scaling applications.
- Remember to replace `<resource>` with the actual resource type and name you want to interact with!

## Declarative Commands

- You're providing the desired state in a file, and Kubernetes figures out how to get there.
- It's like providing a destination, and letting the system decide the path.
- The declarative approach typically revolves around using the `kubectl apply` command with various manifest files.

| Command                            | Description                                                           |
|------------------------------------|-----------------------------------------------------------------------|
| `kubectl apply -f <filename.yaml>` | Apply the configuration in `<filename.yaml>` to the resource.         |
| `kubectl apply -f <directory/>`    | Apply the configurations in all YAML or JSON files within a directory.|
| `kubectl apply -k <kustomization>` | Apply a kustomization directory.                                      |
| `kubectl delete -f <filename.yaml>`| Delete the resources defined in `<filename.yaml>`.                     |
| `kubectl diff -f <filename.yaml>`  | Diff the local resource with the running configuration.               |
| `kubectl replace -f <filename.yaml>`| Replace the resource with the configuration in `<filename.yaml>`.      |

- This approach ensures the configuration files are the source of truth, and the desired state is maintained.
- The declarative approach is preferred for:
  - Managing complex configuration
  - Maintaining consistency across environments
  - Leveraging version control practices

## Comparison

| Aspect                   | Imperative Commands                                       | Declarative Commands                                          |
|--------------------------|-----------------------------------------------------------|----------------------------------------------------------------|
| **Definition**           | Commands that manage resources by direct action.          | Commands that manage resources based on manifest files.        |
| **Usage**                | Utilized for one-off tasks, quick edits, or experimentation. | Used for maintaining the desired state as defined in files.    |
| **Command Examples**     | `kubectl create`, `kubectl run`, `kubectl expose`         | `kubectl apply -f <filename.yaml>`                             |
| **File Requirement**     | No YAML or JSON file required.                            | Requires YAML or JSON file describing the desired state.       |
| **Flexibility**          | Limited in complex scenarios.                             | More flexible and can handle complex configurations.           |
| **State Tracking**       | No automatic tracking of the current state.               | Can track the current state and make updates as needed.        |
| **Recommended For**      | Ad-hoc actions and prototyping.                           | Production environments, version control, and collaboration.   |
