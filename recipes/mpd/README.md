# Music Player Daemon (MPD)

The MPD server will listen on port 6600 by default.
If no `mpd.conf` file exists in the config directory, the recipe will create one that exposes three outputs:

- an ALSA output
- an MP3 HTTP stream on port 8081
- a FLAC HTTP stream on port 8082

The outputs can be disabled from your MPD client, or removed from the configuration
Refer to the [example `mpd.conf` file][mpdconf-example] for a full list of available configuration options.
