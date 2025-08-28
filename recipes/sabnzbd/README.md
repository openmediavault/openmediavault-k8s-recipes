# SABnzbd

Upon first start of the service, the config file `sabnzb.ini` in the shared folder is created.
However, the web will be inaccessible and the following error message is shown:

> Access denied - Hostname verification failed: https://sabnzbd.org/hostname-check

You have to edit `sabnzb.ini`, set `host_whitelist = sabnzbd.<fqdn>` and then restart the pod.
Afterwards, the web interface will be accessible and configuration wizard will be shown that allows performing the initial configuration.
