# First lambda function with containers

Just testing lambda functions as container image. A quickstart.

1. Build the image
```bash
docker build -t hello-world .
```
2. [Optional] Run the dockerized image
```bash
docker run -p 9000:8080 hello-world:latest
```
3. Test the function locally
```bash
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}' .
```
4. Authenticate the docker cli to your Amazon ECR registry
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACCOUNT_NO}.dkr.ecr.${REGION}.amazonaws.com  
```
5. Create a repository in ECR
```bash
aws ecr create-repository --repository-name hello-world --image-scanning-configuration scanOnPush=true --image-tag-mutability MUTABLE
```

6. Tag image to match the one in ECR
```bash
docker tag  hello-world:latest ${ACCOUNT_NO}.dkr.ecr.${REGION}.amazonaws.com/hello-world:latest
```

7. Deploy
```bash
docker push ${ACCOUNT_NO}.dkr.ecr.${REGION}.amazonaws.com/hello-world:latest
```