# configserver
configserver

It is always recommended to externalize and centralize microservice configuration parameters using Spring Cloud Config server.
The Spring Config server stores properties in a version-controlled repository such as Git or SVN.
Unlike Spring Boot, Spring Cloud uses a bootstrap context, which is a parent context of the main application. Bootstrap context is responsible for loading configuration properties from the Config server. The bootstrap context looks for bootstrap.properties or bootstrap.yml for loading initial configuration properties. Hence rename application.properties as bootstrap.properties in configserver

1) Refresh all micro services properties by POST /actuator/bus-refresh
   ===================================================================
curl localhost:8081/actuator/refresh -d {} -H "Content-Type: application/json"
curl localhost:8081/actuator/bus-refresh -d {} -H "Content-Type: application/json"