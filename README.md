# Play Databases with Adminer and Docker


<!-- vim-markdown-toc GFM -->

* [Prepare the Environment:](#prepare-the-environment)
* [Environment Variable](#environment-variable)
* [Adminer](#adminer)
* [Postgres](#postgres)
    * [Configuration](#configuration)
    * [Connection with Adminer and Postgres](#connection-with-adminer-and-postgres)
* [MySQL](#mysql)
    * [Configuration](#configuration-1)
    * [Connection with Adminer and MySQL](#connection-with-adminer-and-mysql)
* [MongoDB](#mongodb)
    * [Configuration](#configuration-2)
    * [Connection with Adminer and MongoDB](#connection-with-adminer-and-mongodb)
* [Global file](#global-file)

<!-- vim-markdown-toc -->

## Prepare the Environment:
https://medium.com/@etiennerouzeaud/play-databases-with-adminer-and-docker-53dc7789f35f

Versions used of this software:
- Docker : >= 17.12.0-ce ;
- Docker Compose : >= 1.17.0 ;
- Adminer : >= 4.5.0 ;
- Postgres : 10 ;
- MySQL : 8.0.3 ;
- MongoDB : 3.6.2

## Environment Variable
```
cp .env.example .env
```
We can see the environment variable file `.env` .

## Adminer
`docker-compose.yml` :
```
version: '3'

services:
    adminer:
        image: dockette/adminer:full-php5
        restart: always
        container_name: ${ADMINER_CONTAINER_NAME}
        ports:
            - ${ADMINER_PORT}:80


```
*Note* :
> 1. We can see the environment variable such as `${ADMINER_CONTAINER_NAME}` and `${ADMINER_PORT}` in the `.env` file.
> 1. Why don’t we use the official Adminer image ? You can use it, but it will not works with Mongo and PHP7.

Now, you can run this container with the command:
```
docker-compose up
```

Working but we don’t have database server yet.

## Postgres
### Configuration
```
version: '3'

services:
    adminer:
        # ...

    dbPostgres:
        image: postgres:10
        container_name: ${POSTGRES_CONTAINER_NAME}
        restart: always
        volumes:
            - ${POSTGRES_DATA_DIR}:/var/lib/postgresql/data
        ports:
            - ${POSTGRES_PORT}:5432
        environment:
            - POSTGRES_DB=${POSTGRES_DB}
            - POSTGRES_USER=${POSTGRES_USER}
            - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
```

### Connection with Adminer and Postgres
Go back in Adminer (http://localhost:8001). Then complete the form :
- *System* : select “PostgresSQL” ;
- *Server* : type “dbPostgres” ;
- *Username* : type “root” ;
- *Password* : type “123456” ;
- *Database* : type “pgDB” ;


## MySQL
### Configuration
### Connection with Adminer and MySQL
## MongoDB
### Configuration
### Connection with Adminer and MongoDB

## Global file
We can see the global file `docker-compose.yml` .
