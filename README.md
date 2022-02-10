# Spring Microservices in Action: 2nd Ed. - Notes


### 12 Factor App:
1. One codebase tracked in revision control, many deploys/instances (code & app is 1:1). OneHome is a distributed system, each component is an app. 
2. Explicitly declare and isolate dependencies (don't share between apps)
3. Store config in the environment - Config varies substantially across deploys, code does not. Store as env vars, not config files!!!
4. Backing Services - can be changed via configuratin alone. No code changes nessessary
5. Build, Release, Run - releases are append only ledgers; they are immutable. Builds happen on code merge. Release can be optional. 
6. Processes - processes never store state (e.g. sticky sessions). 
7. Port Binding - does not rely on runtime injection of a webserver. The web app exports HTTP as a service by binding to a port.
8. Concurrency - Scale out ... ? 
9. Disposability - they can be started or stopped at a moment’s notice. This facilitates elastic scaling
10. Dev/Prod Parity - problem: time gap, personnel gap, tools gap (I will add the data gap)
11. Logs - Treat logs as event streams. Routing of logs is external to the app, managed by the environment (discrepancy)
12. Admin Processes - Run admin/management tasks as one-off processes. They run against a release, using the same codebase and config as any process run against that release.

### Decomposing Monoliths:
#### Right-Sizing Microservices: 
- It's better to start with broad, course-grained microservices and refactor to smaller 
- Focus first on how the microservices will interact with each other (embrace REST, use URIs to indicate intent, JSON for request/response, and HTTP status codes to communicate results)
- Service responsibilities change over time as our understanding of the problem domain grows <br>


#### Smells
**Too coarse**
- A service with too many responsibilities
- A service that manages data across a large number of tables
- A service with too many test cases (may be doing too much)

**Too fine grained**
- The microservices in one part of the problem domain breed like rabbits
- Your microservices are heavily interdependent on one another
- Your microservices become a collection of simple CRUD (Create, Replace, Update, Delete) services

A microservices architecture should be developed with an evolutionary thought process, where you know that you aren’t going to get the design right the first time. That is why it’s better to start with your first set of services being more coarse-grained than fine-grained.
Microservice architectures are 'emergent'. 



Principles: 
	Application logic is broken down into small-grained components with well defined boundaries of responsibility that coordinate to deliver a solution
	Each component has a small domain of responsibility and is deployed completely independently of one another.
	The underlying technical implementation of the service is irrelevant because the applications always communicate with a technology-neutral protocol
	Microservices allow organizations to have small development teams with well-defined areas of responsibility

	Microservices are the gateway drug for building cloud applications.

Benefits: 
	Flexible - decoupled services can be composed and rearranged to quickly deliver new functionality
	Resilient - no longer a single “ball of mud” where a degradation in one part of the application causes the whole application to fail. Graceful degredation
	Scalable - easier to scale apps horizontally 

	In short: Small, Simple, and Decoupled Services = Scalable, Resilient, and Flexible Applications

Clouds
	One Prem
	IaaS
	PaaS
	SaaS
	CaaS
	FaaS

Core microservice development pattern: 
	Service Granularity
	Communication protocols (usually JSON)
	Interface Design 
	Configuration Management (Spring Cloud Config (Git, Consul, Eureka))

Microservice Routing Patterns
	Service Discovery (Spring Cloud Discovery/Netflix Eureka)
	Service Routing (Spring Cloud API Gateway)

Microservice Client Resiliency Patterns 
	Client-side load balancing (Spring Cloud Load Balancer)
	Circuit breakers (Resilience4j)
	Bulk Head (Resilience4j)
	Fallback (Resilience4j)

Microservice Security patterns
	Authentication
	Authorization
	Credential management and propagation
	Spring: Spring Cloud Security 

Microservice Logging and tracing patterns
	Log correlation (Spring Cloud Sleuth)
	Log aggregation (Spring Cloud Sleuth (ELK Stack))
	Tracing (Spring Cloud Sleuth/Zipkin)

Microservice Build Deploy Patterns
	Problem: configuration drift (“I made only one small change on the stage server, but I forgot to make the change in production.”)
	Build and deployment pipeline
	Infrastructure as code
	Immutable servers/Phoenix servers
	Benefits: If we have infrastructure as code, we can build entire ephimeral environments for testing
	Spring: NA, need to rely on external tool such as Jenkins, AWS CodeBuild/Deploy, etc

Microservice Metrics pattern
	Metrics 
	Metrics service - where you can store and query the application metrics
	Metrics visualization suite - where you can visualize business-related time data for the application and infrastructure


Pg 55 for deployment lifecycle diagram 



## Spring Cloud Configs
Link: https://cloud.spring.io/spring-cloud-config/reference/html/#_quick_start
When adding applicaiton configs, can do in the following way:
	- <application-name>.properties <--- default
	- <application-name>-non-prod.properties
	- <application-name>-prod.properties
When querying the REST api for any specific profile, that profile and the "default" configs are returned. This is because Config clients will fill 
in any non-existing profile properties with the defaults. This behavior belongs to the clients, not the API. 

Can read from :
1. Filesystem/classpath
2. VCS backed configs (e.g. Git)
3. Hashicorp Vault

To run in filesystem or classpath mode, run with the 'native' profile and following configs:
```yaml

  cloud:
    config:
      server:
        native:
#          search-locations: file:///Users/some-dir/some-file # file system
          search-locations: classpath:/config                # classpath

```
