
<p align="center">
  <img src="resources/img/logo.png" alt="repository logo" width="50%" height="50%">
</p>


# Flux Marketplace

<div style="float: right;">
  <div>
    <img src="https://raw.githubusercontent.com/hauke-cloud/.github/main/resources/img/organisation-logo-small.png" alt="hauke.cloud logo" width="109" height="123">
    <div style="margin-top: 10px;">
      <a href="https://hauke.cloud" target="_blank">
        <img src="https://img.shields.io/badge/home-hauke.cloud-brightgreen" alt="hauke.cloud" />
      </a>
      <a href="https://github.com/hauke-cloud" target="_blank">
        <img src="https://img.shields.io/badge/github-hauke.cloud-blue" alt="hauke.cloud Github Organisation" style="display: block; margin-top: 5px;"/>
      </a>
    </div>
  </div>
</div>

<p>
This repository provides all the Helm/OCI/Kustomize deployments as Flux
manifests, which [hauke.cloud](https://hauke.cloud) also uses.

- We keep the versions up-to-date
- The deployments are tested in our development and staging environments
- We set (in our opinion) good, generally valid default values
- Of course, these deployments are somewhat tailored to our needs, but
  we try to keep them as generally valid as possible
- Missing components (such as secrets) are documented
- Here you can find example deployments of applications we develop and maintain

</p>

## Table of Contents

- [Getting started](#-getting-started)
- [Usage](#-usage)
- [License](#license)
- [Contributing](#contributing)
- [Contact](#contact)

## 🚀 Getting started
Before you begin, ensure you have the following:

- A Kubernetes cluster
- [kubectl](https://kubernetes.io/docs/tasks/tools/) installed and configured to interact with your cluster
- The [fluxv2](https://github.com/fluxcd/flux2) operator installed
- Flux CLI installed [fluxv2-cli](https://github.com/fluxcd/flux2)


## :wrench: Configuration

## :airplane: Usage
To quickly install the demo app, simply follow these steps. Please note that this does not involve any changes/adaptations to your requirements.

### Step 1: Create a Flux v2 GitRepository

The GitRepository manifest will instruct Flux to track this GitHub repository.

1. Create a new file named `flux-marketplace-gitrepository.yaml`:

   ```yaml
   apiVersion: source.toolkit.fluxcd.io/v1beta2
   kind: GitRepository
   metadata:
     name: flux-marketplace
     namespace: flux-system
   spec:
     interval: 1m
     url: https://github.com/hauke-cloud/flux-marketplace
     ref:
       branch: main
   ```

2. Apply the manifest to your cluster:

   ```bash
   kubectl apply -f flux-marketplace-gitrepository.yaml
   ```

### Step 2: Create a Flux v2 Kustomization

The Kustomization manifest will apply the resources found in the "apps/demo" directory of the repository.

1. Create a new file named `flux-marketplace-kustomization.yaml`:

   ```yaml
   apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
   kind: Kustomization
   metadata:
     name: demo-apps
     namespace: flux-system
   spec:
     interval: 10m
     path: "./apps/demo"
     prune: true
     sourceRef:
       kind: GitRepository
       name: flux-marketplace
     validation: client
   ```

2. Apply the manifest to your cluster:

   ```bash
   kubectl apply -f flux-marketplace-kustomization.yaml
   ```

### Conclusion

With these two manifests in place, Flux will now track the `hauke-cloud/flux-marketplace` repository and automatically apply any changes made to the `apps/demo` directory. Ensure that your `flux-system` namespace is properly set up and that your Flux installation is running correctly.

If you encounter any issues, feel free to open an issue or contact support.


## 📄 License

This Project is licensed under the GNU General Public License v3.0

- see the [LICENSE](LICENSE) file for details.

## :coffee: Contributing

To become a contributor, please check out the [CONTRIBUTING](CONTRIBUTING.md) file.
## :email: Contact

For any inquiries or support requests, please open an issue in this
repository or contact us at [contact@hauke.cloud](mailto:contact@hauke.cloud).
