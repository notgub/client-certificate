# Client Certificate Project

A proof of concept (POC) project for implementing client certificate authentication with a web service deployed on AWS ECS.

## Project Overview

This project demonstrates a complete client certificate authentication setup with:
- **Web Service**: Next.js application deployed on AWS ECS Fargate
- **Load Balancer**: Application Load Balancer (ALB) for traffic distribution
- **DNS**: Route 53 hosted zone with A record for domain management
- **Infrastructure**: CloudFormation templates for automated deployment

## Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Route 53      │    │   Application    │    │   ECS Fargate   │
│   DNS (A Record)│───▶│   Load Balancer  │───▶│   Service       │
│   notgub.com    │    │   (Port 80)      │    │   (Port 3000)   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
```

## Project Structure

```
client-certificate/
├── aws-resource/                 # AWS infrastructure
│   ├── templates/
│   │   └── ecs-services.yaml    # CloudFormation template
│   ├── deploy.sh                # Deployment script
│   └── README.md               # AWS deployment guide
├── web-service/                 # Next.js web application
│   ├── src/
│   ├── package.json
│   └── README.md
├── mobile-app/                  # Mobile application
│   └── README.md
└── README.md                   # This file
```

## Features

### AWS Infrastructure
- **ECS Cluster**: Fargate-based container orchestration
- **ECS Service**: Scalable service with auto-scaling capabilities
- **Application Load Balancer**: HTTP/HTTPS traffic distribution
- **Route 53**: DNS management with A record for `notgub.com`
- **CloudWatch**: Logging and monitoring
- **IAM Roles**: Secure execution and task roles

### Web Service
- **Next.js**: React-based web framework
- **Container**: Docker containerization
- **Port**: Runs on port 3000
- **Environment**: Production-ready configuration

### DNS Configuration
- **Domain**: `notgub.com`
- **Record Type**: A record with ALB alias
- **Hosted Zone**: Uses existing Route 53 hosted zone

## Quick Start

### Prerequisites
1. **AWS CLI** configured with appropriate permissions
2. **Docker** for container management
3. **Node.js** for local development

### Deployment

1. **Deploy AWS Infrastructure**:
   ```bash
   cd aws-resource
   ./deploy.sh
   ```

2. **Build and Deploy Web Service**:
   ```bash
   cd web-service
   npm install
   npm run build
   # Deploy to ECR and update ECS service
   ```

3. **Access the Application**:
   - URL: `http://notgub.com`
   - The A record will point to your Application Load Balancer

## Configuration

### Environment Variables
- `NODE_ENV`: Set to "production" in ECS
- Container port: 3000
- Load balancer port: 80

### AWS Resources
- **Region**: ap-southeast-7 (default)
- **VPC**: Uses existing VPC with specified subnets
- **Security Groups**: Configurable via parameters
- **CPU/Memory**: Configurable task resources

## Development

### Local Development
```bash
cd web-service
npm install
npm run dev
```

### Testing
```bash
# Run tests
npm test

# Build for production
npm run build
```

## Monitoring

- **CloudWatch Logs**: Application logs in `/ecs/web-service`
- **ECS Metrics**: CPU, memory, and network metrics
- **ALB Metrics**: Request count, response time, error rates

## Security

- **IAM Roles**: Least privilege access
- **Security Groups**: Network-level security
- **Container Security**: Non-root user execution
- **HTTPS Ready**: ALB supports SSL termination

## Troubleshooting

### Common Issues
1. **DNS not resolving**: Check Route 53 hosted zone configuration
2. **Service not starting**: Verify ECS task definition and IAM roles
3. **Load balancer health checks failing**: Check container port and health check path

### Useful Commands
```bash
# Check ECS service status
aws ecs describe-services --cluster web-service-cluster --services my-family-task-service-ey5v6cyh

# View CloudWatch logs
aws logs tail /ecs/web-service --follow

# Check ALB target health
aws elbv2 describe-target-health --target-group-arn <target-group-arn>
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## License

This project is for POC purposes. Please ensure compliance with your organization's policies.

## Support

For issues and questions:
1. Check the troubleshooting section
2. Review AWS CloudFormation documentation
3. Check ECS service logs in CloudWatch
