<!--
 Copyright 2023 lesongvi
 
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
 
     http://www.apache.org/licenses/LICENSE-2.0
 
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->

# Table of contents
- [Docker](#docker)
	- [Where Docker images are stored?](#where-docker-images-are-stored)
	- [Docker vs Virtual Machines](#docker-vs-virtual-machines)
- [ECS (Elastic Container Service)](#ecs-elastic-container-service)
- [Fargate](#fargate)
- [ECR (Elastic Container Registry)](#ecr-elastic-container-registry)
- [Serverless](#serverless)
	- [Lambda](#lambda)
		- [Why Lambda?](#why-lambda)
		- [Benefits of AWS Lambda](#benefits-of-aws-lambda)
		- [AWS Lambda language support](#aws-lambda-language-support)
		- [Example: Serverless Thumbnail creation](#example-serverless-thumbnail-creation)
		- [Example: Serverless CRON Job](#example-serverless-cron-job)
		- [AWS Lambda Pricing: example](#aws-lambda-pricing-example)
		- [Amazon API Gateway](#amazon-api-gateway)
	- [AWS Batch](#aws-batch)
		- [AWS Batch example](#aws-batch-example)
	- [Batch vs Lambda](#batch-vs-lambda)
- [Amazon Lightsail (Simple Cloud Servers - shouldn't be used in your career as a AWS developer)](#amazon-lightsail-simple-cloud-servers---shouldnt-be-used-in-your-career-as-a-aws-developer)
- [Other Compute Services Summary](#other-compute-services-summary)
- [Lambda summary](#lambda-summary)

# Docker
- Docker is a software development platform to deploy apps
- Apps are packaged in containers that can be run on any OS
- Apps run the same, regardless of where they run
	- Any machine
	- No compatibility issues
	- Predictable behavior
	- Less work
	- Easier to maintain and deploy
	- Works with any language, any OS, any technology
- Scale containers up and down very quickly (seconds)

## Where Docker images are stored?
- Docker images are stored in Docker Repositories

- Public: Docker Hub (hub.docker.com)
	- Find base images for many technologies or OS:
		- Ubuntu
		- MySQL
		- Node.js, Java,...

- Private: Amazon ECR (Elastic Container Registry)

## Docker vs Virtual Machines
![](/assets/docker_versus_vm.png)
- Docker is "sort of" a virtualization technology, but not exactly
- Resources are shared with the host => many containers can run on the same machine

# ECS (Elastic Container Service)
![](/assets/ecs_diagram.png)
- Launch Docker containers on AWS
- You must provision & maintain the infrastructure (EC2 instances)
- AWS takes care of starting / stopping containers
- Has integrations with the Application Load Balancer

# Fargate
![](/assets/fargate.png)
- Launch Docker containers on AWS
- You don't need to provision the underlying infrastructure (EC2 instances) - simpler!
- Serverless offering
- AWS just runs the containers for you based on the CPU / RAM you need

# ECR (Elastic Container Registry)
![](/assets/ecr.png)
- Private Docker Registry on AWS
- This is where you store your Docker images so they can be run by ECS / Fargate

# Serverless
- Serverless is a new paradigm in which the developers don't have to manage servers anymore...
- They just deploy code
- They just deploy... functions!
- Initially... Serverless == FaaS (Function as a Service)
- Serverless was pioneered by AWS Lambda but now also includes anything that's managed: databases, messaging, storage, etc...
- Serverless does not mean there are no servers, it means you don't manage them / provision / see them

## Lambda
### Why Lambda?
![](/assets/why_lambda.png)

### Benefits of AWS Lambda
- Easy pricing:
	- Pay per request and compute time
	- Free tier of 1M requests per month and 400,000 GB-seconds of compute time per month
- Integrated with the whole AWS suite of services
- Event-Driven: functions get invoked by AWS when needed
- Integrated with many programming languages
- Easy monitoring through AWS CloudWatch
- Easy to get more resources per function (up to 10 GB of RAM)
- Increasing RAM will also improve CPU and network

### AWS Lambda language support
- Node.js
- Python
- Java (Java 8 compatible)
- C# (.NET Core)
- Go
- C# / Powershell
- Ruby
- Custom Runtime API (community supported, example Rust)

- Lambda Container Image:
	- The container image must implement the Lambda Runtime API
	- ECS / Fargate is preferred for running arbitrary Docker images

### Example: Serverless Thumbnail creation
![](/assets/use_case_lambda.png)

### Example: Serverless CRON Job
![](/assets/use_case_lambda_2.png)

### AWS Lambda Pricing: example
- You can find overall pricing here: https://aws.amazon.com/lambda/pricing/
- Pay per calls:
	- First 1M requests are free
	- $0.20 per 1M requests thereafter ($0.0000002 per request)
- Pay per duration: (in increment of 1 ms)
	- 400,000 GB-seconds of compute time per month are free
	- == 400,000 seconds if you use 1 GB of RAM
	- == 3,200,000 seconds if you use 128 MB of RAM
	- After that $1.00 for every 600,000 GB-seconds
- It is usually very cheap to run Lambda functions so it's very popular

#### Amazon API Gateway
![](/assets/amazon_api_gateway_with_lambda.png)
- Fully managed service to create, publish, maintain, monitor, and secure APIs at any scale
- Serverless and scalable
- Support RESTful APIs and WebSockets APIs
- Support for security, user authentication, API throttling, API keys, monitoring,...

## AWS Batch
- Fully managed batch processing at any scale
- Efficiently run 100,000s of computing batch jobs on AWS
- A "batch" job is a job with a start and an end (opposite of a serverless function)
- Batch will dynamically launch EC2 instances or Spot Instances
- AWS Batch provisions the right amount of compute / memory
- You submit or schedule batch jobs and AWS Batch does the rest!
- Batch jobs are defined as Docker images and run on ECS
- Helpful for cost optimizations and focusing less on the infrastructure

### AWS Batch example
![](/assets/batch_job_example.png)

## Batch vs Lambda
![](/assets/batch_versus_lambda.png)

# Amazon Lightsail (Simple Cloud Servers - shouldn't be used in your career as a AWS developer)
- Virtual servers, storage, databases and networking for a low, predictable price
- Simpler alternative to using EC2, RDS, EBS, ELB, Route53, etc...
- Great for people with less cloud experience!
- Can setup notifications and monitoring of your Lightsail resources
- Use cases:
	- Simple web applications (has templates for LAMP, Nginx, MEAN, Node.js, ...)
	- Websites (Wordpress, Magento, Plesk, Joomla, Drupal, ...)
	- Dev / Test environments
- Has high availablity (zones) but no auto-scaling (need to manually upgrade), limited AWS integrations

# Other Compute Services Summary
- Docker: container technology to run your apps anywhere
- ECS: run Docker containers on EC2 instances
- Fargate:
	- Run Docker containers without provisioning the underlying infrastructure
	- Serverless offering (no EC2 instances to manage)
- ECR: store your Docker images
- Batch: run batch jobs on AWS across managed EC2 instances
- Lightsail: simple cloud servers, great for people with less cloud experience. Predictable & low pricing for simple application & DB stacks.

# Lambda summary
- Lambda is Serverless, Functions as a Service (FaaS), seamless scaling, reactive
- Lambda Billing:
	- By the time run (in ms) and by the memory allocated (RAM, in GB) provisioned
	- By the number of invocations
- Language support: many programming languages supported except (arbitrary) Docker images
- Invocation time: up to 15 minutes
- Use cases:
	- Create thumbnails of images uploaded to S3
	- Run a serverless CRON job
- API Gateway: expose Lambda functions as HTTP API