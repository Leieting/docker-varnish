# Varnish Docker container

> Alpine Linux 3.4

> Varnish 4.1.2

## Usage

To use this container, you will need to provide your custom config.vcl (which is usually the case).

```
docker run -d \
  --link web-app:backend-host \
  --volumes-from web-app \
  --env 'VCL_CONFIG=/data/path/to/varnish.vcl' \
  leieting/varnish
```

In the above example we assume that:
* You have your application running inside `web-app` container and web server there is running on port 80 (although you don't need to expose that port, as we use --link and varnish will connect directly to it)
* `web-app` container has `/data` volume with `varnish.vcl` somewhere there
* `web-app` is aliased inside varnish container as `backend-host`
* Your `varnish.vcl` should contain at least backend definition like this:  
```
backend default {
    .host = "backend-host";
    .port = "80";
}
```

## Environmental variables

You can configure Varnish daemon by following env variables:

> **VCL_CONFIG** `/etc/varnish/default.vcl`  
> **CACHE_SIZE** `64m`  
> **VARNISHD_PARAMS** `-p default_ttl=3600 -p default_grace=3600`


## Author(s)

* Marcin Ryzycki (<marcin@m12.io>)  
* Przemyslaw Ozgo (<linux@ozgo.info>)
* Thomas Sunde Nielsen (thomas@leieting.no)

Forked from [million12/docker-varnish](https://github.com/million12/docker-varnish)
