GRAFANA_PORT=55039
GRAFANA_CONTAINER_NAME=Reference-Grafana

LOKI_PORT=55040
LOKI_CONTAINER_NAME=Reference-Loki

PROJECT_NAME=reference_loki
REFERENCE_DOCKER_DIR=../Reference_Docker/

# ----- ----- ----- ----- -----

all:
	@echo init
	@echo up
	@echo down
	@echo clean

init: init-dir init-env

init-dir:
	cp -r ../Reference_Docker/Loki/data data

init-env:
	@rm -f .env
	@touch .env
	@echo 'GRAFANA_PORT=$(GRAFANA_PORT)' >> .env
	@echo 'LOKI_PORT=$(LOKI_PORT)' >> .env
	@echo 'LOKI_CONTAINER_NAME=$(LOKI_CONTAINER_NAME)' >> .env

up:
	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Loki/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_loki \
	up -d loki

	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Grafana/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_grafana \
	up -d grafana

down:
	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Loki/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_loki \
	down

	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Grafana/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_grafana \
	down

# GRAFANA_CONTAINER_NAME can have - (e.g. Reference-Grafana)
# but docker-compose will preprocess and convert - into _
clean:
	docker volume rm $(PROJECT_NAME)_grafana_$(shell echo $(GRAFANA_CONTAINER_NAME) | tr '-' '_')