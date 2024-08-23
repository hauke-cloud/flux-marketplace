## Step 1: Create a Flux v2 GitRepository

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

## Step 2: Create a Flux v2 Kustomization

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

## Conclusion

With these two manifests in place, Flux will now track the `hauke-cloud/flux-marketplace` repository and automatically apply any changes made to the `apps/demo` directory. Ensure that your `flux-system` namespace is properly set up and that your Flux installation is running correctly.

If you encounter any issues, feel free to open an issue or contact support.
