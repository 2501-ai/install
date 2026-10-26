# 2501 installation files

Each [release](https://github.com/2501-ai/install/releases) here is one 2501 build, `X.Y.Z-rN`. Its files are a working standard installation, the same for every site, generated from the 2501 Helm chart and signed by 2501. Install from the files of the build you run. Kubernetes and OpenShift need `kubectl` or `oc` only; Helm is optional.

| You run | Files | Command |
|---|---|---|
| Kubernetes | `kubernetes.yaml`, `kubernetes.settings.example.yaml`, `kubernetes.ingress.example.yaml` | `kubectl apply -f kubernetes.yaml -n <namespace>` |
| OpenShift | `openshift.yaml`, `openshift.settings.example.yaml`, `openshift.ingress.example.yaml` | `oc apply -f openshift.yaml -n <namespace>` |
| Docker Swarm | `compose.yaml`, `env.example` | `set -a; . ./.env; set +a; docker stack deploy -c compose.yaml ai-2501` |
| Helm or ArgoCD, if you use them | `2501-<build>.tgz`, `values.example.yaml` | `helm upgrade --install ai2501 2501-<build>.tgz -n <namespace> -f values.yaml` |

On Kubernetes and OpenShift, you create once the `2501-settings` ConfigMap from the settings example, your Secrets and your Ingress (OpenShift turns it into a Route). On Swarm, you copy `env.example` to `.env` and fill it. Only Command Center's host name is needed: the first tenant, administrator and LLM are optional and can be set later in Command Center.

What your site needs beyond the standard installation (a proxy, a corporate CA, your own database, a registry of yours, another storage class) you add yourself, in an overlay of your own next to our files: a kustomize folder applied with `kubectl apply -k`, or a second compose file on Swarm. The 2501 deployment documentation gives a recipe for each. Never edit our files: every build replaces them.

Every build has its own files: they name the exact images of that build, so an upgrade always takes the new files, and keeps your settings, Secrets and overlay. `https://github.com/2501-ai/install/releases/latest/download/<file>` always points to the newest build.

## Verify a file

Every file comes with a `.bundle`. Check it before you apply the file:

```sh
cosign verify-blob --key https://keys.2501.ai/cosign.pub --insecure-ignore-tlog=true \
  --bundle compose.yaml.bundle compose.yaml
```

## Support

A version is supported for 3 months after its release: each release says until when. Fixes go into the newest version, so a version past that date upgrades first. Older releases stay here.

The images these files name are pulled with the registry access 2501 gives you.
