FLOG_LOG_FORMAT=apache_common

PROMTAIL_PORT=58000
LOKI_PORT=58001
GRAFANA_PORT=58002

FLOG_LOG_PATH=$(PWD)/log/
LOKI_LOG_PATH=$(PWD)/log/
LOKI_CONFIG_PATH=$(PWD)/config/loki/
LOKI_DATA_PATH=$(PWD)/data/loki/
GRAFANA_DATA_PATH=$(PWD)/data/grafana/

FLOG_CONTAINER_NAME=Reference-Loki-Flog
LOKI_PROMTAIL_CONTAINER_NAME=Reference-Loki-Promtail
LOKI_LOKI_CONTAINER_NAME=Reference-Loki-Loki
GRAFANA_CONTAINER_NAME=Reference-Loki-Grafana

PROJECT_NAME=reference_loki
REFERENCE_DOCKER_DIR=../Reference_Docker/

# ----- ----- ----- ----- -----

all:
	@echo init
	@echo flog-[up]
	@echo up
	@echo down
	@echo info
	@echo clean

# initial username and password of Grafana is "admin"
info:
	@echo '00_Logging'
	@echo 'Grafana: http://localhost:$(GRAFANA_PORT)'

init: init-env
	@echo 'Initiated'

init-env:
	@rm -f .env
	@touch .env
	@echo 'FLOG_LOG_FORMAT=$(FLOG_LOG_FORMAT)' >> .env
	@echo 'PROMTAIL_PORT=$(PROMTAIL_PORT)' >> .env
	@echo 'LOKI_PORT=$(LOKI_PORT)' >> .env
	@echo 'GRAFANA_PORT=$(GRAFANA_PORT)' >> .env

	@echo 'FLOG_LOG_PATH=$(FLOG_LOG_PATH)' >> .env
	@echo 'LOKI_LOG_PATH=$(LOKI_LOG_PATH)' >> .env
	@echo 'LOKI_CONFIG_PATH=$(LOKI_CONFIG_PATH)' >> .env
	@echo 'LOKI_DATA_PATH=$(LOKI_DATA_PATH)' >> .env
	@echo 'GRAFANA_DATA_PATH=$(GRAFANA_DATA_PATH)' >> .env

	@echo 'FLOG_CONTAINER_NAME=$(FLOG_CONTAINER_NAME)' >> .env
	@echo 'LOKI_PROMTAIL_CONTAINER_NAME=$(LOKI_PROMTAIL_CONTAINER_NAME)' >> .env
	@echo 'LOKI_LOKI_CONTAINER_NAME=$(LOKI_LOKI_CONTAINER_NAME)' >> .env
	@echo 'GRAFANA_CONTAINER_NAME=$(GRAFANA_CONTAINER_NAME)' >> .env

up: flog-up loki-up grafana-up

flog-up:
	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Flog/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_flog \
	up -d flog

loki-up:
	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Loki/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_loki \
	up -d loki promtail

grafana-up:
	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Grafana/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_grafana \
	up -d grafana

down:
	docker compose \
	-f $(REFERENCE_DOCKER_DIR)Flog/docker-compose.yml \
	--env-file .env \
	--project-name $(PROJECT_NAME)_flog \
	down

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