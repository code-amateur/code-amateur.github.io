# Netflix Eureka discovery server

## Runnung a Eureka Server

### In standalone mode

Add below dependency
     
        <dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		</dependency>


Enable the Discovery Server by putting `@EnableEurekaServer`

Disable reistring the server with another Eureka Server by setting below flags.


    eureka.client.register-with-eureka=false
    eureka.client.fetch-registry=false

### In High Availability mode

You can run multiple instance of Eureka Servers in HA mode (Peer Awareness). This will allow eureka servers to reigister with each other.

Bellow are the configurations for running 2 instances of Eureka server in a developer machine.

Instance 1


    spring.application.name=discovery-server
    server.port=8761

    eureka.instance.hostname=localhost
    eureka.instance.preferIpAddress=true

    eureka.client.register-with-eureka=true
    eureka.client.fetch-registry=true
    eureka.client.serviceUrl.defaultZone=http://localhost:8762/eureka/

Instance 2


    spring.application.name=discovery-server
    server.port=8762

    eureka.instance.hostname=localhost
    eureka.instance.preferIpAddress=true

    eureka.client.register-with-eureka=true
    eureka.client.fetch-registry=true
    eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka/

More information on Eureka Peer Awareness is here : http://cloud.spring.io/spring-cloud-static/spring-cloud.html#_peer_awareness