# Prerequisites

To get `RustFS` up and running, the K8s plugin must be configured accordingly.
On the `Settings` page, the ports used by `RustFS` must be added under `Load Balancer`.

Add the following ports:

| Name            | Port | Exposed Port | Protocol | Expose |
|-----------------|------|--------------|----------|--------|
| rustfs-endpoint | 9000 | 9000         | TCP      | Yes    |

# UI

The default login is:

**Username** - admin  
**Password** - admin
