# DevOps Portfolio - TP07: CI/CD

![CI/CD Pipeline](https://github.com/LucioEzequiel/devops-TP06/actions/workflows/cicd.yml/badge.svg)

App de notas (Flask + Postgres + Nginx) con pipeline CI/CD usando GitHub Actions.

## Pipeline

| Stage      | Trigger         | Que hace                           |
|------------|-----------------|------------------------------------|
| lint       | todo push       | flake8 en Python, yamllint en YAML |
| test       | despues de lint | pytest con reporte de cobertura    |
| build-push | main y develop  | docker buildx, push a Docker Hub   |
| deploy     | (comentado)     | SSH al servidor, compose pull + up |

El job deploy esta comentado en cicd.yml: el runner de GitHub no puede
llegar por SSH a un servidor detras de una red casera.

## Secrets requeridos

- DOCKERHUB_USERNAME
- DOCKERHUB_TOKEN (Access Token de Docker Hub, no la contrasena)

## Correr tests localmente

    cd backend
    pip install -r requirements-dev.txt
    pytest tests/ -v --cov=. --cov-report=term-missing

## Estructura del pipeline

    feature/* -> lint -> test
    develop   -> lint -> test -> build -> push
    main      -> lint -> test -> build -> push
