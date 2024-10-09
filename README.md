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
amali@MampendaPC MINGW64 ~/IdeaProjects/DAT250/docker-intro/docker-intro (main)
$ docker run -p 5432:5432 -e POSTGRES_USER=myusername -e POSTGRES_PASSWORD=mypassword -e POSTGRES_DB=mydatabase -d --name my-postgres --rm postgres postgres
dfbd112eca53a482da5d4b289cfa977dcc3a85e04a61142ba47381ce420d2857
```
After which, I could see my running Docker container: 
```
$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                    NAMES
23fe9671801e   postgres   "docker-entrypoint.s…"   17 seconds ago   Up 16 seconds   0.0.0.0:5432->5432/tcp   my-postgres
```
Once this was done, I opened `Settings` > `Build, Execution, Deployment` > `Docker` and clicked on the "Add"-button 
(plus) sign in the upper left corner. IntelliJ detected that I was running Docker Desktop and sat all the fields for me,
so I could see the message "Connection successful" in the lower left corner. I clicked "Apply" and "OK" and opened 
`Services` where I could see the Docker daemon running and clicked "connect" (green play button).

I was unable to connect to the running Docker container, so I stopped and deleted it. Then, opened `Services` > `Docker` >
`Images` in IntelliJ again and right-clicked `Images`, where a drop-down menu appeared with the option to 
"Create container". A new window opened, prompting for me to input some values into the fields. So I filled it in 
accordingly: 

![img.png](img.png)

This did not work either because I kept getting errors when trying to enter the container bash. 

```
psql: FATAL: role "postgres" does not exist
psql: FATAL: role "my-username" does not exist
psql: FATAL: role "user" does not exist
```

So, I deleted the container and tried again:
```
amali@MampendaPC MINGW64 ~
$ docker run -p 5432:5432 -e POSTGRES_PASSWORD=mypassword -d --name my-postgres --rm postgres postgres 
73261b55aedb388fc031eff8a908f30b6d2df10f1efe645d816dae17dc91956b

amali@MampendaPC MINGW64 ~
$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                    NAMES
73261b55aedb   postgres   "docker-entrypoint.s…"   26 seconds ago   Up 25 seconds   0.0.0.0:5432->5432/tcp   my-postgres

amali@MampendaPC MINGW64 ~
$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                    NAMES
73261b55aedb   postgres   "docker-entrypoint.s…"   43 seconds ago   Up 42 seconds   0.0.0.0:5432->5432/tcp   my-postgres

amali@MampendaPC MINGW64 ~
$ winpty docker exec -it my-postgres bash
root@73261b55aedb:/# pwd
/
root@73261b55aedb:/# psql
psql: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  role "root" does not exist
root@73261b55aedb:/# psql -U postgres
psql (17.0 (Debian 17.0-1.pgdg120+1))
Type "help" for help.

postgres=#

```

