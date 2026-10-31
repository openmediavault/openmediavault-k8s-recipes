# Radicale

A small but powerful CalDAV (calendars, to-do lists) and CardDAV
(contacts) server.

The app can be reached at `https://radicale.<FQDN>:8443` after
deployment.

## Prerequisites

### Shared folders

Before deploying this recipe, create the following **shared folders**
in `Storage | Shared Folders` in the openmediavault Workbench:

| Variable                 | Purpose                                  |
|--------------------------|------------------------------------------|
| `configSharedFolderName` | Stores the Radicale configuration files. |
| `dataSharedFolderName`   | Stores Radicale's application data.      |

### Configuration

By default Radicale rejects all authentication attempts. To use
Radicale, you must create the appropriate configuration files in the
config shared folder you created previously. Please refer to
Radicale's documentation for a [basic
configuration](https://radicale.org/v3.html#basic-configuration) or
for full details on the available
[configuration](https://radicale.org/v3.html#configuration) options.

Should you modify the configuration after deploying this recipe,
recreate Radicale's pod to reload the configuration. This can be done
in `Services | Kubernetes | Resources | Pods` in the openmediavault
Workbench. From here search for the Radicale pod and delete it, after
which the pod will be automatically recreated.
