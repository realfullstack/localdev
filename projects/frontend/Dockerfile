FROM tianon/true as true
FROM node:14.15-buster-slim

RUN apt-get update && apt-get install -y \
    bash \
    wait-for-it \
    make \
    curl \
    sudo \
    git \
    procps \
    net-tools \
    && rm -rf /var/lib/apt/lists/*

COPY --from=true /true /true

#configure user app and enable sudo
ARG DOCKER_UID

#Create the app/app with desired uid/home
RUN mkdir -p /app /home/app \
    && userdel node && rm -rf /home/node \
    && addgroup app \
    && useradd -g app -G sudo --home /home/app --uid $DOCKER_UID app \
    && chown app:app /home/app \
    && chown app:app /app \
    && echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers


USER app
WORKDIR /app
