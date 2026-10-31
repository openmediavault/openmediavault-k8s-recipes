# Usage

Generate the `.htpasswd` entries with `htpasswd -nB <username>` and insert
them into the recipe. Separate multiple entries with `\n`.

Initialize a repository with (the username must be part of the path because
`--private-repos` is enabled):

```
restic -r rest:https://<username>:<password>@restic.<FQDN>:8443/<username>/ init
```

By default `--append-only` is enabled, so clients can not delete or prune
snapshots. Remove that option in the recipe if you need this.
