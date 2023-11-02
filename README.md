# qcon-service-mesh-for-less
This repo contains the instructions for trying out the Istio Ambient mode setup discussed in "Service Mesh for Less" at QCon San Francisco

### Prerequisites
- `yq` - `brew install yq` or see here (https://mikefarah.gitbook.io/yq/v/v3.x/)
- `k3d` - `brew install k3d` or see here (https://k3d.io/v5.4.7/#installation)
- `kubectl` - `brew install kubectl` or see here (https://kubernetes.io/docs/tasks/tools/#kubectl)
- Kubernetes cluster with enough resources to run the 100 namespace app
    - Tested with Istio Ambient: 17x e2-standard-8 instances / 1.25.10-gke.2700
    - Tested with Istio Ambient + Waypoint Proxy: 18x e2-standard-8 instances / 1.25.10-gke.2700
    - Tested with default Istio (sidecar): 30x e2-standard-8 / 1.25.10-gke.2700


#### Renaming Cluster Context
If your local clusters have a different context name, you will want to have it match the expected context name(s) `ambient-demo` and `sidecar-demo` respectively
```
kubectl config rename-context <your_cluster_name> ambient-demo

kubectl config rename-context <your_cluster_name> sidecar-demo
```

## To deploy
```
./aoa-tools/deploy.sh deploy -f environments/istio/ambient

./aoa-tools/deploy.sh deploy -f environments/istio/sidecar
```

### Installer options
```
Syntax: installer [-f|-i|-h]

commands:
deploy     deploys an environment with the specified path
destroy    destroys an environment with the specified path

options:
-f     path to environment files
-i     install infra
-h     print help
```

Notes on defaults: 
- If `-i` is used, the installer will check for a folder named `.infra` in the environment directory and will install the infra before running the script. This currently only supports `k3d` for local deployments
- If the overlay is not specified by passing the `-o` flag, the installer will default to `base`. This flag is useful for example to pass in the `-o m1` to use the `m1` overlays containing ARM based images

### vars.env
The `vars.env` exists in each demo environment directory and the specified variables are treated as the source of truth for the installation. The installer will use any passed in flags and attempt to discover all of the necessary variables in the pre-check. Please verify the output before continuing.

Note: All variables present in the `vars.env` will be exported and available everywhere for continued use

