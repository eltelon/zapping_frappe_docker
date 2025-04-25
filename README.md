[![Build Stable](https://github.com/frappe/frappe_docker/actions/workflows/build_stable.yml/badge.svg)](https://github.com/frappe/frappe_docker/actions/workflows/build_stable.yml)
[![Build Develop](https://github.com/frappe/frappe_docker/actions/workflows/build_develop.yml/badge.svg)](https://github.com/frappe/frappe_docker/actions/workflows/build_develop.yml)

Everything about [Frappe](https://github.com/frappe/frappe) and [ERPNext](https://github.com/frappe/erpnext) in containers.

# Getting Started

To get started you need [Docker](https://docs.docker.com/get-docker/), [docker-compose](https://docs.docker.com/compose/), and [git](https://docs.github.com/en/get-started/getting-started-with-git/set-up-git) setup on your machine. For Docker basics and best practices refer to Docker's [documentation](http://docs.docker.com).

Once completed, chose one of the following two sections for next steps.

### Try in Play With Docker

To play in an already set up sandbox, in your browser, click the button below:

<a href="https://labs.play-with-docker.com/?stack=https://raw.githubusercontent.com/frappe/frappe_docker/main/pwd.yml">
  <img src="https://raw.githubusercontent.com/play-with-docker/stacks/master/assets/images/button.png" alt="Try in PWD"/>
</a>

### Try on your Dev environment

First clone the repo:

```sh
git clone https://github.com/frappe/frappe_docker
cd frappe_docker
```

Then run: `docker compose -f pwd.yml up -d`

### To run on ARM64 architecture follow this instructions

After cloning the repo run this command to build multi-architecture images specifically for ARM64.

`docker buildx bake --no-cache --set "*.platform=linux/arm64"`

and then

- add `platform: linux/arm64` to all services in the `pwd.yml`
- replace the current specified versions of erpnext image on `pwd.yml` with `:latest`

Then run: `docker compose -f pwd.yml up -d`

## Final steps

Wait for 5 minutes for ERPNext site to be created or check `create-site` container logs before opening browser on port 8080. (username: `Administrator`, password: `admin`)

If you ran in a Dev Docker environment, to view container logs: `docker compose -f pwd.yml logs -f create-site`. Don't worry about some of the initial error messages, some services take a while to become ready, and then they go away.

# Documentation

### [Frequently Asked Questions](https://github.com/frappe/frappe_docker/wiki/Frequently-Asked-Questions)

### [Production](#production)

- [List of containers](docs/list-of-containers.md)
- [Single Compose Setup](docs/single-compose-setup.md)
- [Environment Variables](docs/environment-variables.md)
- [Single Server Example](docs/single-server-example.md)
- [Setup Options](docs/setup-options.md)
- [Site Operations](docs/site-operations.md)
- [Backup and Push Cron Job](docs/backup-and-push-cronjob.md)
- [Port Based Multi Tenancy](docs/port-based-multi-tenancy.md)
- [Migrate from multi-image setup](docs/migrate-from-multi-image-setup.md)
- [running on linux/mac](docs/setup_for_linux_mac.md)
- [TLS for local deployment](docs/tls-for-local-deployment.md)

### [Custom Images](#custom-images)

- [Custom Apps](docs/custom-apps.md)
- [Custom Apps with podman](docs/custom-apps-podman.md)
- [Build Version 10 Images](docs/build-version-10-images.md)

### [Development](#development)

- [Development using containers](docs/development.md)
- [Bench Console and VSCode Debugger](docs/bench-console-and-vscode-debugger.md)
- [Connect to localhost services](docs/connect-to-localhost-services-from-containers-for-local-app-development.md)

### [Troubleshoot](docs/troubleshoot.md)

# Contributing

If you want to contribute to this repo refer to [CONTRIBUTING.md](CONTRIBUTING.md)

This repository is only for container related stuff. You also might want to contribute to:

- [Frappe framework](https://github.com/frappe/frappe#contributing),
- [ERPNext](https://github.com/frappe/erpnext#contributing),
- [Frappe Bench](https://github.com/frappe/bench).



#### ZAPPING ####

#Readme enriched by Zapping

#This tutorial has been created using https://discuss.frappe.io/t/tutorial-erpnext-15-setup-docker/112103



#USE THIS CONFIGURATION IT YOU WANT SKIP REDIS CONFIGURATION
#Configuration 
```
$ nvm use v18
$ PYENV_VERSION=3.10.13 bench init \
  --skip-redis-config-generation \
  --frappe-branch version-15 \
  frappe-bench
```

$ cd frappe-bench/
#It's possible configure redis in a config like this:
```
{
  "redis_cache": "redis://:password@host_redis:6379/0",
  "redis_queue": "redis://:password@host_redis:6379/1",
  "redis_socketio": "redis://:password@host_redis:6379/2"
}
```

#However it's possible configure redis using bench command

```
$ bench set-config -g db_host mariadb
$ bench set-config -g redis_cache redis://redis-cache:6379
$ bench set-config -g redis_queue redis://redis-queue:6379
$ bench set-config -g redis_socketio redis://redis-queue:6379
```

#To configure a site called zapping.local, it's posible using this command:
```
#$ bench new-site erp.zapping.live --db-type mariadb --db-host host --db-port port --db-name erp_zapping --db-root-username erpnext --db-password 123

$ bench new-site --mariadb-root-password 123 --admin-password admin --no-mariadb-socket erp.zapping.live
```

#Commands getting custom app from a private repository
```
$ bench get-app erpnext https://github.com/eltelon/zapping_frappe_erpnext
$ bench get-app payments https://github.com/eltelon/zapping_frappe_payments
```


$ bench get-app --branch version-15 erpnext
$ bench get-app --branch version-15 https://github.com/frappe/payments.git

#Commands to install custom apps in your site
$ bench use erp.zapping.live #this command setted your default site

$ bench install-app erpnext
$ bench install-app payments

#Comandos para obtener una app de un repositorio
$ bench get-app erpnext https://github.com/eltelon/zapping_frappe_erpnext
$ bench get-app payments https://github.com/eltelon/zapping_frappe_payments

##Extras
#Command to update a custom_app
$ bench --site name_of_site migrate

#If you make changes that require DocType, fixtures, or migrations, make sure you have them versioned and exported with:
$ bench --site name_of_site export-fixtures --app name_of_custom_app

#Sometimes it's necessary compile frontend if you make some changes:
$ bench build

