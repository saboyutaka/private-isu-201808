.DEFAULT_GOAL := help

build: up bundle db-reset ## build develoment environment

serve: up attach ## Run Serve

up: ## Run web container
	docker-compose up -d web

attach: ## Attach running web container for binding.pry
	docker attach `docker ps -f name=private-isu_web -f status=running --format "{{.ID}}"`

bundle: # Run Bundle install
	docker-compose run --rm web bundle install

db-reset: _download_dbdump ## Reset DB
	docker-compose up -d db
	docker-compose exec db sh /var/tmp/wait.sh
	docker-compose exec db mysql -uroot -e 'drop database if exists isuconp'
	docker-compose exec db mysql -uroot -e 'create database isuconp'
	docker-compose exec db sh -c 'mysql -uroot isuconp < /var/tmp/db.dump'

_download_dbdump: ## Download db.dump.tgz from Dropbox
	@cd db
	@if ! [ -f db.dump ];then curl -L -O -J 'https://www.dropbox.com/s/b0os3izfmmj957g/db.dump?dl=0'; fi

.PHONY: help
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'
