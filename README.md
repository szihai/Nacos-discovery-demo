# Nacos-discovery-demo
## Introduction
Nacos is a config server and service registry. It can be deployed alone as 3rd party service provider or being integrated into other frameworks like Spring Cloud.

For service discovery, Nacos supports both DNS-based service discovery and RPC-based (Dubbo/gRPC) service discovery. 

Nacos provides rich features in service discovery. It has real-time health check to prevent services from sending requests to unhealthy hosts or service instances. It supports both transport layer (ping or tcp) health check and application layer (such as http, redis, MySQL, and user-define) health check. For the health check of complex clouds and network topologies(such as VPC, Edge Service etc), Nacos provides both agent mode and server mode. Also, it provide a unity service health dashboard to help you manage the availability and traffic of services.

In this demo, we'll showcase how to use Nacos in Spring Cloud.

## The demo code
### Provider
It is pretty straightforward for the service provider. It uses annotation `@SpringBootApplication` and 
`@EnableDiscoveryClient` to invoke Nacos scanning. This is the same for other libraries such as Eureka. For the demo we create a REST api via path `"/echo/{string}"`. All it does is echoing the input. 

### Consumer
The consumer will consume the service. But it will also expose APIs for end users to access it(via browser or terminal). To spice things up, in the code we provide two approaches to expose the APIs: One is Feign client, the other is REST client.

Both Feign and RestTemplate are ways to expose REST apis in spring boot.

The feign client uses @FeignClient(name = "service-provider") annotation with interface defination that matches the echo service to serve the end user. The REST api is exposed as `/echo-feign/`.
 
The REST client uses @LoadBalanced and @Bean annotation to specify a load balancer client. In our code, the restTemplate is using the load balancer. And the restTemplate is going to invoke the serice at `"http://service-provider/echo/"`.


## Run the demo

We'll use Maven to build both projects.

## Deploy in on Alibaba Cloud
### EDAS system
