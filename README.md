# Docker 
This project will be an introduction to using the Docker platform with a PostgreSQL database. After creating a new 
Spring Boot project, I checked that it ran correctly with the commands 

```
./gradlew test 
./gradlew bootRun # launced on http://localhost:8080 in browser
./gradlew bootJar # application jar in build/directory
```
I checked that I already had Docker and PostgresSQL installed in my computer.
```
$ docker -v
Docker version 27.1.1, build 6312585

$ docker pull postgres
Using default tag: latest
latest: Pulling from library/postgres
Digest: sha256:4ec37d2a07a0067f176fdcc9d4bb633a5724d2cc4f892c7a2046d054bb6939e5
Status: Image is up to date for postgres:latest
docker.io/library/postgres:latest
```
Initially, I had no running Docker containers: 
```
$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```
By default, PostgreSQL uses port 5432, so I mapped the port on both host and container with the argument `-p 5432:5432`,
and set the username and password with the environmental argument `-e`.

```
docker run -p 5432:5432 \
           -e POSTGRES_USER=myusername \
           -e POSTGRES_PASSWORD=mypassword \
           -d --name my-postgres --rm postgres
```
After which, I could see my running Docker container: 
```
$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                    NAMES
23fe9671801e   postgres   "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds   0.0.0.0:5432->5432/tcp   my-postgres
```
I connected the database to the built-in client in IntelliJ by clicking on the database icon on the right-side menu, 
then "Create Source" and adding the correct fields for my database. 