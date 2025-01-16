# fx-fullend: start via docker fx-backend + fx-frontend   🚀

This template should help get you started developing with Docker to run backend and frontend projects.

Let's run via Docker two projects:
1. [fx-backend](https://github.com/mgsviryd/fx-backend.git) : Spring Boot 3 + REST Api + JPA 
2. [fx-frontend](https://github.com/mgsviryd/fx-frontend.git): Vue 3 + Vite
---

## Environment

#### Mac OS
| Name                              | Description                                                                                                                                                                            |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [brew](https://brew.sh/)          | Manager to install and control versions of packages, e.g. `jdk` and `maven`<br/> ```/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"``` |
| [nano](https://brew.sh/)                            | Command-line editor<br/> `brew install nano`                                                                                                                                 |
| [git](https://git-scm.com/)    | Version control system<br/>```brew install git```                                                                                                                                      |
| [docker](https://www.docker.com/) | Platform designed to help developers build, share, and run container applications<br/>```brew install docker```                                                                        |

#### PC
| Name                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [chocolatey](https://chocolatey.org/) | Manager to install and control versions of packages, e.g. `jdk` and `maven`<br/>```/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"<br/>Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))``` |
| [nano](https://chocolatey.org/)                      | Command-line editor<br/>```choco install nano```                                                                                                                                                                                                                                                                                                                                                                                                                   |
| [git](https://git-scm.com/)         | Version control system<br/>```choco install git -y``` |
| [docker](https://www.docker.com/)     | Platform designed to help developers build, share, and run container applications<br/>```choco install docker-desktop```                                                                                                                                                                                                                                                                                                                                           |

---
## Variants
> Describe 2 variants to start project via docker:
1. mysql on your localhost (use `docker-compose.yml` and `.env` files)
2. mysql on your docker (use `docker-compose.mysqldb.yml` and `.env.mysqldb` files)


## Pre-setup

### Clone git repository
```shell
git clone https://github.com/mgsviryd/fx-fullend.git    
```
### Go to directory
```shell
cd fx-fullend
```
### Clone git repositories
```shell
git clone https://github.com/mgsviryd/fx-backend.git    
git clone https://github.com/mgsviryd/fx-frontend.git    
```

### Create .env file and set environment variables
#### Variant 1
Create .env file manually or use next command-line:
```shell
nano .env
```
Paste and edit <your_db_name>, <your_mysql_username>, <your_mysql_password>:

```
SPRING_DATASOURCE_URL=jdbc:mysql://host.docker.internal:3306/<your_db_name>?serverTimezone=UTC&allowPublicKeyRetrieval=true&useSSL=false&createDatabaseIfNotExist=true
SPRING_DATASOURCE_USERNAME=<your_mysql_username>
SPRING_DATASOURCE_PASSWORD=<your_mysql_password>
```

#### Variant 2
Create .env.mysqldb file manually or use next command-line:
```shell
nano .env.mysqldb
```
Paste and edit <your_docker_db_name>, <your_docker_mysql_username>, <your_docker_mysql_password>):

```
MYSQL_DATABASE=<your_docker_db_name>
MYSQL_PORTS=3307:3306
SPRING_DATASOURCE_URL=jdbc:mysql://mysqldb:3306/<your_docker_db_name>?serverTimezone=UTC&allowPublicKeyRetrieval=true&useSSL=false&createDatabaseIfNotExist=true
SPRING_DATASOURCE_USERNAME=<your_docker_mysql_username>
SPRING_DATASOURCE_PASSWORD=<your_docker_mysql_password>
```

### Check your ports
By default, we use next ports: 
 - 3306 - for mysql (must be open) - for Variant 1,
 - 3307 - for forward traffic to the mysqldb container (it listens traffic on own 3306) (must be close) - for Variant 2,
 - 8080 - for backend (must be close), 
 - 8081 - for frontend (must be close), you can choose another one
```shell
nc -zv localhost 3306
```
**Status:** ✅ Success
```shell
nc -zv localhost 3307
```
**Status:** 🚫 Refused
```shell
nc -zv localhost 8080
```
**Status:** 🚫 Refused
```shell
nc -zv localhost 8081
```
**Status:** 🚫 Refused

---

## Setup (via Docker)
⚠️ do [Pre-setup](#pre-setup) first

### Start docker daemon
On a typical installation the Docker daemon is started by a system utility. For more information see [here](https://docs.docker.com/engine/daemon/start/).


### Variant 1
#### Build docker image and run docker container
```shell
docker compose -f docker-compose.yml --env-file .env up --build
```
#### Done
[http://localhost](http://localhost)

#### Stop and remove docker container and docker image
```shell
docker compose -f docker-compose.yml --env-file .env down --rmi 'all'
```


### Variant 2
#### Build docker image and run docker container
```shell
docker compose -f docker-compose.mysqldb.yml --env-file .env.mysqldb up --build
```
#### Done
[http://localhost](http://localhost)

#### Stop and remove docker container and docker image
```shell
docker compose -f docker-compose.mysqldb.yml --env-file .env.mysqldb down --rmi 'all'
```


---
