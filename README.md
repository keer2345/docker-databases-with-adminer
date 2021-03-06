# Play Databases with Adminer and Docker
## Usage
```
cp .env.example .env
docker-compose up -d
docker-compose down -v
```


<!-- vim-markdown-toc GFM -->

* [Prepare the Environment:](#prepare-the-environment)
    * [Versions used of this Software](#versions-used-of-this-software)
    * [Environment Variable](#environment-variable)
* [Adminer](#adminer)
* [Postgres](#postgres)
    * [Configuration](#configuration)
    * [Connection with Adminer and Postgres](#connection-with-adminer-and-postgres)
* [MySQL](#mysql)
    * [Configuration](#configuration-1)
* [MongoDB](#mongodb)
    * [Configuration](#configuration-2)
    * [Connection with Adminer and MongoDB](#connection-with-adminer-and-mongodb)
* [Global file](#global-file)

<!-- vim-markdown-toc -->

## Prepare the Environment:

> https://medium.com/@etiennerouzeaud/play-databases-with-adminer-and-docker-53dc7789f35f

### Versions used of this Software
- Docker : >= 17.12.0-ce ;
- Docker Compose : >= 1.17.0 ;
- Adminer : >= 4.5.0 ;
- Postgres : 10 ;
- MySQL : 8.0.3 ;
- MongoDB : 3.6.2

### Environment Variable
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

Access to `http://127.0.0.1:8001` . Working but we don’t have database server yet.

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
### Connect to Postgres with Spring Boot
For example, config `application.properties`:
```
spring.datasource.url=jdbc:postgresql://localhost:54320/springboot
spring.datasource.username=root
spring.datasource.password=123456
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.properties.hibernate.format_sql=true
```

### Connection to Postgres with pgAdmin

- Access to http://127.0.0.1:8002
- Login with user `test@123.com` and password `123456`

``` yml
  pgadmin:
    image: dpage/pgadmin4:5.1
    container_name: ${PGADMIN_CONTAINER_NAME}
    restart: always
    ports:
      - ${PGADMIN_PORT}:8080/tcp
    environment: 
      - PGADMIN_LISTEN_ADDRESS=0.0.0.0
      - PGADMIN_LISTEN_PORT=8080
      - PGADMIN_DEFAULT_EMAIL=${PGADMIN_DEFAULT_EMAIL}
      - PGADMIN_DEFAULT_PASSWORD=${PGADMIN_DEFAULT_PASSWORD}
```

![](https://raw.githubusercontent.com/keer2345/docker-databases-with-adminer/master/images/pgadmin.png)

### Connection to Postgres with Adminer

Go back in Adminer (http://localhost:8001). Then complete the form :
- *System* : select “PostgresSQL” ;
- *Server* : type “dbPostgres” ;
- *Username* : type “root” ;
- *Password* : type “123456” ;
- *Database* : type “pgDB” ;

![](https://raw.githubusercontent.com/keer2345/docker-databases-with-adminer/master/images/postgre_login.png)
![](https://raw.githubusercontent.com/keer2345/docker-databases-with-adminer/master/images/postgre_welcome.png)


## MySQL
### Configuration
```
    dbMysql:
        image: mysql:8.0.15
        command: --default-authentication-plugin=mysql_native_password
        container_name: ${MYSQL_CONTAINER_NAME}
        restart: always
        volumes:
            - ${MYSQL_DATA_DIR}:/var/lib/mysql
        ports:
            - ${MYSQL_PORT}:3306
        environment:
            - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
            - MYSQL_DATABASE=${MYSQL_DB}
```

## MongoDB
### Configuration
### Connection with Adminer and MongoDB

## Global file
We can see the global file `docker-compose.yml` .
