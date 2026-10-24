# Radicale

A small but powerful CalDAV (calendars, to-do lists) and CardDAV
(contacts) server.

The app can be reached at `https://radicale.<FQDN>:8443` after
deployment.

## Prerequisites

Before deploying this recipe, create the following **shared folders**
in `Storage | Shared Folders` in the openmediavault Workbench:

| Variable                 | Purpose                                  |
|--------------------------|------------------------------------------|
| `configSharedFolderName` | Stores the Radicale configuration files. |
| `dataSharedFolderName`   | Stores Radicale's application data.      |

Within the config shared folder, create the Radicale configuration
file(s) as necessary. Please refer to Radicale's documentation for a
[basic configuration](https://radicale.org/v3.html#basic-configuration) 
or for full details on the available
[configuration](https://radicale.org/v3.html#configuration) options.
