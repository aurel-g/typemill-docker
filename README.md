# typemill-docker
Unofficial Docker image for TYPEMILL based on Debian, designed to be easy to use.

## What is TYPEMILL ?

> TYPEMILL is a small flat file cms created for editors and writers. It provides a author-friendly dashboard and a visual-block-editor for markdown based on vue.js. Use TYPEMILL for manuals, documentations, web-books and similar publications. The website http://typemill.net itself is an example for TYPEMILL.

![TYPEMILL Screenshot](https://typemill.net/media/tm-demo.gif)

Get more information on TYPEMILL repository : https://github.com/typemill/typemill.

## Docker image

### Guidelines

As TYPEMILL itself, this image has been designed to be easy to use : no extra setup step, few parameters.

### Good to know

* Default content and configuration files will be created automatically if the volumes are bound to empty directories (see _Volumes_ below)
* You can customize which UID/GID will hold the files and run Apache process (see _Environment variables_ below)
* Files ownerships and permissions will be set adequately at each startup

### Volumes

* `/var/www/html/settings` : required if you want your settings (i.e. user accounts, site medatada...) to persist. Can be bound to an empty directory, 
 will be initialized during the setup process
* `/var/www/html/content` : required if you want your content to persist. Will be initialized with default content if bound to an empty directory
* `/var/www/html/media` : as for the `content` directory, required for the media files to persist. Empty by default.
* `/var/www/html/cache` : optional, if you want the cache files to persist (for performance purposes)

### Environment variables

* `TYPEMILL_UID` : id of the system user (defaults to 33 which corresponds to default UID for `www-data` user who runs Apache under Debian)
* `TYPEMILL_GID` : id of the system group (defaults to 33 too)

### Ports

Apache listens on *port 80* (HTTP). There is no built-in support for HTTPS in this image.

### Examples

Depending on you system's configuration, you may need to use `sudo` before `docker` command.

#### Quick & dirty (demo)

This command line will run a Typemill instance without data persistance (all data will be lost when the container will stop) on port 80 under default user UID/GID :

`docker run --rm --name=typemill-demo --net=host aberty/typemill-docker`

* note that it will fail if port 80 is already in use on the system. In this case, please have a look on the more _Typical example_ below
* the `--rm` switch means that the container will be cleared from memory when you'll stop it
* the `--name` parameter gives a non-random name to the container, allowing to stop it more easily
* to stop the container, use this command : `docker stop typemill-demo`

#### Typical usage

* let's say you want to save Typemill's data directories under `/var/typemill` path.
* using an optional `typemill` docker network for Typemill, created via `docker network create typemill`
* giving `1000:1000` as UID/GID to run Typemill
* assuming you want to run the tool on port `8080`

```
docker run --rm \
    --name=typemill \
    --net=typemill \
    -p 8080:80 \
    -e TYPEMILL_UID=1000 \
    -e TYPEMILL_GID=1000 \
    -v /var/typemill/settings/:/var/www/html/settings/ \
    -v /var/typemill/cache/:/var/www/html/cache/ \
    -v /var/typemill/content/:/var/www/html/content/ \
    -v /var/typemill/media/:/var/www/html/media/ \
    aberty/typemill-docker
```
