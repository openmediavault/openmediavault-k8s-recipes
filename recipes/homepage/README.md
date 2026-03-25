# Homepage

A modern, fully static, fast, secure fully proxied, highly customizable application dashboard.

The app can be reached at `https://homepage.<FQDN>:8443` after deployment.

## Configuration

All configuration is stored in the `homepage` ConfigMap. Edit the ConfigMap directly with `kubectl` to customise homepage:

```shell
kubectl edit configmap homepage -n homepage-app
```

The ConfigMap contains the following keys, each mapping to a YAML configuration file:

| Key               | Purpose                                                                                 |
| ----------------- | --------------------------------------------------------------------------------------- |
| `settings.yaml`   | Global settings (theme, colour, layout, etc.)                                           |
| `bookmarks.yaml`  | Bookmark links grouped into sections.                                                   |
| `services.yaml`   | Service tiles grouped into sections, with optional widget data.                         |
| `widgets.yaml`    | Information widgets shown at the top of the page (search, resources, kubernetes, etc.). |
| `kubernetes.yaml` | Kubernetes integration mode (`cluster` or `default`).                                   |
| `docker.yaml`     | Docker socket connections (not used in this recipe).                                    |
| `proxmox.yaml`    | Proxmox connections for the Proxmox widget (optional).                                  |
| `custom.css`      | Custom CSS overrides.                                                                   |
| `custom.js`       | Custom JavaScript.                                                                      |

After saving the ConfigMap, homepage picks up the changes automatically without a pod restart.

For full configuration reference see <https://gethomepage.dev/configs/>.

## Kubernetes service discovery

The recipe runs with `mode: cluster` in `kubernetes.yaml`, which allows homepage to automatically discover services across all namespaces. To make a service appear as a tile, add the following annotations to its `Ingress` or Traefik `IngressRoute`:

```yaml
annotations:
  gethomepage.dev/enabled: "true"
  gethomepage.dev/name: "My Service"
  gethomepage.dev/description: "A short description"
  gethomepage.dev/group: "My Group"
  gethomepage.dev/icon: "myservice.png"
```

See <https://gethomepage.dev/configs/kubernetes/> for the full list of supported annotations.
