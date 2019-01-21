# Graylog GELF Module for Logspout

This module allows Logspout to send Docker logs in the GELF format to Graylog via UDP or TCP.

## Why

This module is based on [Rick Alm's module](https://github.com/rickalm/logspout-gelf), which is based on Micha Hausler's initial module for Logspout. This module contains
additional features including:

* TCP support (part of Rick Alm's module)
* Retry when TCP connection fails
* Swarm hostname support
* Null-terminated GELF messages

From Rick:

> Micha Hausler did an initial module for Logspout to output in Gelf format, but the datamodel he chose was based on GELF ideas. This version of the module outputs in the same format as the Docker GELF logger.
>
> The disadvantage to using the Docker GELF logger is the loss of the local Docker log (e.g. `docker logs <container>`). By using Logspout to effectively "tail" the log, this creates an additional copy of the log sent in GELF Format.

## Build

To build, you'll need to fork [Logspout](https://github.com/gliderlabs/logspout), add the following code to `modules.go` 

```
_ "github.com/karlvr/logspout-gelf"
```
and run `docker build -t $(whoami)/logspout:gelf`

Alternatively you can use my prebuilt Docker image [karlvr/logspout-gelf](https://hub.docker.com/r/karlvr/logspout-gelf).

## Run

```
docker run \
    -v /var/run/docker.sock:/var/run/docker.sock \
    $(whoami)/logspout:gelf \
    gelf://<graylog_host>:12201
```

### Hostname

The contents of `/etc/host_hostname` is used to set the hostname reported to Graylog, if available. This follows
the behaviour of the syslog module.

e.g.

```
docker run \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v /etc/hostname:/etc/host_hostname:ro \
    $(whoami)/logspout:gelf \
    gelf://<graylog_host>:12201
```

Otherwise the `GELF_HOSTNAME` environment variable is used, if available.

## License

MIT. See [License](LICENSE)
