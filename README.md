# 2501 installation files

Each [release](https://github.com/2501-ai/install/releases) here is one 2501 build, `X.Y.Z-rN`. Its files are generated from the 2501 Helm chart and signed by 2501. Install from the files of the build you run.

| You run | Files | Command |
|---|---|---|
| Helm or ArgoCD | `2501-<build>.tgz`, `values.example.yaml` | `helm upgrade --install ai2501 2501-<build>.tgz -f values.yaml` |
| Kubernetes without Helm | `kubernetes.yaml`, `kubernetes.settings.example.yaml`, `kubernetes.ingress.example.yaml` | `kubectl apply -f kubernetes.yaml -n <namespace>` |
| OpenShift without Helm | `openshift.yaml`, `openshift.settings.example.yaml`, `openshift.ingress.example.yaml` | `oc apply -f openshift.yaml -n <namespace>` |
| Docker Swarm | `compose.yaml`, `env.example` | `set -a; . ./.env; set +a; docker stack deploy -c compose.yaml ai-2501` |

On Kubernetes and OpenShift, you create once the `2501-settings` ConfigMap from the settings example, your Secrets and your Ingress (OpenShift turns it into a Route). On Swarm, you copy `env.example` to `.env` and fill it. An upgrade applies the files of the new build and leaves your own objects and `.env` as they are.

Every build has its own files: they name the exact images of that build, so an upgrade always takes the new files. `https://github.com/2501-ai/install/releases/latest/download/<file>` always points to the newest build.

## Verify a file

Every file comes with a `.bundle`. Check it before you apply the file:

```sh
cosign verify-blob --key https://keys.2501.ai/cosign.pub --insecure-ignore-tlog=true \
  --bundle compose.yaml.bundle compose.yaml
```

## Support

A version is supported for 3 months after its release: each release says until when. Fixes go into the newest version, so a version past that date upgrades first. Older releases stay here.

The images these files name are pulled with the registry access 2501 gives you.
