# Wildfly Showcase

This is a project based on the [Wildfly](https://wildfly.org) application server. It contains a hello world application, which demonstrates features of Wildfly and Eclipse Microprofile

Software requirements to run the samples are `maven`, `openjdk-1.8` (or any other 1.8 JDK) and `docker`.
When running the Maven lifecycle it will create the war package. The war will be copied into a
Docker image using Spotify's `dockerfile-maven-plugin` during the package phase.

## How to run

Before running the application it needs to be compiled and packaged using Maven. It creates the required war,
jar and Docker image and can be run via `docker`:

```shell script
$ mvn clean package
$ docker run --rm -p 8080:8080 ${artifactId}
```

Wait for a message log similar to this:

> 19:19:21,704 INFO  [org.jboss.as] (Controller Boot Thread) WFLYSRV0025: WildFly Full 19.0.0.Final (WildFly Core 11.0.0.Final) started in 7429ms - Started 308 of 470 services (240 services are lazy, passive or on-demand)

If everything worked you can access the OpenAPI UI via http://localhost:8080/swagger-ui.

## Resolving issues

Sometimes it may happen that the containers did not stop as expected when trying to stop the pipeline early. This may
result in running containers although they should have been stopped and removed. To detect them you need to check
Docker:

```shell script
$ docker ps -a | grep ${artifactId}
```

If there are containers remaining although the application has been stopped you can remove them:

```shell script
$ docker rm <ids of the containers>
```