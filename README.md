This repository contains a recipe collection for easy deployment of container-based applications with the 
`openmediavault-k8s` Kubernetes plugin. 

# Table of contents

- [General information](#general-information)
- [metadata.yaml](#metadatayaml)
- [Domain Specific Language](#domain-specific-language)
- [FAQ](#faq)

# General information

The following rules should be observed in the recipes:

- Create a namespace for each application according the schema: `<APPNAME>-app`
- Traefik is used by the `openmediavault-k8s` plugin. So use `IngressRoute` over `Ingress`.
- For HTTPS ingresses apply `tls: {}` to make use of the default TLS certificate.
- The default entry points for `IngressRoute` are `web` (HTTP) and `websecure` (HTTPS).
- Make use of a `NodePort` service only if the application does not work behind a reverse proxy.
- Make use of the `HelmChart` resource if the application can be installed via Helm.

# metadata.yaml

## Depends

The `depends` field is used to declare a dependency relationship by one recipe on another.

Please note that the recipes do not have a package management-like function to automatically install dependent recipes. This would go beyond the scope of the recipe collection and is therefore up to the user.

## Architecture

Use this translation table for the `architecture` field in the `metadata.yaml` file:

| Architecture  | Also known as              |
|---------------|----------------------------|
| `i386`        | `i386`                     |
| `amd64`       | `x86_64`                   |
| `armel`       | `armv6l`, `linux/arm/v6`   |
| `armhf`       | `armv7l`, `linux/arm/v7`   |
| `arm64`       | `aarch64`                  |

## Section

The `section` field in the `metadata.yaml` file is inspired by those that are used in Debian, e.g. `graphics`, `net` or `utils`. Get a full list [here](https://www.debian.org/doc/debian-policy/ch-archive.html#s-subsections).

Additionally, you can use:

- `kubernetes`

# Domain Specific Language

The recipe editor of the `openmediavault-k8s` has a DSL (Domain Specific Language)
that supports the user in getting specific information from their openmediavault
host system in Kubernetes manifests.

The following functions are currently available:

- **hostname()** – Get the hostname of the host.
- **domain()** – Get the domain name of the host.
- **fqdn()** – Get the FQDN of the host.
- **ipaddr(name=NULL)** – Get the IPv4 address of the specified network interface. If not set, the IPv4 address of the interface of the first default route is used.
- **ipaddr6(name=NULL)** – Get the IPv6 address of the specified network interface. If not set, the IPv6 address of the interface of the first default route is used.
- **uid(name)** – Get the UID of the specified user.
- **gid(name)** – Get the GID of the specified group.
- **sharedfolder_path(name)** – Get the full path of the specified shared folder.
- **conf_get(id, uuid=NULL)** – Get the specified database configuration object.
- **tz()** – Get the time zone of the host.

The following filters are currently available:

- **get(key, default=NULL)** – Get the specified key, e.g. `foo.bar.baz` from a dictionary.

For the built-in features of the used template engine please have a look here:

- [Basic knowledge](https://twig.symfony.com/doc/3.x/templates.html)
- [Filters](https://twig.symfony.com/doc/3.x/filters/index.html) and [functions](https://twig.symfony.com/doc/3.x/functions/index.html)

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
            runAsUser: {{ uid('nobody') }}
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
```yaml
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: immich-websecure
  namespace: immich-app
  ...
spec:
  entryPoints:
    - websecure
  routes:
    - match: Host(`immich.{{ ipaddr() }}.sslip.io`)
      kind: Rule
      services:
        - name: immich-server
          port: 3001
```

# FAQ

## Why do recipes use subdomains instead of URL path prefixes?

Using subdomains (e.g., app.mynas.internal) is preferred over path prefixes (e.g., mynas.internal/app) for accessing apps via the Traefik reverse proxy in the openmediavault Kubernetes plugin because:
  - Cleaner and simpler routing – Subdomains keep Ingress rules modular and avoid complex path rewrites.
  - Fewer compatibility issues – Many apps assume they run at / and break when served under a path prefix due to issues with relative URLs, static assets, or routing.
  - Avoids URL rewriting problems – With path prefixes, you often need to rewrite URLs or modify the app, which can cause subtle bugs or failures.
  - Better isolation and security – Subdomains help enforce security boundaries between apps.
  - Easier scaling and maintenance – Apps on subdomains are more portable, scalable, and microservice-friendly.
