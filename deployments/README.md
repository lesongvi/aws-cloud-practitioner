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

# CloudFormation
## What is CloudFormation?
- CloudFormation is a declarative way of outlining your AWS Infrastructure, for any resources (most of them are supported)
- For example, within a CloudFormation template, you say:
	- I want a security group
	- I want two EC2 machines using this security group
	- I want an S3 bucket
	- I want a load balancer (ELB) in front of these machines

- Then CloudFormation creates those for you, in the right order, with the exact configuration that you specify

## Benefits of CloudFormation
- Infrastructure as code
	- No resources are manually created, which is excellent for control
	- Changes to the infrastructure are reviewed through code

- Cost:
	- Each resources within the stack is stagged with an identifier so you can easily see how much a stack costs you
	- You can estimate the costs of your resources using the CloudFormation template
	- Savings strategy: In Dev, you could automation deletion of templates at 5PM and recreated at 8AM safely

- Productivity:
	- Ability to destroy and re-create an infrastructure on the cloud on the fly
	- Automated generation of Diagram for your templates!
	- Declarative programming (no need to figure out ordering and orchestration)

- Don't re-invent the wheel
	- Leverage existing templates on the web
	- Leverage the documentation

- Supports (almost) all AWS resources
	- Everything we'll see in this course is supported
	- You can use "Custom resources" for resources that are not supported
	- You can call CloudFormation is base of Infrastructure as Code on AWS

## CloudFormation Stack Designer
![](/assets/cloudformation_stack_designer.png)

## AWS Cloud Development Kit (AWS CDK)
![](/assets/aws_cdk.png)
- Define your cloud infrastructure using a familiar programming language:
	- Javascript/Typescript, Python, Java and .NET
- The code is "compiled" into CloudFormation template (JSON/YAML) and deployed using CloudFormation
- You can therefore deploy infrastructure and application runtime code together:
	- Great for Lambda functions
	- Great for Docker containers in ECS / EKS

### CDK Example
```typescript
export class MyEcsConstructStack extends core.Stack {
	constructor(scope: core.App, id: string, props?: core.StackProps) {
		super(scope, id, props);

		const vpc = new ec2.Vpc(this, 'MyVpc', { maxAzs: 2 }); // default is all AZs in region

		const cluster = new ecs.Cluster(this, 'MyCluster', { vpc });
		
		new ecs_patterns.ApplicationLoadBalancedFargateService(this, 'MyFargateService', {
			cluster, // Required
			cpu: 256, // Default is 256
			desiredCount: 6, // Default is 1
			taskImageOptions: {
				image: ecs.ContainerImage.fromRegistry('amazon/amazon-ecs-sample'), // Required
			},
			memoryLimitMiB: 2048, // Default is 512
			publicLoadBalancer: true, // Default is false
		});
	}
}
```

## AWS Elastic Beanstalk
- Elastic Beanstalk is a developer centric view of deploying an application on AWS
- It uses all the component we have seen before:
	- EC2, ASG, ELB, RDS, etc...
- But it's all in one view that's easy to make sense of!
- We still have full control over the configuration

- Beanstalk = Platform as a Service (PaaS)
- Beanstalk is free but you pay for the underlying instances

<small>*Note: All the configuration is actually done through CloudFormation behind the scenes*</small>

### What Beanstalk does under the hood?
- Managed Service
	- Instance configuration / OS is handled by Beanstalk
	- Deployment strategy is configurable but performed by Beanstalk
	- Capacity provisioning
	- Load balancing & auto scaling
	- Application health monitoring & responsiveness

- What you're responsible for: Just the application code is the responsibility of the developer

- Three architecture models:
	- Single Instance deployment: good for dev
	- LB + ASG: great for production or pre-production web applications
	- ASG only: great for non-web apps in production (workers, etc...)

### What platforms does Beanstalk support?
- Support for many platforms:
	- Go
	- Java SE
	- Java with Tomcat
	- .NET on Windows Server with IIS
	- Node.js
	- PHP
	- Python
	- Ruby
	- Packer Builder
	- Single Container Docker
	- Multi-Container Docker
	- Preconfigured Docker
	- ... If not supported, you can write your own custom platform (advanced)

### AWS Beanstalk - Health Monitoring
![](/assets/beanstalk_health_monitoring.png)

# AWS CodeDeploy
![](/assets/codedeploy.png)
- We want to deploy our application automatically to many EC2 instances

- Works with EC2 Instances
- Works with On-Premises Servers
- Hybrid service

- Server / Instances must be provisioned and configured ahead of time with the CodeDeploy Agent

# AWS CodeCommit
![](/assets/codecommit.png)

# AWS CodeBuild
![](/assets/codebuild.png)

# AWS CodePipeline
![](/assets/codepipeline.png)

# AWS CodeArtifact
![](/assets/codeartifact.png)

# AWS CodeStar
![](/assets/codestar.png)

# AWS Cloud9
![](/assets/aws_cloud9.png)

# AWS Systems Manager (SSM)
- Helps you manage your EC2 and On-Premises systems at scale
- Another hybrid AWS service
- Get operational insights about the state of your infrastructure
- Suite of 10+ products
- Most important features are:
	- Patching automation for enhanced compliance
	- Run commands across an entire fleet of servers
	- Store parameter configuration using Parameter Store
- Works for Linux, Windows, MacOS, and Raspberry Pi OS (Raspbian)
## How SSM works
![](/assets/how_ssm_works.png)

## SSM Session Manager
![](/assets/ssm_session_manager.png)

# AWS OpsWorks
## Chef and Puppet needed => AWS OpsWorks
![](/assets/aws_opsworks_chef_puppet.png)

## Why OpsWorks?
![](/assets/opsworks_architecture.png)
- OpsWorks is working like Elastic Beanstalk but for Chef and Puppet
- The only reason you wanted to use OpsWorks is if you're already using Chef and Puppet, and you want to migrate your configuration to AWS and to reuse your existing Chef and Puppet templates

# Deployment summary
- CloudFormation: (AWS only)
	- Infrastructure as Code, works with almost all AWS resources
	- Repeat across regions / accounts
- Beanstalk: (AWS only)
	- Platform as a Service (PaaS), limited to certain programming languages / Docker
	- Deploy code consistently with a known architecture: ex, ALB + EC2 + RDS
- CodeDeploy: (hybrid): Deploy & upgrade any application onto servers
- System Manager (SSM): (hybrid): Patch, configure and run commands at scale
- OpsWorks: (hybrid): Managed Chef & Puppet in AWS

# Developer Services Summary
- CodeCommit: (AWS only): Store code in private Git repositories (version controlled)
- CodeBuild: (AWS only): Build & test code in AWS
- CodeDeploy: (hybrid): Deploy code onto servers
- CodePipeline: (hybrid): Orchestration of pipeline (from code to build to deploy)
- CodeArtifact: (AWS only): Store software packages / dependencies on AWS
- CodeStar: Unified view for allowing developers to do CICD and code
- Cloud9: Cloud IDE (Integrated Development Environment) with collaboration features
- AWS CDK: Define your cloud infrastructure using a programming language