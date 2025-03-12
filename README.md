# Getting Started

## Prerequisites

In order to start developing you need to satisfy the following prerequisites:

- Docker
- docker-compose
- user added to docker group

## Use VSCode Remote Containers extension

For most people getting started with Frappe development, the best solution is to use [VSCode Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers).

Before opening the folder in container, determine the database that you want to use. The default is MariaDB.
If you want to use PostgreSQL instead, edit `.devcontainer/docker-compose.yml` and uncomment the section for `postgresql` service, and you may also want to comment `mariadb` as well.

VSCode should automatically inquire you to install the required extensions, that can also be installed manually as follows:

- Install Dev Containers for VSCode
  - through command line `code --install-extension ms-vscode-remote.remote-containers`
  - clicking on the Install button in the Vistual Studio Marketplace: [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
  - View: Extensions command in VSCode (Windows: Ctrl+Shift+X; macOS: Cmd+Shift+X) then search for extension `ms-vscode-remote.remote-containers`

## Bootstrap Containers for development

```
git clone https://github.com/frappe/frappe_docker.git
cd frappe_docker
cp -R devcontainer-example .devcontainer
cp -R development/vscode-example development/.vscode
```

## Download VS Code & Remote Container Extenstion

```
code .
```

## Setup first bench

```
sudo chmod 777 /workspace/development
bench init --skip-redis-config-generation --frappe-branch version-14 frappe-bench
```

## Setup hosts

```
bench set-config -g db_host mariadb
bench set-config -g redis_cache redis://redis-cache:6379
bench set-config -g redis_queue redis://redis-queue:6379
bench set-config -g redis_socketio redis://redis-queue:6379
```

## Create a new site with bench

```
bench new-site --mariadb-root-password 123 --admin-password admin --no-mariadb-socket development.localhost
```

## Set bench developer mode on the new site

```
bench --site development.localhost set-config developer_mode 1
bench --site development.localhost clear-cache
```

## Install an app

```
bench get-app --branch version-14 --resolve-deps erpnext
bench --site development.localhost install-app erpnext
```
## Start Frappe without debugging

`bench start`

## Brows your ERPNext now 

http://development.localhost:8000
