.PHONY: help build up down logs migrate createsuperuser shell bash test lint format clean

help:
	@echo "Available commands:"
	@echo "  make build              - Build all images"
	@echo "  make up                 - Start all services"
	@echo "  make down               - Stop all services"
	@echo "  make logs               - View all logs"
	@echo "  make migrate            - Run Django migrations"
	@echo "  make createsuperuser    - Create Django superuser"
	@echo "  make shell              - Django shell"
	@echo "  make bash               - Bash in Django container"
	@echo "  make test               - Run tests"
	@echo "  make lint               - Run linting"
	@echo "  make clean              - Clean up containers and volumes"

build:
	docker-compose build

up:
	docker-compose up -d

down:
	docker-compose down

logs:
	docker-compose logs -f

migrate:
	docker-compose exec django python manage.py migrate

createsuperuser:
	docker-compose exec django python manage.py createsuperuser

makemigrations:
	docker-compose exec django python manage.py makemigrations

shell:
	docker-compose exec django python manage.py shell

bash:
	docker-compose exec django bash

test:
	docker-compose exec django python manage.py test

lint:
	docker-compose exec django flake8 .

format:
	docker-compose exec django black .

ps:
	docker-compose ps

restart:
	docker-compose restart

stop:
	docker-compose stop

restart-django:
	docker-compose restart django

restart-nginx:
	docker-compose restart nginx

restart-db:
	docker-compose restart db

logs-django:
	docker-compose logs -f django

logs-nginx:
	docker-compose logs -f nginx

logs-db:
	docker-compose logs -f db

collectstatic:
	docker-compose exec django python manage.py collectstatic --noinput

backup-db:
	docker-compose exec db pg_dump -U postgres myapp_db > backup.sql

restore-db:
	docker-compose exec -T db psql -U postgres myapp_db < backup.sql

clean:
	docker-compose down -v
	find . -type d -name __pycache__ -exec rm -rf {} +
	find . -type f -name "*.pyc" -delete

install-deps:
	docker-compose exec django pip install -r requirements.txt

update-deps:
	docker-compose exec django pip list --outdated

nginx-test:
	docker-compose exec nginx nginx -t

nginx-reload:
	docker-compose exec nginx nginx -s reload

health:
	curl -k https://localhost/health/

db-connect:
	docker-compose exec db psql -U postgres -d myapp_db

db-list:
	docker-compose exec db psql -U postgres -l

db-size:
	docker-compose exec db psql -U postgres -d myapp_db -c "SELECT pg_size_pretty(pg_database_size(current_database()));"
