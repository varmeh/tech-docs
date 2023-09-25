# Helm

- [Helm](#helm)
  - [Why do we need Helm?](#why-do-we-need-helm)
    - [The Complexity of Kubernetes](#the-complexity-of-kubernetes)
    - [Enter Helm](#enter-helm)
    - [Illustrative Example](#illustrative-example)
  - [Pre-requisites](#pre-requisites)
  - [What is Helm?](#what-is-helm)
  - [Helm Functionalities](#helm-functionalities)
    - [Core Functionalities](#core-functionalities)
    - [Helm 3 Specific Features](#helm-3-specific-features)
  - [Steps to Get Started with Helm](#steps-to-get-started-with-helm)
    - [Install Helm](#install-helm)
    - [Add Helm Repositories](#add-helm-repositories)
    - [Searching for Charts](#searching-for-charts)
    - [Installing a Chart](#installing-a-chart)
    - [List Installed Releases](#list-installed-releases)
    - [Customizing a Chart Before Installing](#customizing-a-chart-before-installing)
    - [Uninstalling a Release](#uninstalling-a-release)
    - [Creating Your Own Chart](#creating-your-own-chart)
    - [Packaging and Sharing Your Chart](#packaging-and-sharing-your-chart)
    - [Helm Get Chart Manifests](#helm-get-chart-manifests)
    - [Decode Helm Revision Information Stored in secrets](#decode-helm-revision-information-stored-in-secrets)
    - [Helm Context](#helm-context)
      - [What is Helm Context?](#what-is-helm-context)
      - [How to Access Context Data?](#how-to-access-context-data)
    - [Helm Hooks](#helm-hooks)
  - [References](#references)

## Why do we need Helm?

Yes, that's a concise way to describe it. Helm 3 is essentially a package manager for Kubernetes applications, enabling users to define, install, and upgrade even the most complex Kubernetes applications efficiently.

When you describe Helm 3 as a "wrapper," you're highlighting its role in abstracting the complexities of individual Kubernetes manifest files and streamlining the process of deploying and managing applications.

**Why Helm is likened to a wrapper for release management:**

In essence, while `kubectl` provides the means to interact with a Kubernetes cluster at a granular level, Helm simplifies the management and deployment of applications on Kubernetes, especially for complex applications. So, considering Helm 3 as a "wrapper" for effective release management in Kubernetes is pretty apt.

### The Complexity of Kubernetes

Kubernetes is incredibly powerful. It orchestrates containerized applications, ensuring they run smoothly, scale, and self-heal. However, `with power comes complexity`:

1. `Multiple Configuration Files`:
   - Deploying even a simple application requires numerous YAML files:
     - Deployment
     - Service
     - PersistentVolume
     - PersistentVolumeClaim
     - And possibly ConfigMaps or Secrets
   - *Order of deployment is important*. For instance, a Deployment can't be created before a Service.
2. `Updates & Rollbacks`:
   - Deploying updates or rolling back to a previous version isn't always straightforward.
   - Each change requires careful tweaking of YAML files.
3. `Sharing & Reusability`: Sharing an application setup with others isn't easy. Packaging all required configurations and ensuring they are applied in the right order is cumbersome.

### Enter Helm

Consider Helm as a wrapper for Kubernetes applications. It provides a set of functionalities that make it easier to manage Kubernetes applications:

- `Templates and Values`: Helm charts use templates to generate Kubernetes manifest files. This allows for parameterization, so one chart can be used to deploy to different environments or configurations without changing the core application definition.
- `Versioning`: Helm supports versioning of application releases, allowing users to roll back to a previous version if needed.
- `Dependencies`: Helm can manage dependencies for applications. For instance, if your application requires a specific database, Helm can ensure that database is deployed alongside your application.
- `Atomic Deployments`: Helm tries to ensure that application deployments are atomic. This means either all resources in the application are successfully deployed, or none are, minimizing half-baked deployments.
- `Releases`: With Helm, each time you deploy a chart, it's called a "release". Helm keeps track of these releases, allowing you to manage your deployments effectively.
- `Repository`: Helm has a concept of chart repositories, which are like artifact repositories, but for Helm charts. This allows teams to share and distribute applications easily.

### Illustrative Example

Consider deploying a basic `Node.js` application with a connected database:

- `Without Helm`:
  - Write a Deployment YAML for the Node.js app.
  - Write a Service YAML to expose the app.
  - Set up a PersistentVolume and PersistentVolumeClaim for the database.
  - Write Deployment and Service YAMLs for the database.
  - If the setup changes, manually tweak each YAML.

- `With Helm`:
  - Use an existing Node.js chart or create one.
  - Define values in `values.yaml` like image location, service type, database details.
  - Deploy using `helm install`, which deploys all necessary components.
  - Update using `helm upgrade` with a different `values.yaml` for staging or production.

## Pre-requisites

- A running Kubernetes cluster.
- `kubectl` installed and configured.
- Basic knowledge of Kubernetes.

## What is Helm?

- `Helm` is a package manager for Kubernetes.
- It allows developers and operators to package applications and deploy them on Kubernetes clusters.
- Helm is primarily a `client-side` tool, especially in its v3 incarnation.
- It communicates with the Kubernetes API server to perform its actions.
- It interacts with the `currently active Kubernetes context`, so it's important to set the right context before using Helm.
- Select context using `kubectl config use-context <context-name>`.

## Helm Functionalities

Certainly! Helm 3 brought about a range of functionalities and improvements over its predecessor. Here's a breakdown of what Helm 3 offers:

### Core Functionalities

1. `Chart Management`:
    - Develop and package Kubernetes charts.
    - Search for charts in `Helm Hub` or from configured repositories.
    - Retrieve and view chart details.

2. `Release Management`:
    - Install charts into Kubernetes to create a new release.
    - Upgrade, roll back, or uninstall releases.
    - List, get, and check the history of releases.

3. `Repository Management`:
    - Add, list, or remove chart repositories.
    - Update repositories to refresh available charts.

4. `Plugin Management`:
    - Helm has a powerful plugin system that allows extending its functionalities.
    - Install, list, or uninstall plugins.

### Helm 3 Specific Features

1. `Removal of Tiller`:
    - No need for a server-side component.
    - Utilizes native Kubernetes API calls for operations.

2. `Release Namespaces`:
    - Releases are now namespace-scoped, meaning the same release name can be used in different namespaces.

3. `Three-way Strategic Merge Patches`:
    - When upgrading or rolling back, Helm tries to make the minimal edits instead of recreating resources from scratch. This is achieved by comparing the old manifest, the new manifest, and the live state of the object.

4. `Chart Dependencies`:
    - Dependencies are maintained in the `Chart.yaml` file, and `charts/` directory can have the packaged charts.
    - `helm dependency` command to manage dependencies.

5. `Library Charts`:
    - These are reusable charts without any application-specific content, allowing users to share and reuse snippets of code.

6. `Improved CRD Support`:
    - Helm now has better support for installing and managing Kubernetes Custom Resource Definitions (CRDs).

7. `OCI Integration`:
    - Experimental feature to use Helm with the Open Container Initiative for package distribution.

8. `JSON Schema Chart Validation`:
    - Allows chart maintainers to provide JSON schema for their chart's values. This ensures that the provided values match the expected format.

9. `Consistent Label System`:
    - Helm now has a consistent set of labels that are applied to all resources.

## Steps to Get Started with Helm

### Install Helm

- For Mac: `brew install helm`

### Add Helm Repositories

To fetch charts (packages) from the community:

```bash
helm repo list
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add stable https://charts.helm.sh/stable
helm repo update
helm repo list
```

### Searching for Charts

```bash
helm search repo bitnami
```

### Installing a Chart

For instance, let's install the `nginx` server:

```bash
helm install my-nginx stable/nginx-ingress
```

This command installs the `nginx-ingress` chart with the release name `my-nginx`.

### List Installed Releases

```bash
helm list
```

### Customizing a Chart Before Installing

Charts often have configurable parameters. To customize these:

a. Download default values:

```bash
helm show values stable/nginx-ingress > values.yaml
```

b. Modify `values.yaml` using any text editor.

c. Install/Upgrade the chart using:

```bash
helm install my-nginx stable/nginx-ingress -f values.yaml
```

OR for upgrading:

```bash
helm upgrade my-nginx stable/nginx-ingress -f values.yaml
```

### Uninstalling a Release

```bash
helm uninstall my-nginx
```

### Creating Your Own Chart

To scaffold a new chart:

```bash
helm create my-app
```

This creates a new directory called `my-app` with the structure of a typical Helm chart.

### Packaging and Sharing Your Chart

Once your application is defined within a chart, you can package it into a `.tgz` file:

```bash
helm package my-app
```

### [Helm Get Chart Manifests](https://www.udemy.com/course/helm-kubernetes-packaging-manager-for-developers-and-devops/learn/lecture/28400456#overview)

- To get the manifest files of a chart.
- A manifest is a YAML-encoded representation of the Kubernetes resources that were generated from this release's chart(s).
- If a chart is dependent on other charts, those resources will also be included in the manifest.

```bash
helm get manifest my-nginx
```

### [Decode Helm Revision Information Stored in secrets](https://gist.github.com/DzeryCZ/c4adf39d4a1a99ae6e594a183628eaee)

- Helm saves in revision information in the form of secrets.
- To get revision information, use command:

```bash
kubectl get secrets
```

- It would show a list of secrets. Helm Secrets would have name like `sh.helm.release.v1.<release-name>.v<revision-number>`.
- You could check internals using command:

```bash
kubectl get secret sh.helm.release.v1.my-nginx.v1 -o json
```

- In the output, you would see a key `data.release` with base64 encoded value which holds the revision information.

- This information is tripple encoded
  - base64 decode - `Kubernetes secrets encoding`
  - base64 decode (again) - `Helm encoding`
  - gzip decompress - `Helm zipping`

- To decode this information, use command:

```bash
kubectl get secrets sh.helm.release.v1.my-nginx.v1 -o jsonpath='{.data.release}' | base64 -D | base64 -D | gzip -d > dump.json
code dump.json
```

---

### Helm Context

#### What is Helm Context?

- In Helm, the term `context` often refers to the set of data available to templates as they're being evaluated.
- The context provides a structured way to access data that the template might need.

| Context Component | Description                                                   | Example Keys/Values                               |
|--------------------|---------------------------------------------------------------|---------------------------------------------------|
| **Values**         | User-supplied configuration for the chart.                    | Any value specified in `values.yaml` or via CLI.  |
| **Release**        | Information about the release itself.                        | `Release.Name`, `Release.Namespace`, `Release.IsUpgrade`, `Release.Revision`, `Release.Service` |
| **Chart**          | Information about the chart being deployed.                  | `Chart.Name`, `Chart.Version`, `Chart.AppVersion` |
| **Files**          | Access to files packaged into the chart.                     | `Files.Get`, `Files.GetBytes`                    |
| **Capabilities**   | Info about the capabilities of the Kubernetes cluster.       | Provides capabilities like `.Capabilities.APIVersions.Has` to check for available APIs. |
| **Template**       | Information about the current template being executed.       | `Template.Name`, `Template.BasePath`              |

#### How to Access Context Data?

- The context data is accessed using the `.` operator.
- For instance, to access the `Release.Name` value, you can use `.Release.Name`.
- To check all the values available in the context, follow the steps below:

- `debug.yaml file`:
  - Create the a file called `debug.yaml` in the `templates/` directory of your chart
  - Add the following to your `debug.yaml`:

{% raw %}

```yaml
  {{- if .Values.debug.enabled -}}
  {{- . | toYaml -}}
  {{- end -}}
```

{% endraw %}

- `Add debug.enabled Variable` to values.yaml:
  - By `default`, this will ensure the `debug.yaml` content is not rendered

```yaml
debug:
  enabled: false
```
  
- `Overrideing debug.enabled`: to check the values during debug, override the `debug.enabled` value to `true`:

```bash
helm template [RELEASE_NAME] [CHART] --set debug.enabled=true > output.yaml
```

### Helm Hooks

| Hook Phase       | Purpose & Use Cases | Examples  |
|------------------|---------------------|-----------|
| Pre-installation | Run before any release resources are installed.            | Initializing a database, setting up infrastructure.  |
| Post-installation| Execute once all resources in the release are installed.   | Notify an external system about a new release.   |
| Pre-upgrade      | Handle tasks before the actual upgrade starts.            | Running data migration scripts, backup operations before an upgrade.                       |
| Post-upgrade     | Manage operations after the upgrade process is done.      | Reinitializing state, clearing caches, notifying monitoring tools about the upgrade.       |
| Pre-rollback     | Conduct tasks before a release is rolled back to its previous version.  | Taking snapshots, storing logs before rollback. |
| Post-rollback    | Handle operations after a release is successfully rolled back. | Restoring from pre-rollback snapshots.           |
| Pre-delete       | Handle cleanup or other tasks before release resources are deleted.   | Removing dynamic resources created by the release, notifying other services about deletion.|
| Post-delete      | Run tasks after the release's resources are deleted.      | Verifying resource deletion, freeing up external resources.                                |
| CRD Hooks        | Handle Custom Resource Definitions (CRDs) and ensure they're set up before other parts of the chart. | Ensuring CRDs are installed or upgraded before other charts. |
| Test Hooks       | Validate if a release or upgrade is successful.            | Running tests associated with a release. If tests fail, the release is marked as failed.   |

## References

- [Helm Concepts Explained](https://www.youtube.com/watch?v=kJscDZfHXrQ&ab_channel=KodeKloud)
- [Advantages of Helm](https://www.udemy.com/course/helm-kubernetes-packaging-manager-for-developers-and-devops/learn/lecture/28719560#overview)
- [Helm Charts](https://www.udemy.com/course/helm-kubernetes-packaging-manager-for-developers-and-devops/learn/lecture/28687720#overview)
- [Helm Revision History using K8s Secrets](https://www.udemy.com/course/helm-kubernetes-packaging-manager-for-developers-and-devops/learn/lecture/28368340#overview)
