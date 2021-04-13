# Micro-services research

- Monolithic vs. service-oriented architecture (SOA)
	- **Micro-services architecture**: an opinionated form of SOA. [by Sam Newman]
	- **Micro-services architecture**: traditional SOA + containerizational technologies, APIs + DevOps. [by Redhat]
	- SOA + Enterprise Service Bus (ESB)? new single point of failure!

![Monolithic vs. Microservices approaches](https://www.redhat.com/cms/managed-files/monolithic-vs-microservices.png)

## MSA Principles: [[by Sam Newman](https://www.youtube.com/watch?v=PFQnNFe27kU&ab_channel=Devoxx)]

- **Modelled around business domain**: vertical rather than horizontal.
- **Culture of automation**:
	- Infrastructure automation
	- Automated testing
	- Continuous delivering ![Automation is critical at scale](https://i.ibb.co/gPkVjsk/Screen-Shot-1400-01-08-at-15-13-14.png)

- **Hide implementation details**:
	- Hide the database
	- Have models within services boundaries and models out of boundaries
- **Decentralise all the things**:
	- Autonomy as self-service
	- Autonomy as shared governance
	- Autonomy as Dumb-pipes, smart endpoints

- **Deploy independently**: (the golden rule!)
	- One service per host (host as an environment and set of resources)
	- Consumer-driven deployment (considering the consumer's expectations, e.g. "Pact")
	- Co-exist endpoints (If v1 is already used, use the v2, after weeks remove the v1) ![One service per host](https://i.ibb.co/m567jVZ/Screen-Shot-1400-01-10-at-10-07-11.png)

- **Consumer first**:
	- Documentation (e.g. "Swagger")
	- Service discovery (e.g. "etcd" or "Consul"); A matter of scale. What is running where? Humane Registeries.

- **Isolate failure**:
	- *Strangler application pattern* (Strangling the monolith)
		1. Fix timeouts (e.g. 2mins -> 2secs)
		2. Separate thread pools (*Bulkhead pattern*)
		3. Circuit breakers (near-zero downtime deployment) ![Failure isolation steps](https://i.ibb.co/WfQ6xft/Screen-Shot-1400-01-10-at-11-43-40.png)

- **Highly observable**:
	- Aggregate logs (e.g. "Kibana")
	- Aggregate stats (timeseries-based systems)
	- Correlation IDs (coming from live production logs) ![Correlation IDs](https://i.ibb.co/646S8Y7/Screen-Shot-1400-01-10-at-11-48-30.png)

## MSA challenges: [by Redhat]
1. **Building:** identifying the interservices dependencies 
2. **Testing:** integration testing + end-to+end testing
3. **Versioning:** maintaining the backward compatibility
4. **Deployment:** automation
5. **Logging:** centralized logs
6. **Monitoring:** centralized view of the system
7. **Debugging:** dozens or hundreds of services?! how?!

## Service Mesh: [by RedHat]
- optimize the requests routing between services, as a layer of infrastructure,
- is an alternative for coding the logic governing communications into each service.
- "sidecar" proxies ---> Mesh network !["sidecar" proxies ---> Mesh network](https://www.redhat.com/cms/managed-files/service-mesh-1680.png)
- provide performance metrics
- watch and optimize requests
- reroute requests away from failed services
- Distributed Tracing (e.g. "Jaeger")

### Stateless vs. Stateful applications [by RedHat]
A **stateless** process or application can be understood in isolation. There is no stored knowledge of or reference to past transactions. Each transaction is made as if from scratch for the first time. Stateless applications provide one service or function and use content delivery network (CDN), web, or print servers to process these short-term requests.

**Stateful** applications and processes, however, are those that can be returned to again and again. They’re performed with the context of previous transactions and the current transaction may be affected by what happened during previous transactions. If a stateful transaction is interrupted, the context and history have been stored so you can more or less pick up where you left off.
### Containers and state [by RedHat]
As cloud computing and microservices grow in popularity, so too has containerization of applications, whether stateful or stateless. Containers are units of code for an application that are packaged up, together with their libraries and dependencies, so that they’re able to be moved easily.

With the growth in popularity of containers, companies began to provide ways to manage both stateless and stateful containers using data storage, Kubernetes, and StatefulSets. Statefulness is now a major part of container storage and the question has become not if to use stateful containers, but when. 


### [Netflix MSs](https://www.youtube.com/watch?v=57UK46qfBLY&ab_channel=GOTOConferences) assumptions:
1. Buy technologies > use or contribute to OSS techs > technologies > build [only] what you have to.
2. Services should be stateless: not rely on sticky sessions, prove by chaos testing.
3. Scale out (horizontal) > Scale up (hits a limit)
4. Redundancy and isolation for resiliency: Make more than one of anything, isolate the blast radius for any given failure.
5. Automate destructive testing (Simian Army, Chaos Monkey) 

### Netflix MSs lessons:
- IPC (inter-process communication) is crucial for loose coupling:
  - Common language between the services
  - Establishes the contract of interaction (e.g. "Asgard" or "Spinnaker")
- Caching to protect DBs (?!)
- Operational visibility matters: Automated error-detection algorithms
- Reliability matters [*in progress*]
- ![End to end ownership](https://i.ibb.co/F3dkGgV/Screen-Shot-1400-01-10-at-16-28-14.png)
- ![End to end ownership with velocity](https://i.ibb.co/gybw2n6/Screen-Shot-1400-01-10-at-16-29-02.png)


### Soundcloud's MSs: [BFF pattern in action](https://www.youtube.com/watch?v=jfN6HOgURXM&ab_channel=microXchg)
BFF: Backend For Frontend (Client Applications -> BFFs -> MicroServices)
1. ~~Public API~~ -> Client specific API
2. Humane Registry: Critial path and MicroServices
![BFF proxy to Critical path AND MSs](https://i.ibb.co/X7hSKNg/Screen-Shot-1400-01-11-at-14-24-51.png)

3. Add other MicroServices between BFFs and MSs, to reduce inter-services communications
![](https://i.ibb.co/Nt1W8nw/Screen-Shot-1400-01-11-at-14-22-04.png)

4.
	- Foundations Layer: CRUD operations, services on this layer communicate only with data sources.
	- Value Added Layer
	- BFF
![Foundation layer, Value added layer, BFF](https://i.ibb.co/71CDm5N/Screen-Shot-1400-01-11-at-14-21-27.png)
