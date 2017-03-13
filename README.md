

```bash
mkdir fake-meetings
cd fake-meetings
npm install -g express-generator
express
npm install
```
Develop your billion dollar application. :-)

To Test:
```bash
npm start 
```

Create Dockerfile.

```dockerfile
FROM node:boron

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app


# Install app dependencies
COPY package.json /usr/src/app/
RUN npm install

COPY . /usr/src/app

EXPOSE 3000
CMD [ "npm", "start" ]

```

Build docker image.
```bash
docker build -t fake-meetings .
```

Test:
```bash
 docker run -p 3000:3000 -it --rm fake-meetings

```

View http://localhost:3000 to view app inside docker container.






First login to ECR with 

```bash
 aws ecr get-login --region us-east-1

```

Push to ECR.

```bash
docker tag fake-meetings:latest 392939824843.dkr.ecr.us-east-1.amazonaws.com/fake-meetings:0.1
docker push  392939824843.dkr.ecr.us-east-1.amazonaws.com/fake-meetings:0.1
```

`Dockerrun.aws.json` file to deploy to EB.

```json
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "392939824843.dkr.ecr.us-east-1.amazonaws.com/fake-meetings:0.1",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": "3000"
    }
  ]
}
```