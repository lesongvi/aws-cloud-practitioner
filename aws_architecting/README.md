<!--
 Copyright 2024 lesongvi
 
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
 
     https://www.apache.org/licenses/LICENSE-2.0
 
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->

# AWS WhitePapers Well-Architected Framework
- Stop guessing your capacity needs
- Test systems at production scale
- Automate to make architectural experimentation easier
- Allow for evolutionary architectures
	- Design based on changing requirements
- Drive architectures using data
- Improve through game days
	- Simulate applications for flash sales, etc.

## AWS Cloud Best Practices - Design Principles
- **Scalability**: vertical and horizontal scaling
- **Disposable Resources**: servers should be disposable & easily configurable
- **Automation**: Serverless, Infrastructure as Service, Auto Scaling,...
- **Loose Coupling**:
	- *Monolith* are applications that do more and more over time, become bigger and more complex
	- Break it down into smaller, loosely coupled components
	- A change or a failure in one component should not affect the entire system
- **Services, not Servers**:
	- Don't use just EC2 instances, use managed services, databases, serverless, etc.

## Well-Architected Framework - 6 Pillars
1. **Operational Excellence**
2. **Security**
3. **Reliability**
4. **Performance Efficiency**
5. **Cost Optimization**
6. **Sustainability**

- <u>**They are not something to balance, or trade-offs, they're a synergy**</u>

### Pillar 1: Operational Excellence
- Includes the ability to run and monitor systems to deliver business value and to continually improve supporting processes and procedures
- Design Principles:
	- **Perform operations as code** - Infrastructure as Code
	- **Make frequent, small, reversible changes** - So that in case of failure, you can revert back. E.g., Blue-Green Deployment
	- **Refine operations procedures frequently** - And ensure that team members are familiar with it
	- **Anticipate failure** - Use Chaos Engineering, Game Days, etc.
	- **Learn from all operational failures** - Post-mortem analysis, etc.
	- **Use managed services** - To reduce the operational burden
	- **Implement observability for actionable insight** - performance, reliability, cost... E.g., CloudWatch, X-Ray, etc.

#### For educational purposes only
![](/assets/pillar_1-aws_services_educative.png)

### Pillar 2: Security
- Includes the ability to protect information, systems, and assets while delivering business value through risk assessments and mitigation strategies
- Design Principles:
	- **Implement a strong identity foundation** - Centralize privilege management and reduce (or even eliminate) reliance on long-term credentials - Principle of Least Privilege - IAM, MFA, etc.
	- **Enable traceability** - Integrate logs and metrics with systems to automatically respond and take action - CloudTrail, Config, etc.
	- **Apply security at all layers** - Like edge network, VPC, subnet, load balancer, every instance, operating system, and application - Security Groups, NACLs, WAF, etc.
	- **Automate security best practices** - Automate security best practices like patch management, AMI, etc. - Inspector, GuardDuty, etc.
	- **Protect data in transit and at rest** - Encryption, tokenization, and access control - KMS, CloudHSM, etc.
	- **Keep people away from data** - Reduce or eliminate the need for direct access or manual processing of data - S3, Glacier, etc.
	- **Prepare for security events** - Run incident response simulations and use tools with automation to increase your speed for detechtion, investigation, and recovery - Shield, Macie, etc.

#### For educational purposes only
![](/assets/pillar_2-aws_services_educative.png)

### Pillar 3: Reliability
- Ability of a system to recover from infrastructure or service disruptions, dynamically acquire computing resources to meet demand, and mitigate disruptions such as misconfigurations or transient network issues
- Design Principles:
	- **Test recovery procedures** - Use automation to simulate different failures or to recreate scenarios that led to failures before
	- **Automatically recover from failure** - Anticipate and remediate failures before they occur
	- **Scale horizontally to increase aggregate system availability** - Distribute requests across multiple, smaller resources to ensure that they don't share a common point of failure
	- **Stop guessing capacity** - Maintain the optimal level to satisfy demand without over or under-provisioning - Use Auto Scaling, etc.
	- **Manage change in automation** - Use automation to make changes to your infrastructure.

