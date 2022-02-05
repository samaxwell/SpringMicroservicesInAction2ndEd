Microservices


12 Factors:
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
	Configuration Management (Spring Cloud Config)

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

