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
