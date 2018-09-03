# Play Databases with Adminer and Docker


<!-- vim-markdown-toc GFM -->

* [Reference:](#reference)
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
* [Environment Variable](#environment-variable)
* [Global file](#global-file)

<!-- vim-markdown-toc -->

## Reference:
https://medium.com/@etiennerouzeaud/play-databases-with-adminer-and-docker-53dc7789f35f

Versions used of this software:
- Docker : >= 17.12.0-ce ;
- Docker Compose : >= 1.17.0 ;
- Adminer : >= 4.5.0 ;
- Postgres : 10 ;
- MySQL : 8.0.3 ;
- MongoDB : 3.6.2

## Adminer
`docker-compose.yml` :
```
version: '3'

services:
    adminer:
        image: dockette/adminer:full-php5
        restart: always
        ports:
            - ${ADMINER_PORT}:80


```
> We can see the environment variable such as `${ADMINER_PORT}` in the `.env` file.
>
> Why don’t we use the official Adminer image ? You can use it, but it will not works with Mongo and PHP7.


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
        restart: always
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

## Environment Variable
We can see the environment variable file `.env` .
## Global file
We can see the global file `docker-compose.yml` .
