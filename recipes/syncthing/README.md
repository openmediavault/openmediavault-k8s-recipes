# Prerequisites

To get Syncthing up and running, the K8s plugin must be configured accordingly.
On the `Settings` page, the ports used by Syncthing must be added under `Load Balancer`.

Add the following ports:

| Name         | Port  | Exposed Port | Protocol | Expose |
|--------------|-------|--------------|----------|--------|
| st-listen    | 22000 | 22000        | TCP      | Yes    |
| st-discovery | 21027 | 21027        | UDP      | Yes    |
