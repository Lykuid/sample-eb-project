# Problem

At Lykuid we needed a mechanism to ingest customer data. It had to provide high availability and complete isolation, 
so customers are not impacted by possible downtime, service upgrades, or bugs introduced from other components. 
For this we need an isolated service which is simple and robust. 

We also needed predictable response times and minimal resource constraints. The platform needed to support 
high concurrency without requiring a large thread or worker pool. In order to do this we needed an application where all IO is asynchronous.

# Solution

We chose Node.js because it provides concurrency without having to manage resource pools. With Node.js we were able to implement our logic in a performant high level language without the concern of being blocked by any outside services. 

Elastic Beanstalk is Amazon managed and provides monitoring, auto provisioning and reduces our maintenance by providing upgrades and auto expanding and shrinking. EB also provides log management, archival and metric collection. An Amazon provided Docker platform is also provided. This allowed us to run our application in a containerized environment. 

# Why Docker with Elastic Beanstalk?

Traditional Elastic Beanstalk deployments use Amazon Linux running Node which runs your application. This ties you to using Amazon’s Node.js version and configuration. By using Docker we are able to customize the Node.js environment and package it with our dependencies. This provides greater control over our application and doesn’t tie us to the constraints of traditional Elastic Beanstalk environments. Doing it this way also gives the flexibility to use any of the published Docker base images on Docker hub or other registries. 

For this use case, we selected Amazon’s Elastic Beanstalk with the Docker Platform and Elastic Container Registry (ECR).
 Elastic Beanstalk provides us with a cluster of ingestion nodes spread across multiple availability zones with a managed 
 platform capable of running standard docker images.
 
 Elastic Beanstalk provides us with deployment automation, health monitoring, log and metric collection and auto scaling. 

![Elastic Beanstalk / Docker Architecture](https://lykuid.github.io/sample-eb-project/lykuideb.svg)


We begin by placing a `Dockerfile` for a node project.

A developer writes a `Dockerfile` which describes how to package his application into a Docker image. This allows him to build the image using the `docker build` command. He then can tag the image with `docker tag` and push to ECR using `docker push`. This image is now housed on Amazon's infrastructure and is ready to be deployed using Elastic Beanstalk. With the image on ECR, the developer is able to launch a docker Elastic Beanstalk environment and deploy his application by providing a `Dockerrun.aws.json` file. 



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
