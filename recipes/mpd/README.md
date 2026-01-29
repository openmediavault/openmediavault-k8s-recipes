# Music Player Daemon (MPD)

The MPD server will listen on port 6600 by default.

If no `mpd.conf` file exists in the config directory, the recipe will create one with two predefined outputs:

- an ALSA output
- a 320kpbs MP3 stream that streams to the Icecast mountpoint at `http://icecast.mpd.<FQDN>:8080/listen.mp3`

The outputs can be disabled from your MPD client, or removed from the configuration.
Refer to the [example `mpd.conf` file][mpdconf-example] for a full list of available configuration options.

In addition, a web interface provided by [myMPD] is provided at `http://mpd.<FQDN>:8080/`.

MPD audio is also provided to a [Snapcast] server.
The corresponding web interface can be reached under `http://snapcast.mpd.<FQDN>:8080/`.

[myMPD]: https://jcorporation.github.io/myMPD/
[Snapcast]: https://github.com/badaix/snapcast
