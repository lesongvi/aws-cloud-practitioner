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

# Table of Contents
- [Elastic Load Balancing (ELB) & Auto Scaling Groups (ASG)](#elastic-load-balancing-elb--auto-scaling-groups-asg)
	- [Scalability & High Availability](#scalability--high-availability)
		- [Vertical Scalability](#vertical-scalability)
		- [Horizontal Scalability](#horizontal-scalability)
		- [High Availability](#high-availability)
		- [High Availability vs Scalability summary](#high-availability-vs-scalability-summary)
		- [Scalability vs. Elasticity (vs. Agility)](#scalability-vs-elasticity-vs-agility)
	- [Elastic Load Balancer (ELB)](#elastic-load-balancer-elb)
	- [Auto Scaling Group (ASG)](#auto-scaling-group-asg)
		- [ASG works hand in hand with ELB](#asg-works-hand-in-hand-with-elb)
			- [ASG Scaling Strategies](#asg-scaling-strategies)
	- [ELB & ASG Summary](#elb--asg-summary)

# Elastic Load Balancing (ELB) & Auto Scaling Groups (ASG)
## Scalability & High Availability
- Scalability means that an application / system can handle greater loads by adapting
- There are two kinds of scalability:
	- Vertical Scalability: Increase the size of the instance (more RAM, CPU, etc.)
	- Horizontal Scalability (aka Elasticity): Increase the number of instances/systems for your application (clones)
- Scalability is linked but different to High Availability

### Vertical Scalability
- Increase the size of the instance
- For example, your application runs on a `t2.micro` and you want to increase performance. You can upgrade it to a `t2.large`.
- Vertical scalability is very common for non distributed systems, such as a database.
- There's usually a limit to how much you can vertically scale (hardware limit)

### Horizontal Scalability
- Increase the number of instances that serve your application
- For example, your application runs on a `t2.micro` and you want to increase performance. You can add another `t2.micro`.
- Horizontal scalability is very common for distributed systems, such as web applications.
- Horizontal scalability is easier to achieve than vertical scalability

### High Availability
- High Availability usually goes hand in hand with Horizontal Scalability
- High Availability means running your application / system in at least 2 data centers (aka Availability Zones)
- The goal of High Availability is to survive a data center loss (disaster) and still be able to serve your customers

### High Availability vs Scalability summary
- Scalability:
	- Vertical: Increase size of the instance (scale up / down): From `t2.nano` to `u-12tb1.metal`...
	- Horizontal: Increase number of instances / systems (scale out / in): From 1 to 100 instances...
		- Auto Scaling Group (ASG) will help with Horizontal Scalability
		- Elastic Load Balancer (ELB) will help with Horizontal Scalability
- High Availability: Run your app / system in multiple data centers (AZ)
	- Auto Scaling Group (ASG) will help with High Availability on multiple data centers (AZ)
	- Elastic Load Balancer (ELB) will help with High Availability on multiple EC2 instances

### Scalability vs. Elasticity (vs. Agility)
- Scalability: Ability to accomodate a larger load by making the hardware stronger (scale up / scale vertically) or by adding nodes (scale out / scale horizontally)
- Elasticity: Once a system is scalable, elasticity means that there will be some "auto-scaling" so that the system can scale based on actual load. This is "cloud-friendly": pay-per-use, match demand, optimize cost
- Agility: Ability of an organization to transform itself. Agility allows the organization to adapt to market evolution and to "get ahead of the competition". Agility is a business concept, not a technical concept.

## Elastic Load Balancer (ELB)
- Elastic Load Balancer (ELB) automatically distributes incoming application traffic across multiple targets, such as EC2 instances, containers, IP addresses, and Lambda functions
- It can handle the load of your application traffic in a single Availability Zone or across multiple Availability Zones
- ELB scales your load balancer as traffic to your application changes over time
- It costs less to setup your own load balancer with ELB than to do it yourself but it will be a lot more effort on your end (maintenance, integration,...)
![](/assets/alb_nlb_gwlb.png)
- There are 3 types of ELB:
	- Application Load Balancer (ALB, HTTP / HTTPS only) - Layer 7
	- Network Load Balancer (NLB, ultra-high performance, allows for TCP) - Layer 4
	- Gateway Load Balancer (GWLB, for third-party appliances) - Layer 3
	- Classic Load Balancer (CLB, retired in 2023) - Layer 4 & 7

## Auto Scaling Group (ASG)
- In real life, load on your websites and applications can change a lot.
- In the cloud, you can create and get rid of servers very quickly.
- ASG will automatically:
	- Scale out (add EC2 instances) to match an increased load
	- Scale in (remove EC2 instances) to match a decreased load
	- Ensure we have a minimum and a maximum number of machines running
	- Automatically register new instances to a load balancer
	- Replace unhealthy instances
- Huge cost savings: only run at an optimal capacity (principle of the cloud: pay-per-use, low cost, high **elasticity**)

### ASG works hand in hand with ELB
![](/assets/asg_in_aws_with_lb.png)
- As ASG scale in / out, it automatically registers / deregisters instances from ELB

#### ASG Scaling Strategies
- Manual Scaling: You manually change the desired capacity of the ASG
- Dynamic Scaling: You define **policies** to automatically scale in / out
	- Simple / Step Scaling: When a CloudWatch alarm is triggered (example CPU > 70% in 5 mins), then add 2 units. Or when a CloudWatch alarm is triggered (example CPU < 10% in 10 mins), then remove 1 unit
	- Target Tracking Scaling: Most simple and easy to set-up. Example: I want the average ASG CPU to stay at around 40%
	- Scheduled Actions: Anticipate a scaling based on known usage patterns. Example: increase the min capacity to 10 at 5pm on Fridays
	- Predictive Scaling: Uses Machine Learning to predict future traffic and scale accordingly. Automatically provisions the right number of EC2 instances based on predicted demand. Useful when your load has predictable time-based patterns

## ELB & ASG Summary
- High Availability vs. Scalability (Vertical vs. Horizontal) vs. Elasticity vs. Agility in the cloud
- Elastic Load Balancer (ELB):
	- Distributes traffic across backend EC2 instances, can be Multi-AZ
	- Supports health checks
	- 4 types: Classic (old - L4 & L7), Application (HTTP / HTTPS - L7), Network (ultra-high performance, TCP, UCP - L4), Gateway (for third-party appliances - L3)
- Auto Scaling Group (ASG):
	- Implement Elasticity for your application, across multiple AZ
	- Scale EC2 instances based on the demand on your system, replace unhealthy instances
	- Integrate with ELB