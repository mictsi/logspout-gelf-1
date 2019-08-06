# This Project is replaced with https://github.com/mictsi/logspout-gelf-tls 


# Graylog GELF Module for logspout

This module allows logspout to send Docker logs in the GELF format to Graylog via UDP, TCP or TLS.

Unlike other logspout GELF modules, this module uses Logspout's built-in transports (UDP, TCP and TLS) so all of logspout's configuration for those transports applies. This is particularly important for TLS (see below for configuration examples).

## Why

This module is based on [Rick Alm's module](https://github.com/rickalm/logspout-gelf), which is based on [Micha Hausler's initial module for logspout](https://github.com/micahhausler/logspout-gelf). This module contains
additional features including:

* TCP support (part of Rick Alm's module)
* Retry when TCP connection fails
* Swarm hostname support
* Null-terminated GELF messages

This module attempts to copy the logging format used by the [Docker gelf log driver](https://github.com/moby/moby/blob/master/daemon/logger/gelf/gelf.go)
and [Micha Hausler's logspout module](https://github.com/micahhausler/logspout-gelf/blob/master/gelf.go).

## Build

To build, you'll need to fork [Logspout](https://github.com/gliderlabs/logspout), add the following code to `modules.go` 

```
_ "github.com/karlvr/logspout-gelf"
```
and run `docker build -t $(whoami)/logspout:gelf .`

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
