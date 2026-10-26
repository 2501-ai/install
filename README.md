# 2501 installation files

Each [release](https://github.com/2501-ai/install/releases) here is one 2501 build, `X.Y.Z-rN`. Its files are generated from the 2501 Helm chart and signed by 2501. Install from the files of the build you run.

| You run | Files | Command |
|---|---|---|
| Helm or ArgoCD | `2501-<build>.tgz`, `values.example.yaml` | `helm upgrade --install ai2501 2501-<build>.tgz -f values.yaml` |
| Kubernetes without Helm | `kubernetes.yaml`, `settings.example.yaml`, the Ingress example | `kubectl apply -f kubernetes.yaml` |
| OpenShift without Helm | `openshift.yaml`, `settings.example.yaml`, the Route example | `oc apply -f openshift.yaml` |
| Docker Swarm | `compose.yaml`, `.env.example` | `set -a; . ./.env; set +a; docker stack deploy -c compose.yaml ai-2501` |

On Kubernetes and OpenShift, your settings live in the `2501-settings` ConfigMap, created once from `settings.example.yaml`, and your Route or Ingress is yours too: an upgrade applies the new manifests and leaves them as they are. On Swarm, they live in your `.env`.

## Verify a file

Every file comes with a `.bundle`. Check it before you apply the file:

```sh
cosign verify-blob --key https://keys.2501.ai/cosign.pub --insecure-ignore-tlog=true \
  --bundle compose.yaml.bundle compose.yaml
```

The images these files name are pulled with the registry access 2501 gives you.
