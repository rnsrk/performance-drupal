# Drupal Performance Stack

## Stack
* Webserver from nginx-alpine
* Database from postgres-alpine
* Drupal from drupal-fpm-alpine
	+ with apcu, uploadprogress
	+ max_execution_time = 60
	+ max_input_time = 30
	+ max_input_nesting_level = 640
	+ max_input_vars = 10000
	+ memory_limit = 512M
	+ upload_max_filesize = 20M
	+ max_file_uploads = 50
	+ post_max_size = 20M

* Search from solr-slim-buster
* TripleStore from slim-buster

## Start

### Prepare Configs
* Clone Repo with `git clone git@github.com:rnsrk/performance-drupal.git`.
* Rename `.example-env` to `.env` and set environment variables for postgres service.
* Rename `./nginx_context/.example-htpasswd` to `.htpasswd` and set environment variables for nginx service.
* Rename `./drupal_context/.example-settings.php` to `settings.php` and set environment variables for drupal service.

### Add local domains
* Add the domains to your `/etc/hosts`:
```
127.0.0.1	drupal.local
127.0.0.1	graphdb.local
127.0.0.1	adminer.local
```
### Get Triplestore
* Get [GraphDB free standalone server](https://graphdb.ontotext.com/).
* Copy zip-file to to `/graphdb_context` and rename it to `graphdb.zip`.

### Start containers
* Start containers with `docker compose up -d`.

### Install Drupal
* Go to `http://drupal.local` for Drupal installation

### Visit Workbenches
* Go to `http://graphdb.local` for GraphDB workbench
* Go to `http://adminer.local` for Adminer workbench

## Roadmap
* implement iipserver correctly
* have readable/writable mount volumes for postgres, solr, graphdb