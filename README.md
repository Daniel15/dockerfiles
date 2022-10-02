![Docker images](https://github.com/invoiceninja/dockerfiles/workflows/Docker%20images/badge.svg)
[![Docker image, latest](https://img.shields.io/docker/image-size/invoiceninja/invoiceninja/latest?label=latest)](https://hub.docker.com/r/invoiceninja/invoiceninja)
[![Docker image, alpine](https://img.shields.io/docker/image-size/invoiceninja/invoiceninja/alpine?label=alpine)](https://hub.docker.com/r/invoiceninja/invoiceninja)
[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/invoiceninja)](https://artifacthub.io/packages/search?repo=invoiceninja)
[![Pusblish Image](https://github.com/invoiceninja/dockerfiles/actions/workflows/publish-image.yaml/badge.svg)](https://github.com/invoiceninja/dockerfiles/actions/workflows/publish-image.yaml) [![Cache v5 Image](https://github.com/invoiceninja/dockerfiles/actions/workflows/build-image-v5.yaml/badge.svg)](https://github.com/invoiceninja/dockerfiles/actions/workflows/build-image-v5.yaml)

# Docker for [Invoice Ninja](https://www.invoiceninja.com/)

:crown: **Features**

:lock: Automatic HTTPS (:heart: [Caddy](https://caddyserver.com/))  
:fire: NGINX webserver support [NGINX](https://nginx.org/)  
:hammer: Fully production-ready through Helm Chart  
:pencil: Adjustable to your needs via environment variable  

## Get some Kubernetes + Helm with that!

Introducing our very own [Helm Chart](https://github.com/invoiceninja/dockerfiles/tree/master/charts/invoiceninja) that helps you launch a simple standalone app to a production-ready, highly available Invoice Ninja setup. All you need to do is initialise Kubernetes (available with Docker Desktop), install [Helm](https://helm.sh/docs/intro/install/), and spin up Invoice Ninja using the steps provided [here](https://github.com/invoiceninja/dockerfiles/tree/master/charts/invoiceninja#installing-the-chart).

Other resources:

[Helm Chart](https://github.com/Saddamus/invoiceninja-helm) by @Saddamus  
[K8s Manifest](https://github.com/invoiceninja/dockerfiles/issues/94) by @spacepluk
[You Tube installation video by DBTech](https://www.youtube.com/watch?v=xo6a3KtLC2g&ab_channel=DBTech)

## Alternatively get started with Docker Compose

The dockerfile has been revamped to make it easier to get started, by default the base image selected is 5 which will pull in the latest v5 stable image.

```bash
git clone https://github.com/invoiceninja/dockerfiles.git
cd dockerfiles
```

Instead of defining our environment variables inside our docker-compose.yml file we now define this in the `env` file, open this file up and insert your `APP_URL`, `APP_KEY` and update the rest of the variables as required.

```
APP_URL=http://in.localhost:8003/
APP_KEY=<insert your generated key in here>
APP_DEBUG=true
REQUIRE_HTTPS=false
IN_USER_EMAIL=
IN_PASSWORD=
```

If `IN_USER_EMAIL` and `IN_PASSWORD` is not set the default user email and password is "admin@example.com" and "changeme!" respectively. You will use this for the initial login, thereafter, you can delete this two environment variables.

The `APP_KEY` can be generated by running

```bash
docker run --rm -it invoiceninja/invoiceninja php artisan key:generate --show
```

Copy the entire string and insert in the env file at `APP_KEY=base64....`

To ensure folder permissions are correct when the container comes up for the first time it is important that you set the correct folder permissions on the `docker` folder.

From the terminal run

```bash
chmod 755 docker/app/public
sudo chown -R 1500:1500 docker/app
```

### Note for people running the container locally on their PC ###

If you are running the container locally, then the container will need to resolve the host, to support this you will want to insert your LAN IP address and the host name in the hosts file located in ```config/hosts```

For example, lets say your APP_URL is ```http://in5.test:8000``` and your LAN IP is 192.168.0.124 the hosts file will have an entry looking like this:


```192.168.0.124 in5.test```

**Please note that for PDF generation using local host, your domain name MUST end in .test for your PDFs to generate correctly, this is a DNS resolver issue with chromium.

All that is left to do now is bring up the container


``` docker-compose up -d```


**Note: When performing the setup, the Database host is ```db```

### Running on ARM64 (Raspberry Pi 4)

When deploying on an ARM64 system, you need to comment out the `image: mysql:5` line and uncomment `image: mariadb:10.4` in the `docker-compose.yml` file.

### Updating the Image when using `docker-compose`

As `docker-compose` does not support any form of version control, this git provide updates to `docker-compose.yml` directly.

To upgrade to a newer release image, please make sure to update the `docker-compose.yml` first by running

```bash
git pull
```

You may need to manually merge any changes that cannot be merged automatically by git.

### Thanks
Massive thank you to [lwj5](https://github.com/lwj5) for the tireless work to continually improve the dockerfile and its associated tooling.


## Support

If you discover a bug, please create and issue, if you query is general in nature please visit us on our [Forum ](https://forum.invoiceninja.com/)
