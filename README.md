<!-- llm-readme-management spec=1 commit=33dc2e7ce234869210d74e634031b2d24c455987 template=default model=qwen3.6-35b-a3b digest=598d66067ca0 generated=2026-09-08T19:26:52Z -->
<a href="https://hauke.cloud" target="_blank"><img src="https://img.shields.io/badge/home-hauke.cloud-brightgreen" alt="hauke.cloud" style="display: block;" /></a>
<a href="https://github.com/hauke-cloud" target="_blank"><img src="https://img.shields.io/badge/github-hauke.cloud-blue" alt="hauke.cloud Github Organisation" style="display: block;" /></a>
<a href="https://github.com/hauke-cloud/llm-readme-management" target="_blank"><img src="https://img.shields.io/badge/template-default-orange" alt="Repository type - default" style="display: block;" /></a>


# Flux Marketplace


<img src="https://raw.githubusercontent.com/hauke-cloud/.github/main/resources/img/organisation-logo-small.png" alt="hauke.cloud logo" width="109" height="123" align="right">


<llm header>

This repository provides a curated collection of Flux CD YAML manifests that manage pre-configured application deployments and operators on Kubernetes. You can apply these HelmRelease, HelmRepository, and Namespace resources to bootstrap infrastructure like Longhorn or services such as Keycloak directly into your cluster. It is designed for GitOps operators and platform engineers running Flux v2 who need automated rollouts.

</llm>


## :book: Description

<llm description>

This repository provides a curated collection of Kubernetes manifests for GitOps operators running Flux CD v2 on Kubernetes. Within the `hauke-cloud` organisation, it serves as the central manifest source that your Flux installation tracks to provision shared infrastructure and applications. Instead of writing deployment configurations from scratch, you bootstrap a complete stack by Kustomizing over these directories. The manifests declare `HelmRepository` and `HelmRelease` resources that track charts from the `ghcr.io/hauke-cloud/charts` OCI registry and public sources, enabling automatic version reconciliation across your cluster.

- Declares Flux `HelmRepository` resources for OCI registries and third-party chart sources.
- Defines `HelmRelease` resources with automatic version reconciliation and configurable intervals.
- Creates or references Kubernetes Namespaces for each application stack.
- Installs operators via OLM Subscriptions, including Keycloak, PostgreSQL, and Mattermost.
- Publishes custom default values for storage engines, tolerations, and monitoring configurations.

</llm>


## 🚀 Getting started

<llm getting_started hint="Assume nothing about the ecosystem beyond what the analysis names. If the repository has no build step, say what a reader does with it instead.">

1. Clone the repository and enter the directory.
```bash
git clone https://github.com/hauke-cloud/flux-marketplace.git
cd flux-marketplace
```

2. Install pre-commit hooks to enforce quality gates on your local machine.
```bash
pre-commit install
```

3. Execute the pre-commit hooks manually across all files to validate the manifests.
```bash
pre-commit run --all-files
```

Once these steps complete, you have a validated collection of Kubernetes manifests ready to be applied to your cluster.

</llm>


## :airplane: Usage

<llm usage>

You consume this repository by applying the manifests for specific applications, such as `mqtt-device-manager`, located under `apps/`. The namespace definitions use a `${namespace}` placeholder that you must substitute with your target namespace, typically using Kustomize to replace the string before applying the resources.

```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - apps/mqtt-device-manager/namespace.yaml
  - apps/mqtt-device-manager/helmrepository.yaml
  - apps/mqtt-device-manager/helmrelease.yaml
patches:
  - target:
      kind: Namespace
    patch: |-
      - op: replace
        path: /metadata/name
        value: mqtt
```

You customize application behavior by providing values through ConfigMaps referenced in the `valuesFrom` list of each HelmRelease. The repository expects specific ConfigMap names for each service, such as `helm-values-mqtt-device-manager`. You create this ConfigMap with your desired configuration overrides to inject into the deployment.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: helm-values-mqtt-device-manager
data:
  values.yaml: |
    replicaCount: 2
```

</llm>


## 📄 License

This Project is licensed under the GNU General Public License v3.0

- see the [LICENSE](LICENSE) file for details.


## :coffee: Contributing

To become a contributor, please check out the [CONTRIBUTING](CONTRIBUTING.md) file.


## :email: Contact

For any inquiries or support requests, please open an issue in this
repository or contact us at [contact@hauke.cloud](mailto:contact@hauke.cloud).
