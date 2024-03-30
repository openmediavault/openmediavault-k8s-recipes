This repository contains recipes to easily deploy container-based applications with the 
`openmediavault-k8s` Kubernetes plugin. 

The following rules should be observed in the recipes:

- Create a namespace for each application according the schema: `<APPNAME>-app`
- Traefik is used by the `openmediavault-k8s` plugin. So use `IngressRoute` over `Ingress`.
- For HTTPS ingresses use the TLS secret name `host-selfsigned-cert` if you want to use the auto-created self-signed certificate. Use `host-imported-cert` if you have configured an SSL certificate in the plugin settings that is managed via the openmediavault UI. 
- The default entry points for `IngressRoute` are `web` (HTTP) and `websecure` (HTTPS).
- Make use of a `NodePort` service only if the application does not work behind a reverse proxy.
- Make use of the `HelmChart` resource if the application can be installed via Helm.

# metadata.yaml

## Architecture

Use this translation table for the `architecture` flag in the `metadata.yaml` file:

| Architecture | Also known as |
|--------------|---------------|
| i386         | i386          |
| amd64        | x86_64        |
| armel        | armv6l        |
| armhf        | armv7l        |
| arm64        | aarch64       |

## Section

The `section` value in the `metadata.yaml` file is inspired by those that are used in Debian, e.g. `graphics`, `net` or `utils`. Get a full list [here](https://www.debian.org/doc/debian-policy/ch-archive.html#s-subsections).

# Placeholders

The recipe import of the `openmediavault-k8s` plugin supports the following 
placeholders. These are intended to simplify the handling of various system 
settings, for example the mapping of user and group names to IDs.
```
- hostname()
- domain()
- fqdn()
- uid('<NAME>')
- gid('<NAME>')
- sharedfolder_path('<NAME>')
- conf_get('<DB_OBJ_ID>', '<UUID>')
```
## Examples:
```yaml
apiVersion: apps/v1
kind: Deployment
...
spec:
  ...
  template:
    metadata:
      ...
    spec:
      containers:
        - securityContext:
            runAsNonRoot: true
            runAsUser: {{ gid('nobody') }}
            runAsGroup: {{ gid('nogroup') }}
          args:
            - "--port 3000"
            - "--base /wetty"
            - "--force-ssh"
            - "--ssh-port {{ conf_get('conf.service.ssh') | get('port') }}"
            - "--ssh-host {{ hostname() }}"
  ...
```
```yaml
apiVersion: apps/v1
kind: Deployment
...
spec:
  ...
  template:
    metadata:
      ...
    spec:
      containers:
        ...
      volumes:
        - name: originals-volume
          hostPath:
            type: Directory
            path: {{ sharedfolder_path('images') }}
  ...
```
