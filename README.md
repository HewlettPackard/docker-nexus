# HewlettPackard/docker-nexus

Docker images for Sonatype Nexus with the Oracle JDK.

To build:
```
# docker build --rm --tag HewlettPackard/docker-nexus oss/

```

To run (if port 8081 is open on your host):

```
# docker run -d -p 8081:8081 --name nexus HewlettPackard/docker-nexus
```

or to assign a random port that maps to port 8081 on the container:

```
# docker run -d -p 8081 --name nexus HewlettPackard/docker-nexus
```

To determine the port that the container is listening on:

```
# docker ps nexus
```

To test:

```
$ curl http://localhost:8081/service/extdirect/poll/rapture_State_get
```

To build:

Copy the Dockerfile and do the build-

```
$ docker build --rm=true --tag=HewlettPackard/docker-nexus .
```


## Notes

* Default credentials are: `admin` / `admin123`

* It can take some time (2-3 minutes) for the service to launch in a
new container.  You can tail the log to determine once Nexus is ready:

```
$ docker logs -f nexus
```

