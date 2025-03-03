# pyLoad

pyLoad will deploy its config to the settings folder on first start. It'll be running under /pyload so we have
to tell in the config: `settings/pyload.cfg`

```
webui - "Web Interface":
...
str prefix : "Path prefix" = /pyload
```

The default login is: username - pyload password - pyload
Make sure to change in user settings.

Click and Load is exposed on port 9666.
