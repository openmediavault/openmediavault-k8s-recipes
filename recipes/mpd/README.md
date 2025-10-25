# Music Player Daemon (MPD)

The MPD server will listen on port 6600 by default.
You need to expose the port by running the following commands as root:

```
# omv-env set OMV_K8S_TRAEFIK_PORTS "{mpd: {port: 6600, exposedPort: 6600, expose: {default: true}}}"
# omv-salt stage run prepare
# omv-salt deploy run k3s
```

If no `mpd.conf` file exists in the config directory, the recipe will create one with two predefined outputs:

- an ALSA output
- a 320kpbs MP3 stream that streams to the Icecast mountpoint at `http://icecast.mpd.<FQDN>:8080/listen.mp3`

The outputs can be disabled from your MPD client, or removed from the configuration.
Refer to the [example `mpd.conf` file][mpdconf-example] for a full list of available configuration options.
