# web-service

### Build and run container

Build & Test Docker Locally
```
docker build -t nextjs-app .
docker run -p 3000:3000 nextjs-app
```

Push to Amazon ECR (Elastic Container Registry)
```
# Authenticate to ECR
aws ecr get-login-password --region ap-southeast-7 | docker login --username AWS --password-stdin 235494814246.dkr.ecr.ap-southeast-7.amazonaws.com

# Create ECR repo (once)
aws ecr create-repository --repository-name nextjs-app

# Tag and push
docker tag nextjs-app 235494814246.dkr.ecr.ap-southeast-7.amazonaws.com/nextjs-app
docker push 235494814246.dkr.ecr.ap-southeast-7.amazonaws.com/nextjs-app
```