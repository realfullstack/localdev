FROM tianon/true as true
FROM python:3.8.6-slim-buster as backend-django

RUN apt-get update && apt-get install -y \
    bash \
    wait-for-it \
    make \
    curl \
    sudo \
    gettext \
    git \
    && rm -rf /var/lib/apt/lists/*

# we may use pip or other dependency tools during development
RUN pip install --upgrade pip pipenv

COPY --from=true /true /true

#configure user django and enable sudo
ARG DOCKER_UID

#Create the django/django with desired uid/home
RUN addgroup django \
    && mkdir /home/django \
    && useradd -g django -G sudo --home /home/django --uid $DOCKER_UID django \
    && chown django:django /home/django \
    && chown django:django /home/django \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

#Create various directories used by the container (celery and app data)
RUN mkdir -p /app /celerybeat

USER django
WORKDIR /app

ENV PYTHONUNBUFFERED 1
ENV PIPENV_MAX_RETRIES=2
ENV PIPENV_VENV_IN_PROJECT=true
ENV PYTHONPATH="/app:$PYTHONPATH"
ENV DJANGO_SETTINGS_MODULE=config.settings

COPY ./repo/Pipfile* /app/
RUN sudo pipenv install --system --dev