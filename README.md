# Hocker

"Hocker" is a Docker wrapper for "hacky shell script deployment".

## Why?

Deploying code via hacky shell scripts, while not desirable, works OK for many use cases (simple nginx+wordpress+mysql deployments, for example).

The primary goal of Hocker is to simplify those hacky shell scripts, thus making them less error prone, while also adding new and improved functionality.

## What?

Hocker allows for easily converting something like:

```bash
#!/bin/bash
set -e

exec docker run -d \
	--name apt-cacher-ng \
	--restart always \
	--dns 8.8.8.8 --dns 8.8.4.4 \
	--tmpfs /var/cache/apt-cacher-ng \
	tianon/apt-cacher-ng "$@"
```

Into something simpler, which also has more features:

```bash
#!/usr/bin/env hocker
# vim:set ft=sh:

hocker_run tianon/apt-cacher-ng \
	--dns 8.8.8.8 --dns 8.8.4.4 \
	--tmpfs /var/cache/apt-cacher-ng
```

Once saved as `apt-cacher-ng` and made executable:

```console
$ ./apt-cacher-ng --help
usage: apt-cacher-ng [options]
   ie: apt-cacher-ng --redeploy smart --pull always
       apt-cacher-ng --redeploy always
       apt-cacher-ng --pull never

options:

  --pull [always|missing|never] (default: missing)

  --redeploy [smart|always|never] (default: never)

  -p (alias for '--pull always')
  -s (alias for '--redeploy smart')

$ ./apt-cacher-ng -ps

Pulling 'tianon/apt-cacher-ng' ...
Using default tag: latest
latest: Pulling from tianon/apt-cacher-ng
Digest: sha256:1eae691f71061becd87789669e03d9510389c94ff51283a6bc2d4ce20413e6d1
Status: Image is up to date for tianon/apt-cacher-ng:latest

Not restarting 'apt-cacher-ng' (--redeploy 'smart')

```

The "smart" redeployment logic will only restart our `apt-cacher-ng` container when the associated image it's started from changes.

## ...

[youtu.be/79DijItQXMM](https://youtu.be/79DijItQXMM)