#### For educational purposes only
![](/assets/pillar_3-aws_services_educative.png)

**Note**: Service Limits is now called "[Service Quotas](/account_management/README.md#aws-service-quotas)".

### Pillar 4: Performance Efficiency
- Includes the ability to use computing resources efficiently to meet system requirements and to maintain that efficiency as demand changes and technologies evolve
- Design Principles:
	- **Democratize advanced technologies** - Advance technologies become services and hence you can focus more on product development
	- **Go global in minutes** - Easy deployment in multiple regions
	- **Use serverless architecture** - Avoid burden of managing servers
	- **Experiment more often** - Easy to carry out comparative testing
	- **Mechanical sympathy** - Be aware of all AWS services and how they can be used to improve performance

#### For educational purposes only
![](/assets/pillar_4-aws_services_educative.png)

### Pillar 5: Cost Optimization
- Includes the ability to run systems to deliver business value at the lowest price point
- Design Principles:
	- **Adopt a consumption model** - Pay only for what you use
	- **Measure overall efficiency** - Monitor and analyze the cost of your resources. E.g., CloudWatch, Cost Explorer, etc.
	- **Stop spending money on data center operations** - AWS does the infrastucture part and enables customer to focus on organization projects
	- **Analyze and attribute expenditure** - Accurate identification of system usage and costs, helps measure return on investment (ROI) - Make sure to use tags
	- **Use managed services to reduce cost of ownership** - As managed services operate at cloud scale, they can offer a lower cost per transaction or service

#### For educational purposes only
![](/assets/pillar_5-aws_services_educative.png)

*Note*: **Reversed Instance Reporting** is the service that makes sure that if we do reserve an instance, we are actually using it, and not just paying for unused Reserved Instances.

### Pillar 6: Sustainability
- The sustainability pillar focuses on minimizing the environmental impacts of running cloud workloads
- Design Principles:
	- **Understand your impact** - Establish performance indicators, evaluate improvements
	- **Establish sustainability goals** - Set long-term goals for each workload, model return on investment (ROI)
	- **Maximize utilization** - Right size each workload to maximize the energy efficiency of the underlying hardware and minimize idle resources
	- **Anticipate and adopt new, more efficient hardware and software offerings** - And design for flexibility to adopt new technologies over time.
	- **Use managed services** - Shared services reduce the amount of infrastructure; Managed services help automate sustainability best practices as moving infrequent accessed data to cold storage and adjusting compute capacity.
	- **Reduce the downstream inpact of your cloud workloads** - Reduce the amount of energy or resources required to use your services and reduce the need for your customers to upgrade their devices.

#### For educational purposes only
![](/assets/pillar_6-aws_services_educative.png)

## AWS Well-Architected Tool
- Free tool to **review your architecture** against the 6 pillars Well-Architected Framework and **adopt architectural best practices**
- How does it work?
	- Select your workload and answer questions
	- Review your answers against the 6 pillars
	- Obtain advice: get videos and documentations, generate a report, see the results in a dashboard

**Note**: This tool somehow makes me think of the Coding Checklists, where you can review your code against a set of best practices.

## AWS Customer Carbon Footprint Tool
![](/assets/aws_customer_carbon_footprint_tool.png)

## AWS Cloud Adoption Framework (CAF)
- Helps you build and then execute a comprehensive plan for your digital transformation through innovative use of AWS
- Created by AWS Professionals by taking advantage of AWS Best Practices and lessons learned from thousands of customers
- AWS CAF identifies specific organizational capabilities that are essential to the success of your cloud adoption transformation
- AWS CAF groups its capabilities in six perspectives:
	1. **Business Perspective**
	2. **People Perspective**
	3. **Governance Perspective**
	4. **Platform Perspective**
	5. **Security Perspective**
	6. **Operations Perspective**

