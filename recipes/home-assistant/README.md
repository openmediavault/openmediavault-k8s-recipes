# Post installation steps

After installing the Home Assistant recipe, you need to modify the `configuration.yaml`
file in the shared folder that is used to store the Home Assistant configuration.
To get the required information try to access the Home Assistant web interface at 
`https://home-assistant.<FQDN>:8443`. This will generate an error message in the 
log files of the Home Assistant pod.

Go to `Services | Kubernetes | Resources | Pods` in the openmediavault Workbench,
find the Home Assistant pod, select it and click the `Logs` action button at the
top of the datatable.

Look out for the following error message:
```text
[31m2025-07-30 17:16:06.218 ERROR (MainThread) [homeassistant.components.http.forwarded] [31m2025-07-30 17:16:06.218 ERROR (MainThread) [homeassistant.components.http.forwarded] A request from a reverse proxy was received from 10.42.0.13, but your HTTP integration is not set-up for reverse proxies[0m[0m
```

The `configuration.yaml` file needs to be modified by including the following lines
at the bottom of the file:

```yaml
...
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 10.42.0.13
```

Replace `10.42.0.13` with your IP address from the error message. Alternatively 
use `10.42.0.0/16` to allow all IP addresses from the Traefik reverse proxy.

Finally restart the Home Assistant pod by going to `Services | Kubernetes | Resources | Pods`,
select the pod and click the `Delete` action button at the top of the datatable.

Now the Home Assistant web interface should be accessible via the reverse proxy of
the openmediavault Kubernetes plugin.

> ℹ️ **Info:** You might need to modify the `configuration.yaml` file after restoring a backup.
