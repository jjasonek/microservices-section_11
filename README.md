# Udemy Course Microservices with Spring Boot, Docker, Kubernetes Section_11
https://www.udemy.com/course/master-microservices-with-spring-docker-kubernetes/
## spring version: 3.4.5
## gatewayservice version: 3.5.0
## Java 21


## Documentation
After adding the library, the swagger page is accessible through address 
http://localhost:8080/swagger-ui/index.html,
http://localhost:8090/swagger-ui/index.html,
http://localhost:9000/swagger-ui/index.html.

## links

### Eureka Server dashboard
http://localhost:8070/

### Link from Eureka Server to Gateway Server
http://172.24.64.1:8072/actuator/info
{
    "app": {
        "name": "gatewayserver",
        "description": "Eazy Bank Gateway Server Application",
        "version": "1.0.0"
    }
}

### Further links
http://localhost:8072/actuator
http://localhost:8072/actuator/gateway
http://localhost:8072/actuator/gateway/routes

### Example invoking an service:
POST http://localhost:8072/eazybank/accounts/api/create


### actuator links:
http://localhost:8072/actuator
http://localhost:8071/actuator
http://localhost:8070/actuator
http://localhost:8080/actuator
http://localhost:8090/actuator
http://localhost:9000/actuator


## Services

### run Redis server
docker run -p 6379:6379 --name eazyredis -d redis


## Grafana

### Downloads instead of curl:
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/grafana/loki/main/examples/getting-started/loki-config.yaml" -OutFile "loki-config.yaml"
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/grafana/loki/main/examples/getting-started/alloy-local-config.yaml" --output "alloy-local-config.yaml"
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/grafana/loki/main/examples/getting-started/docker-compose.yaml" -OutFile "docker-compose.yaml"

### for all microservices we call following to generate docker images s11:
mvn compile jib:dockerBuild

docker image ls --filter=reference="jjasonek/*:s11"
REPOSITORY               TAG       IMAGE ID       CREATED        SIZE
jjasonek/cards           s11       af786fbc14c8   55 years ago   374MB
jjasonek/configserver    s11       755f84749b36   55 years ago   331MB
jjasonek/accounts        s11       38ff0820cd84   55 years ago   376MB
jjasonek/loans           s11       57bdb8d2f3d9   55 years ago   374MB
jjasonek/gatewayserver   s11       9719f3c329c0   55 years ago   345MB
jjasonek/eurekaserver    s11       f59a03a7eb30   55 years ago   344MB

Alternatively you can use (on Linux):
docker images | grep s10

### push images to docker hub:
docker image push docker.io/jjasonek/accounts:s11
docker image push docker.io/jjasonek/loans:s11
docker image push docker.io/jjasonek/cards:s11
docker image push docker.io/jjasonek/configserver:s11
docker image push docker.io/jjasonek/eurekaserver:s11
docker image push docker.io/jjasonek/gatewayserver:s11

### run docker compose
docker compose up -d
docker compose down

### Grafana 
http://localhost:3000


## Micrometer

### test /actuator/metrics

http://localhost:8080/actuator/metrics:
{
    "names": [
        "application.ready.time",
        "application.started.time",
        "disk.free",
        "disk.total",
        "executor.active",
        "executor.completed",
        "executor.pool.core",
        "executor.pool.max",
        "executor.pool.size",
        "executor.queue.remaining",
        "executor.queued",
        "hikaricp.connections",
        "hikaricp.connections.acquire",
        "hikaricp.connections.active",
        "hikaricp.connections.creation",
        "hikaricp.connections.idle",
        "hikaricp.connections.max",
        "hikaricp.connections.min",
        "hikaricp.connections.pending",
        "hikaricp.connections.timeout",
        "hikaricp.connections.usage",
        "http.client.requests",
        "http.client.requests.active",
        "http.server.requests.active",
        "jdbc.connections.active",
        "jdbc.connections.idle",
        "jdbc.connections.max",
        "jdbc.connections.min",
        "jvm.buffer.count",
        "jvm.buffer.memory.used",
        "jvm.buffer.total.capacity",
        "jvm.classes.loaded",
        "jvm.classes.unloaded",
        "jvm.compilation.time",
        "jvm.gc.live.data.size",
        "jvm.gc.max.data.size",
        "jvm.gc.memory.allocated",
        "jvm.gc.memory.promoted",
        "jvm.gc.overhead",
        "jvm.gc.pause",
        "jvm.info",
        "jvm.memory.committed",
        "jvm.memory.max",
        "jvm.memory.usage.after.gc",
        "jvm.memory.used",
        "jvm.threads.daemon",
        "jvm.threads.live",
        "jvm.threads.peak",
        "jvm.threads.started",
        "jvm.threads.states",
        "logback.events",
        "process.cpu.time",
        "process.cpu.usage",
        "process.start.time",
        "process.uptime",
        "system.cpu.count",
        "system.cpu.usage",
        "tomcat.sessions.active.current",
        "tomcat.sessions.active.max",
        "tomcat.sessions.alive.max",
        "tomcat.sessions.created",
        "tomcat.sessions.expired",
        "tomcat.sessions.rejected"
    ]
}

Now, we can see the particular metrics by its name from the above list:
http://localhost:8080/actuator/metrics/process.cpu.usage
{
    "name": "process.cpu.usage",
    "description": "The \"recent cpu usage\" for the Java Virtual Machine process",
    "measurements": [
        {
            "statistic": "VALUE",
            "value": 0.13436999606134667
        }
    ],
    "availableTags": [
        {
            "tag": "application",
            "values": [
                "accounts"
            ]
        }
    ]
}


