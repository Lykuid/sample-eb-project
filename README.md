

First login to ECR with 

```bash
 aws ecr get-login --region us-east-1

```

Push to ECR.

```bash
docker build -t sample-eb-project .
docker tag sample-eb-project:latest 392939824843.dkr.ecr.us-east-1.amazonaws.com/sample-eb-project:0.1
docker push  392939824843.dkr.ecr.us-east-1.amazonaws.com/sample-eb-project:0.1
```

Dockerrun file to deploy to EB.

```json
{
  "AWSEBDockerrunVersion": "1",
  "Image": {
    "Name": "392939824843.dkr.ecr.us-east-1.amazonaws.com/sample-eb-project:0.1",
    "Update": "true"
  },
  "Ports": [
    {
      "ContainerPort": "8080"
    }
  ]
}
```