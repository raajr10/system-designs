What are microservices?
Cohesive focus and smaller in size/scope
data encapsulated behind a service
each service has its own independent data storage
changable and deployable
could use a different technology stack
communicate using open communication protocol like HTTP
each ms can be independently scaled up and offers reliability


challenges
how to communicate and approach workflow for data consistency?

architectural concepts

backend for frontend api
load balancers
stateless instances - doesnt remember any interaction from previous request. easy to spin up extra instances
api gateway - central contact point for microservices
synchronous communication - wait for response (challenge) - how to reduce. results in temporal coupling/waiting
caching
asychronous communication - message broker using events/messages in fire and forget style
combination of both via message broker
eventual consistent
service can be API based service or worker based service
authentication & authorisation

monolith
single app = frontend + business + data model layer
highly coupled
could lead to spagetti code
trying to add new functionality includes a danger to take the whole monoltih app to go down
fixed technology
single database
targetted scaling out not possible & results to increase in the cost

could replicate environment as there are fewer moving parts involved
could be useful for smaller team and fewer resources
can use microservices ready monolith

still be careful for a successful microservices architecture

microservices success
quick to deploy - shorter development and deployment time
more reliable - if one thing breaks, the other parts of the software shouldnt be affected
resilient
modular approach - agile to structure of organisation, specific teams can own the specific microservices

6 Core Design principles for Microservices architecture
1. autonomous - each service is independently changeable and deployable
2. domain driven cohesion - each service represents a specific part of the business with a cohesvie focus
3. ownership culture - each service is a product with a team behind it (long term rotate team members to spread overall knowledge)
4. resiliency - each service fails fast and alternates functionality.
5. observability - workslfows and component health is visible and traceable
6. automation - tools for automated hosting, testing and deployment


autonomous: loose coupling / independent
should be physically separate and communicate via network - http
make sure they have their own data store/database
http calls between the services - req/response should be consistent to avoid changes
expose minimal data
backward compatibilty - enable version and communicate the changes incorporated
3 digit semantic versioning strategy - major.minor.patch
end points can coexist
make them stateless - rather requests containts data
multiple apps could be part of one conceptual microservice
use shared cache for performance


domain driven cohesion - identify core business domains
performance, frequency of change, risk, having a subdomain - reasons for splitting microservices
dont be afraid to split microserivces AND dont be afraid to merge microservices if it makes data more accessible and convenient
Identify Bounded Context- Domain and Tech team identify core concepts and the related concepts (specific pieces of data/functionality) of the product to create a CONTEXT which acts as a starting point of microservices
concepts that fall out of the context are considered as core concept in itself around which the bounded context is created
this creates blueprint for the microservices
event storming
microservices should be easily rewritable


ownership culture - treat each ms as a product
no duplication of data and functionality
advocate for the product
assign a team to a microservice responsible for long term maintenance and collaborative development
API catalog / Product Catalog - advertise service data & functionality, act as reference point, communicate new use cases
ubquitous language creation - correct and related terminologies
ownership around design, architectural standards and guidance via central team
some things like security, monitoring, logging will be centralised as well


resiliency - software reliable and available. ensure if there are any failures, each service fails fast and there is an alternate functionality to avoid any disruption
design to expect failure of downstream apps/service and handle them elegantly
assume that network / connectivty between these components is prone to failure - issues like latency
design for 3rd party apis are unreliable and can have issues. use cache (caching strategy) to fetch old available version rather than trying to connect back to unreliable api if in case failure occurs.
other resiliency pattern are :
circuit breaker pattern - aaas and cbp. keep count of failures and retries - use cache till that time
retry pattern
timeout pattern
all these should be used in combination
having active backups of the software components (having more instances running)
maintain network health
validate connections and data - error handling 400 bad request (dead letter queue could also be used)


observability
central logging - dont use local logs
requests, responses, messages, timeouts, errors, issues, logging in logging out, start ups and shut downs- gather all data we are going to need
logging format should be consistent
log level is important
correlation id should be associated to a transaction to pin point the exact issue
in case of error include as much as information possible - call stacks, microservice, ms instance
have all this for the whole stack - from network to application
have a healthcheck endpoint
have central monitoring
have a performance stats - cpu, memory, disk usage, network, latency and connectivity issues
ability to set real time alerts to proactively react


automation
hosting, testing and deployment
recreate any kind of environments easily - this can be achieved via IaC, Cloud platforms and containers
also helps in scaling out to manage performance requirements
deployment pipeline - commiting to source control triggers the build with config
helps for quick feedback.it also helps in automated testing - unit and integration tests.helps resolve issues quickly and immediately.
config - environment, versions of microservices (releases)
helps automatic roll back
enables quality gate - helps in promotion from environment to environment
automate any manual toil


