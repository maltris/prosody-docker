# Note

This image is deprecated as there is a working, much smaller and alpine-based alternative available [here](https://github.com/joschi/docker-prosody-alpine).

# Prosody Docker

This is a Prosody Docker image.

## Building

```bash
docker build -t prosody-stretch:0.10 .
```

## Running

Docker images are built off an __Debian Stretch__ base.

```bash
docker run -d --name prosody -p 5222:5222 prosody-stretch:0.10
```

A user can be created by using environment variables `LOCAL`, `DOMAIN`, and `PASSWORD`. This performs the following action on startup:

  prosodyctl register *local* *domain* *password*

Any error from this script is ignored. Prosody will not check the user exists before running the command (i.e. existing users will be overwritten). It is expected that [mod_admin_adhoc](http://prosody.im/doc/modules/mod_admin_adhoc) will then be in place for managing users (and the server).

### Ports

The image exposes the following ports to the docker host:

* __80__: HTTP port
* __443__: HTTPS port
* __5222__: c2s port
* __5269__: s2s port
* __5347__: XMPP component port
* __5280__: BOSH / websocket port
* __5281__: Secure BOSH / websocket port

Note: These default ports can be changed in your configuration file. Therefore if you change these ports will not be exposed.

### Volumes

Volumes can be mounted at the following locations for adding in files:

* __/etc/prosody__:
  * Prosody configuration file(s)
  * SSL certificates
* __/var/log/prosody__:
  * Log files for prosody - if not mounted these will be stored on the system
  * Note: This location can be changed in the configuration, update to match
  * Also note: The log directory on the host (/logs/prosody in the example below) must be writeable by the prosody user
* __/usr/lib/prosody-modules__ (suggested):
  * Location for including additional modules
  * Note: This needs to be included in your config file, see http://prosody.im/doc/installing_modules#paths

### Example

```
docker run -d \
   -p 5222:5222 \
   -p 5269:5269 \
   -p localhost:5347:5347 \
   -e LOCAL=romeo \
   -e DOMAIN=shakespeare.lit \
   -e PASSWORD=juliet4ever \
   -v /data/prosody/configuration:/etc/prosody \
   -v /logs/prosody:/var/log/prosody \
   -v /data/prosody/modules:/usr/lib/prosody-modules \
   prosody/prosody:0.10
```
