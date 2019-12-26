# configserver
configserver

* It is always recommended to externalize and centralize microservice configuration parameters using Spring Cloud Config server.
* The Spring Config server stores properties in a version-controlled repository such as Git or SVN.
* Unlike Spring Boot, Spring Cloud uses a bootstrap context, which is a parent context of the main application. Bootstrap context is responsible for loading configuration properties from the Config server. The bootstrap context looks for bootstrap.properties or bootstrap.yml for loading initial configuration properties. Hence rename application.properties as bootstrap.properties in configserver

1) Refreshing  micro services properties by POST /actuator/bus-refresh
   ====================================================================
   
   If any property is changed, the related service need to be notified by triggering a refresh event with Spring Boot Actuator (/actuator/refresh). The user will have to manually trigger this refresh event. Once the event is triggered, all the beans annotated with @RefreshScope will be reloaded (the configurations will be re-fetched) from the Config Server.

curl localhost:8081/actuator/refresh -d {} -H "Content-Type: application/json"

In a real microservice environment, there will be a large number of independent application services. Therefore is it not practical for the user to manually trigger the refresh event for all the related services whenever a property is changed.

The better approach is to trigger the refresh event for one service and broadcast the event through all other available services.  This sounds good !  here, we are going to explore a way to trigger the refresh event for only one service and that event is automatically propagated (broadcasted) through all the other services. This can be achieved with Spring Cloud Bus.

Spring Cloud Bus links the independent services in the microservices environment through a light weight message broker (e.g:- RabbitMQ or Kafka).  This message broker can be used to broadcast the configuration changes and events. In addition, it can be used as a communication channel among independent services.

A key idea is that the Bus is like a distributed Actuator for a Spring Boot application that is scaled out, but it can also be used as a communication channel between applications.

curl localhost:8081/actuator/bus-refresh -d {} -H "Content-Type: application/json"