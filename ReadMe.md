# Karaf Camel REST / SQL QuickStart

This example demonstrates how to use SQL via JDBC along with Camel's REST DSL to expose a RESTful API.

This example relies on the [Fabric8 Maven plugin](https://maven.fabric8.io) for its build configuration.

### Building

The example can be built with:

    $ mvn install

This automatically generates the application resource descriptors and builds the Docker image, so it requires access to a Docker daemon, relying on the `DOCKER_HOST` environment variable by default.

### Running the example in Fabric8

It is assumed a Kubernetes platform is already running. If not, you can find details how to [get started](http://fabric8.io/guide/getStarted/index.html).

Besides, it is assumed that a MySQL service is already running on the platform. You can deploy it using the provided deployment by executing:

    $ kubectl create -f mysql-deployment.yml

The example can be deployed using a single goal:

    $ mvn fabric8:deploy

This deploys the Kubernetes resource descriptors previously generated to the orchestration platform.

When the example runs in Kubernetes, you can use the Kubernetes client tool to inspect the status, e.g.:

- To list all the running pods:
    ```
    $ kubectl get pods
    ```

- Then find the name of the pod that runs this example, and output the logs from the running pod with:
    ```
    $ kubectl logs <name of pod>
    ```

You can also use the Fabric8 [Web console](http://fabric8.io/guide/console.html) to manage the running pods, view logs and much more.

### Accessing the REST service

When the example is running, a REST service is available to list the books that can be ordered, and as well the order statuses.

If you run the example on a local Fabric8 installation using Vagrant, then the REST service is exposed at <http://qs-camel-rest-sql.vagrant.f8>.

The actual endpoint is using the _context-path_ `camel-rest-sql/books` and the REST service provides two services:

- `books`: to list all the available books that can be ordered,
- `order/{id}`: to output order status for the given order `id`.

The example automatically creates new orders with a running order `id` starting from 1.

You can then access these services from your Web browser, e.g.:

- <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/books>
- <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/books/order/1>

### Swagger API

The example provides API documentation of the service using Swagger using the _context-path_ `camel-rest-sql/api-doc`. You can access the API documentation from your Web browser at <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/api-doc>.

### More details

You can find more details about running this [quickstart](http://fabric8.io/guide/quickstarts/running.html) on the website. This also includes instructions how to change the Docker image user and registry.

### add feature repo
```
$ feature:repo-add mvn:io.fabric8/fabric8-karaf-features/3.0.11/xml/features
$ feature:repo-add camel 2.21.1
```
### install feature
```
$ feature:install transaction jndi jdbc pax-jdbc-config pax-jdbc-pool-dbcp2
$ feature:install camel-blueprint camel-servlet camel-sql camel-jackson camel-swagger-java
$ feature:install fabric8-karaf-blueprint fabric8-karaf-checks
$ install -s mvn:org.postgresql/postgresql/9.4-1202-jdbc41
```

### deploy jar to deploy folder