technologies
1. load balancer & service registery: support multiple stateless microservices instances and shares the incoming traffic using an algorithm. from client perspective, it is unaware.they should be stateless. health probes - improves reliability of the application, calls healthcheck endpoint it can be custom - app and the downstream app dependency too
SR can be built in Lb or have a seperate system
db which lists all ms and their instances ie ip address/locations
helps ms to register on startup and deregister on shut down

2. api gateway and bff api: provides one interface for the data and functionality available
in the backend
loads of other task - like security, auth/authentication, call rate limiting, monitoring, caching and logging capabilities
Lb and SR functionalities can be built in part of it
BFF (type of an API gateway) is a custom microservice which can provide functionalities of API gateway.
provides ability to map incoming requests to the ms, format data and send the response

3. sync comm: simple http calls which consists of request/response
introduces temporal coupling leading to delays when lots of calls are made
avoid physical distributed transaction
all the ms involved should be available

4. api style for ms: RESTful microservices
use blueprint created around bounded context
we can expose concepts as an endpoint and combine them with http
http verbs - get, put, post, delete
use json and xml for sending and receiving data
ms becomes intuitive
data and fucntionality exposed using universal language
enable version for backwards compatibility
Rest supports stateless services, other principles are also in line with ms arch
also supports caching strategies

5. resiliency pattern:
timeout pattern
circuit breaker pattern: number of failures acceptable before we stop connection again. fail fast and provide alternate solution
retry pattern
all these patterns are available in the http call making libraries
caching strategies: use central cache

6. openapi and api catalogs: industry standard
documents to create api catalog
combined with http can help us expose all contracts and interfaces
expose a special endpoint - swagger (normally in json): endpoint, http verbs, request &response format
all these machine readable json documents can be used to create an API catalog

7. async comm: fire and forget mechanism into the message broker
suggestion - use sync comm for FE to BE communication, comm between ms can be async with message broker/service bus/message bus/event bus
terminologies:
sender/client/publisher
receiver/consumer/subscriber
message/command/event
multiple queues can be present with diff algorithms
dead letter queue to send messages which cant be processed at all
event driven architecture & eventual consistency

8. Transaction Manager: manage state of logical transaction using async comm
enabled undoing of partial transactions
saga pattern

9. Eventual consistency & event driven architecture:
all updates are pushed to the queues and events are triggered

10. deploy using containers
more efficient than VM, smaller footprint in terms of usage, fast startup
base images, preconfigured images can be stored in an image repository
orchestration engine - k8s
load balancing, service registry, routing traffic
also supports IaC
defacto standards for deploying microservices

11. cloud platform: iaas, paas, caas, faas

12. security:
use https with ssl and tls
minimise what is exposed on the internet using private/virtual network
api gateway can have extra security measures - call rate limitation for prevention of ddos
auth and authentication - central security component
standards/protocols like oauth2.0, oidc, token exchange, etc
all ms should validate the access token
no need to reinvent the wheel - use tools/service provider

13. central logging and monitoring: use this for troubleshooting
auto scaling options should be availabale in terms of performance & data storage
logs archiving and truncate - for compliance and regulations
use https to connect to the central logging system
search and query functionality
use logging library for consistent logs - structured & standarized across the microservices
use ligh weight format like json
ex - Elastic search, splunk, graphite
real time data of the servers
helps to identify problems brewing in our microservices
visualisation, aggregation of data
schedule alerts via various methods - email, visual alerts, custom alss, sms
need to use monitoring client software to send it to central monitoring

14. automation tools and patterns:
source control systems:
choose branching strategies
bitbucket/github

CI tools:
automatic builds, running layer of tests
Jenkins

CD tools:
store env configurations
release configurations - parameters associated to the release version
validation gates - ex: do not deploy unless this version is not present in the lower env
Spinnaker

*** very important ***
Greenfield situation: new potential application that we want to power with microservices based backend
start off with traditional architecture consisting of FE and BE in modular monolith
code is divided into modules, separated from each other physically even at database level
follow database per service
modules represent specific business context
use proven separation pattern - service/facade pattern
no accidental coupling - each module can be created as a ms
avoid big bang approach, start small with having:
have one single risk free microservice & build knowledge, experience and confidence with the other key components - security, logging, monitoring, using LB, Api gateway, message broker if any, caching, automated cicd pipeline
when ready to grow? event storming is one of the technique to create new microservices

