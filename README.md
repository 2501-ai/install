# 2501 installation files

Each [release](https://github.com/2501-ai/install/releases) here is one 2501 build, `X.Y.Z-rN`. Its files are generated from the 2501 Helm chart and signed by 2501. Install from the files of the build you run.

| You run | Files | You keep |
|---|---|---|
| Kubernetes or OpenShift | `2501-<build>.tgz`, the chart, and `values.example.yaml` | your `values.yaml`, in your Git, and your Secrets |
| Docker Swarm | `compose.yaml`, `env.example` | your `.env` and your Docker secrets |

On Kubernetes and OpenShift, write your `values.yaml` from `values.example.yaml`. It holds no secret, only the names of your Secrets, and everything your site needs is a value in it: a proxy, a corporate CA, your own database or Elasticsearch, a registry of yours, a storage class, sizing. Only Command Center's host name is needed among the settings: the first tenant, administrator and LLM are optional and can be set later in Command Center. Then render the chart on your workstation with the `helm` binary, review, and apply the result; nothing of Helm runs in your cluster:

```sh
helm template ai2501 2501-<build>.tgz -n <namespace> -f values.yaml > manifests.yaml
kubectl -n <namespace> diff -f manifests.yaml
kubectl -n <namespace> apply -f manifests.yaml --prune -l app.kubernetes.io/part-of=2501,app.kubernetes.io/instance=ai2501 ...
```

Use `oc` on OpenShift. The 2501 deployment documentation gives the full prune command and the secrets to create. With Helm or ArgoCD, apply the same chart and `values.yaml` with them instead: `helm upgrade --install ai2501 2501-<build>.tgz -n <namespace> -f values.yaml`.

On Docker Swarm, copy `env.example` to `.env` and fill it, then `set -a; . ./.env; set +a; docker stack deploy -c compose.yaml <stack>`. What your site needs beyond the standard installation goes in a second compose file of yours.

Every build has its own files: they name the exact images of that build. An upgrade is the same commands with the new build's files and the same `values.yaml` or `.env`: review the diff, then apply. A render refuses a `values.yaml` that misses a value a new build needs. `https://github.com/2501-ai/install/releases/latest/download/<file>` always points to the newest build.

## Verify a file

Every file comes with a `.bundle`. Check it before you use the file:

```sh
cosign verify-blob --key https://keys.2501.ai/cosign.pub --insecure-ignore-tlog=true \
  --bundle 2501-<build>.tgz.bundle 2501-<build>.tgz
```

## Support

A version is supported for 3 months after its release: each release says until when. Fixes go into the newest version, so a version past that date upgrades first. Older releases stay here.

The images these files name are pulled with the registry access 2501 gives you, or from your copy of them.
