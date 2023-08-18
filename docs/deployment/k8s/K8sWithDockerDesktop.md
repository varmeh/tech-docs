# Kubernetes with Docker Desktop

- [Kubernetes with Docker Desktop](#kubernetes-with-docker-desktop)
  - [What is it?](#what-is-it)
  - [Enabling Kubernetes](#enabling-kubernetes)
  - [How It Works Under the Hood](#how-it-works-under-the-hood)
  - [Mac HyperKit Hypervisor](#mac-hyperkit-hypervisor)

## What is it?

- Docker Desktop for Mac includes a standalone Kubernetes server that runs on your Mac.

## Enabling Kubernetes

- `Access Preferences:` Go to the Preferences/Settings menu in Docker Desktop.
- `Enable Kubernetes:` There's an option to enable Kubernetes, which will install the Kubernetes components necessary to run a single-node cluster locally.
- `Configure (Optional):` You can customize the Kubernetes version and other settings if needed.
- `Apply & Restart:` Click the "Apply & Restart" button, and Docker Desktop will set up a local Kubernetes cluster for you.

## How It Works Under the Hood

- `Kubernetes Inside Docker:` Docker Desktop runs the Kubernetes control plane and worker components inside Docker containers.
- `Shared Resources:` It shares system resources with Docker, so you don't need a separate VM for Kubernetes.
- `Integration with Docker CLI:` It integrates with your Docker CLI, allowing you to use `kubectl` commands just as you would with a remote cluster.
- `Local Registry:` Supports a local registry, allowing for easier image sharing and testing.
- `Networking:` It sets up networking so that you can communicate with your cluster using `localhost`.

## Mac HyperKit Hypervisor

- `Hypervisor.framework:`
  - Docker Desktop uses macOS's Hypervisor.framework (*through HyperKit*) to run a lightweight Linux VM
  - This is where your Docker containers (and Kubernetes) are actually running
  - It is a lightweight, native macOS hypervisor based on xhyve. It's bundled with Docker Desktop

- `Port Forwarding:` Docker Desktop handles port forwarding from the VM to your macOS system. When a service in a container inside the VM listens on a port, Docker Desktop forwards traffic from that port on `localhost` to the corresponding port on the VM.
- `Kubernetes Services:` When you expose a Kubernetes service on a particular port, it's accessible at that port on `localhost` in your Mac’s browser or other networking tools.