### test /actuator/prometheus
http://localhost:8080/actuator/prometheus
# HELP application_ready_time_seconds Time taken for the application to be ready to service requests
# TYPE application_ready_time_seconds gauge
application_ready_time_seconds{application="accounts",main_application_class="com.eazybytes.accounts.AccountsApplication"} 13.062
# HELP application_started_time_seconds Time taken to start the application
# TYPE application_started_time_seconds gauge
application_started_time_seconds{application="accounts",main_application_class="com.eazybytes.accounts.AccountsApplication"} 13.054
# HELP disk_free_bytes Usable space for path
# TYPE disk_free_bytes gauge
disk_free_bytes{application="accounts",path="C:\\Training\\Microservices\\section_11\\."} 7.50710870016E11
# HELP disk_total_bytes Total space for path
# TYPE disk_total_bytes gauge
disk_total_bytes{application="accounts",path="C:\\Training\\Microservices\\section_11\\."} 1.02301411328E12
...

http://localhost:8090/actuator/prometheus
http://localhost:9000/actuator/prometheus
http://localhost:8070/actuator/prometheus
http://localhost:8071/actuator/prometheus
http://localhost:8072/actuator/prometheus


## Prometheus

### for all microservices we call following to generate docker images s11:
mvn compile jib:dockerBuild

docker image ls --filter=reference="jjasonek/*:s11"

Alternatively you can use (on Linux):
docker images | grep s10

### push images to docker hub:
docker image push docker.io/jjasonek/accounts:s11
docker image push docker.io/jjasonek/loans:s11
docker image push docker.io/jjasonek/cards:s11
docker image push docker.io/jjasonek/configserver:s11
docker image push docker.io/jjasonek/eurekaserver:s11
docker image push docker.io/jjasonek/gatewayserver:s11

### run docker compose
docker compose up -d
docker compose down

### After starting docker compose
The Prometheus console is available at http://localhost:9090
All running containers dashboard is available at http://localhost:9090/targets


## Grafana Tempo

### for all microservices we call following to generate docker images s11:
mvn compile jib:dockerBuild

docker image ls --filter=reference="jjasonek/*:s11"

Alternatively you can use (on Linux):
docker images | grep s10

### push images to docker hub:
docker image push docker.io/jjasonek/accounts:s11
docker image push docker.io/jjasonek/loans:s11
docker image push docker.io/jjasonek/cards:s11
docker image push docker.io/jjasonek/configserver:s11
docker image push docker.io/jjasonek/eurekaserver:s11
docker image push docker.io/jjasonek/gatewayserver:s11


### Note
I should have added the Tempo settings in the datasource.yaml file. But I already did it during the Prometheus setup.
This is why I saw the Tempo between connections during Prometheus testing.


### Logs examples
#### uploading the OTEL java agent class
Picked up JAVA_TOOL_OPTIONS: -javaagent:/app/libs/opentelemetry-javaagent-2.17.0.jar
OpenJDK 64-Bit Server VM warning: Sharing is only supported for boot loader classes because bootstrap classpath has been appended
[otel.javaagent 2025-07-01 21:53:52:655 +0000] [main] INFO io.opentelemetry.javaagent.tooling.VersionLogger - opentelemetry-javaagent - version: 2.17.0

#### Here, in square brackets we have the tags, trace ID and the span ID values.
2025-07-01T22:09:25.900Z  INFO [accounts,,] 1 --- [accounts] [rap-executor-%d] c.n.d.s.r.aws.ConfigClusterResolver      : Resolving eureka endpoints via configuration
2025-07-01T22:14:25.883Z  INFO [accounts,,] 1 --- [accounts] [rap-executor-%d] c.n.d.s.r.aws.ConfigClusterResolver      : Resolving eureka endpoints via configuration
Hibernate: select c1_0.customer_id,c1_0.created_at,c1_0.created_by,c1_0.email,c1_0.mobile_number,c1_0.name,c1_0.updated_at,c1_0.updated_by from customer c1_0 where c1_0.mobile_number=?
Hibernate: insert into customer (created_at,created_by,email,mobile_number,name,customer_id) values (?,?,?,?,?,default)
Hibernate: select a1_0.account_number,a1_0.account_type,a1_0.branch_address,a1_0.created_at,a1_0.created_by,a1_0.customer_id,a1_0.updated_at,a1_0.updated_by from accounts a1_0 where a1_0.account_number=?
Hibernate: insert into accounts (account_type,branch_address,created_at,created_by,customer_id,account_number) values (?,?,?,?,?,?)
2025-07-01T22:17:48.231Z DEBUG [accounts,cd35fc0d87b50a6c6c3ade2c26a59a10,25648573d2842566] 1 --- [accounts] [io-8080-exec-10] c.e.a.controller.CustomerController      : fetchCustomerDetails method start
Hibernate: select c1_0.customer_id,c1_0.created_at,c1_0.created_by,c1_0.email,c1_0.mobile_number,c1_0.name,c1_0.updated_at,c1_0.updated_by from customer c1_0 where c1_0.mobile_number=?
Hibernate: select a1_0.account_number,a1_0.account_type,a1_0.branch_address,a1_0.created_at,a1_0.created_by,a1_0.customer_id,a1_0.updated_at,a1_0.updated_by from accounts a1_0 where a1_0.customer_id=?
2025-07-01T22:17:48.890Z DEBUG [accounts,cd35fc0d87b50a6c6c3ade2c26a59a10,25648573d2842566] 1 --- [accounts] [io-8080-exec-10] c.e.a.controller.CustomerController      : fetchCustomerDetails method end
