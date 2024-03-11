This repository contains recipes to easily deploy applications with the 
openmediavault Kubernetes plugin. 

The following rules should be observed in the recipes:

- Create a namespace for each application according the schema: `<APPNAME>-app`
- Traefik is used by the `openmediavault-k8s` plugin. So use `IngressRoute` over `Ingress`.
- There are `PersistenVolumes`s for each existing shared folder in openmediavault. They are named according this schema: `schared-folder-<NAME>`. The `StorageClass` for these shared folders is `shared-folder`. Use `PersistenVolumeClaim`s to easily use them in your recipe.
- For HTTPS ingresses use the TLS secret name `host-selfsigned-cert` if you want to use the auto-created self-signed certificate. Use `host-imported-cert` if you have configured an SSL certificate in the plugin settings that is managed via the openmediavault UI. 
- The default entry points for `IngressRoute` are `web` (HTTP) and `websecure` (HTTPS).
- Make use of a `NodePort` service only if the application does not work behind a reverse proxy.
- Make use of the `HelmChart` resource if the application can be installed via Helm.
