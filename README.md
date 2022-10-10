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
* Clone Repo with `git clone git@github.com:rnsrk/performance-drupal.git`.
* Rename .example-env to .env and set environment varibles for postgres service.
* Run `./prepare_volumes.sh` to create drupal-data directory and add absolute path to .env variable.
* Get [GraphDB free standalone server](https://graphdb.ontotext.com/).
* Copy zip-file to to /graphdb_context and rename it to "graphdb.zip".
* Start containers with `docker compose up -d`.
* Go to`http://localhost:3001`

