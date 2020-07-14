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
