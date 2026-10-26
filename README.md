# 2501 installation files

Each [release](https://github.com/2501-ai/install/releases) here is one 2501 build, `X.Y.Z-rN`. Its files are generated from the 2501 Helm chart and signed by 2501. Install from the files of the build you run.

| You run | From the release | Command |
|---|---|---|
| Kubernetes or OpenShift, with Helm or ArgoCD | `2501-<build>.tgz`, `values.example.yaml` | `helm upgrade --install ai2501 2501-<build>.tgz -n <namespace> -f values.yaml` |
| Kubernetes or OpenShift, without Helm | `values.example.yaml`, and the build's render image its notes name | `docker run --rm --user "$(id -u):$(id -g)" -v "$PWD:/work" <render image> values.yaml <namespace>`, then `kubectl apply -n <namespace> -f manifests.yaml` |
| Docker Swarm | `compose.yaml`, `env.example` | `set -a; . ./.env; set +a; docker stack deploy -c compose.yaml ai-2501` |

Your `values.yaml`, or your `.env` on Swarm, is yours: keep it in your Git and update it as each release's notes say. The chart checks it on every render. Every build has its own files and render image: they name the exact images of that build, so an upgrade always takes the new ones. `https://github.com/2501-ai/install/releases/latest/download/<file>` always points to the newest build.

## Verify a file

Every file comes with a `.bundle`. Check it before you apply the file, and check the render image the same way with `cosign verify`:

```sh
cosign verify-blob --key https://keys.2501.ai/cosign.pub --insecure-ignore-tlog=true \
  --bundle compose.yaml.bundle compose.yaml
```

## Support

A version is supported for 3 months after its release: each release says until when. Fixes go into the newest version, so a version past that date upgrades first. Older releases stay here.

The images these files name are pulled with the registry access 2501 gives you.
