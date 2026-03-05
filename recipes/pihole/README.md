# Prerequisites

To get Pi-hole up and running, the K8s plugin must be configured accordingly.
On the `Settings` page, the ports used by Pi-hole must be added under `Load Balancer`.

Add the following ports:

| Name    | Port | Exposed Port | Protocol | Expose |
|---------|------|--------------|----------|--------|
| dns-tcp | 53   | 53           | TCP      | Yes    |
| dns-udp | 53   | 53           | UDP      | Yes    |
