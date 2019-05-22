# Todos Edge

An edge for sample Todo Spring Boot apps.

* [Spring Boot](https://spring.io/projects/spring-boot) for app bits
* Specifically [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway) to handle the heavy lifting

## First things first

This application is 1 of 3.  The other 2 ([UI](https://github.com/corbtastik/todos-webui) & [API](https://github.com/corbtastik/todos-api)) stand on their own as samples, but this one depends on at least those.  The API is swappable with any HTTP todos CRUD API.  See [todos-api](https://github.com/corbtastik/todos-api) for a reference.

First clone and run the [UI](https://github.com/corbtastik/todos-webui) and [API](https://github.com/corbtastik/todos-api) before running this app.

## Run on PCF

1. Consider forking [this project](https://github.com/corbtastik/todos-edge) then clone to dev machine
1. cd into project
1. mvnw clean package
1. modify `manifest.yml` for your cloudfoundry tastes (custom route perhaps?)
1. configure `todos.api.endpoint` and `todos.ui.endpoint` in `manifest.yml`, note PCF will hydrate the container with the envars and Spring Boot will apply to exposed properties where the naming conventions match (snake_case to property.case).
    ```yaml
    ---
    applications:
    - name: todos-edge
      memory: 1G
      routes:
      - route: todos-edge.apps.retro.io
      - route: todos.apps.retro.io  
      path: target/todos-edge-1.0.0.SNAP.jar
      env:
        TODOS_UI_ENDPOINT: https://todos-webui.apps.retro.io
        TODOS_API_ENDPOINT: https://todos-redis.apps.retro.io
    ```
1. login to PCF (or [PWS](https://run.pivotal.io/))
1. cf push (awwwweee yeah)

Once the app is running access with the route provided in the manifest.  If the UI and API are configured you should see the api enabled app.

**UI running**

<p align="center">
    <img src="https://github.com/corbtastik/todos-images/raw/master/todos-webui/ui.png" width="640">
</p>

## Local

You can clone, build, run then access ``localhost:9999`` or change the port.  But first clone and run the [UI](https://github.com/corbtastik/todos-webui) and [API](https://github.com/corbtastik/todos-api).

```bash
java -jar ./target/todos-edge-1.0.0.SNAP.jar \
  --todos.api.endpoint=http://localhost:8080 \
  --todos.ui.endpoint=http://localhost:4040 \  
  --server.port=whatever
``` 