### CAF Perspectives and Foundational Capabilities - Business Capabilities
- **Business Perspective** helps ensure that your cloud investment accelerate your digital transformation ambitions and business outcomes.
- **People Perspective** serves **as a bridge between technology and business**, accelerating the cloud journey to help organizations more rapidly evolve to a culture of continuous growth, learning, and where change becomes business-as-normal, with focus on culture, organizational structure, leadership, and workforce.
- **Governance Perspective** helps you orchestrate your cloud initiatives while maximizing organizational benefits and minimizing transformation-related risks.
  
![](/assets/caf_perspectives_and_foundational_capabilities_business_capabilities.png)

### CAF Perspectives and Foundational Capabilities - Technical Capabilities
- **Platform Perspective** helps you build an enterprise-grade, scalable, hybrid cloud platform; modernize existing workloads; and implement new cloud-native solutions.
- **Security Perspective** helps you achieve the confidentiality, integrity, and availability of your data and cloud workloads.
- **Operations Perspective** helps ensure that your cloud services are delivered at a level that meets the needs of your business.
  
![](/assets/caf_perspectives_and_foundational_capabilities_technical_capabilities.png)

### Cloud transformation value chain
![](/assets/cloud_transformation_value_chain.png)

### AWS CAF - Transformation Domains
- **Technology** - using the cloud to migrate and modernize legacy infrastructure, applications, data and analytics platforms...
- **Process** - digitizing, automating, and optimizing your business operations
	- leveraging new data and analytics platforms to create actionable insights
	- using machine learning (ML) to improve your customer service experience...
- **Organization** - Reimagining your operating model
	- Organizing your teams around products and value streams
	- Leveraging agile method to rapidly iterate and evolve
- **Product** - reimagining your business model by creating new value propositions (products & services) and revenue models

### AWS CAF - Transformation Phases
- **Envision** - demonstrate how the Cloud will accelerate business outcomes by identifying transformation opportunities and create a foundation for your digital transformation
- **Align** - identify capability gaps across the 6 AWS CAF Perspectives which results in an Action Plan
- **Launch** - build and deliver pilot initiatives in production and demonstrate incremental business value
- **Scale** - expand pilot initiatives to the desired scale while realizing the desired business benefits

## Right Sizing
- EC2 has many instance types, but choosing the most powerful instance type is not always the best choice, because the cloud is elastic and you can scale up or down as needed
- Right sizing is the process of matching instance type and sizes to your workload performance and capacity requirements **at the lowest possible cost**
- **Scaling up is easy so always start small and scale up as needed**
- It's also the process of looking at deployed instances and identifying opportunities to eliminate or downsize without compromising capacity or other requirements, which results in lower costs
- It's important to Right Size...
	- **before a Cloud Migration** - to avoid over-provisioning
	- **continuously after the cloud onboarding process (requirements change over time)** - to avoid under-provisioning
- CloudWatch, Cost Explorer, Trusted Advisor, 3rd party tools can help.

## AWS Ecosystem
### AWS Ecosystem - Free resources
![](/assets/aws_ecosystem-free_resources.png)

### AWS Ecosystem - AWS Support
![](/assets/aws_ecosystem-aws_support.png)

### AWS Marketplace
![](/assets/aws_marketplace.png)

### AWS Training
![](/assets/aws_training.png)

### AWS Professional Services & Partner Network
![](/assets/aws_professional_services_and_partner_network.png)

## AWS IQ & re:Post
### AWS IQ
![AWS IQ](/assets/aws_iq.png)

### re:Post
![re:Post](/assets/re_post.png)

## AWS re:Post - Knowledge Center
![](/assets/aws_re-post-knowledge_center.png)

## AWS Managed Services (AMS)
![](/assets/aws_managed_services.png)
  
![](/assets/aws_managed_services_diagram.png)
