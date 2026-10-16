# Forgejo

After starting the service, a configuration wizard is started that allows performing the initial configuration via the web interface.

## Upgrade Notes

### Upgrading from 1.0.x to 1.1.x (Forgejo 12 to 15)

If `app.ini` is found in the previously set config shared folder, you
will need to manually move this file into your Forgejo home shared
folder. This file should have the relative path `custom/conf/app.ini`
within your Forgejo's home folder.

For further details of this change, please refer to Forgejo v15's
[release notes](https://forgejo.org/2026-04-release-v15-0/#docker-rootless-config-file-location)
