
* Why Elastic Beanstalk?
   * AWS Managed Updates
   * Load Balancing
   * Autoscaling
   * Log Archival
* Why Docker?
   * Platform standardization
   * Flexible platform deployment






Elastic Beanstalk
WHY 
* Managed updates
* Logs
* Auto Scaling
* Health Monitoring 
   * Agregat 
      * Latency 
      * Error rate
      * Request rate
   * Per instance
      * Latency
      * Error rate
      * Request rate
      * Load avg
      * CPU
* Cloudwatch
   * Same as health monitoring 
   * Network in/out
* Alarms




# Problem

At Lykuid we needed a mechanism to ingest customer data. It had to provide high availability and complete isolation, 
so customers are not impacted by possible downtime, service upgrades, or bugs introduced from other components. 
For this we need an isolated service which is simple and robust. 

We also needed predictable response times and minimal resource constraints. The platform needed to support 
high concurrency without requiring a large thread or worker pool. In order to do this we needed an application where all IO is asynchronous.

# Solution


For this use case, we selected Amazon’s Elastic Beanstalk with the Docker Platform and Elastic Container Registry.
 Elastic Beanstalk provides us with a cluster of ingestion nodes spread across availability zones with a managed 
 platform capable of running standard docker images.




# Application

For the application, we chose to develop in node. Node gives us an environment capable of high concurrency with predictable behavior. It allows us to eliminate concern of thread and worker pools found in other languages.

We used the node base docker image.  This allows us to select our platform from standard docker registries independent of the platforms Elastic Beanstalk provides.

We begin by placing a `Dockerfile` for a node project.

```docker
FROM node:boron

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app


# Install app dependencies
COPY package.json /usr/src/app/
RUN npm install

COPY . /usr/src/app

EXPOSE 8080
CMD [ "npm", "start" ]
```
