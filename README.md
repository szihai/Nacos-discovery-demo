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
To clarify, doing this, end users can already access this service via browser or curl. But the purpose of the demo is to show consumer applications accessing this service. And other service types like gRPC services will be more complicated to create.

### Consumer
The consumer will consume the service. But it will also expose APIs for end users to access it(via browser or terminal). To spice things up, in the code we provide two approaches to expose the APIs: One is Feign client, the other is REST client.

Both Feign and RestTemplate are ways to expose REST apis in spring boot.

The feign client uses @FeignClient(name = "service-provider") annotation with interface defination that matches the echo service to serve the end user. The REST api is exposed as `/echo-feign/`.
 
The REST client uses @LoadBalanced and @Bean annotation to specify a load balancer client. In our code, the restTemplate is using the load balancer. And the restTemplate is going to invoke the serice at `"http://service-provider/echo/"`.


## Run the demo
### Run Nacos server locally
Before we can try out the demo, we need to install and run a local Nacos server. Here are the simple steps to do so:
```
git clone https://github.com/alibaba/nacos.git
cd nacos/
mvn -Prelease-nacos clean install -U  
ls -al distribution/target/

// change the $version to your actual path
cd distribution/target/nacos-server-$version/nacos/bin

sh startup.sh -m standalone
```
Replace the `$version` with the real version number.

### Build and run

We'll use Maven to build both projects. Use either terminal or IDE to do so. After that, run the provider first so it can register.
And then we run the consumer. 
To try it out, in a browser or using curl:
`http://localhost:18082/echo-feign/feign`
This will echo the string `feign` via the feign client.
And `http://localhost:18082/echo-rest/rest`
This will echo the string `rest` via the feign client.

## Deploy in on Alibaba Cloud
### EDAS system
