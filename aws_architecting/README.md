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

## AWS Customer Carbon Footprint Tool

## Right Sizing

## AWS Ecosystem

## AWS IQ & re:Post

## AWS Knowledge Center

## AWS Managed Services
