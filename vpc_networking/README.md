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
- [VPC - Crash Course](#vpc---crash-course)
	- [IP Addresses](#ip-addresses)
	- [VPC & Subnets Primer](#vpc--subnets-primer)
	- [VPC Diagram](#vpc-diagram)
	- [Internet Gateway (IGW) & NAT Gateway](#internet-gateway-igw--nat-gateway)
	- [Network ACL (NACL) & Security Groups](#network-acl-nacl--security-groups)
	- [VPC Flow Logs](#vpc-flow-logs)
	- [VPC Peering](#vpc-peering)
	- [VPC Endpoints](#vpc-endpoints)
	- [AWS PrivateLink (Interface VPC Endpoints)](#aws-privatelink-interface-vpc-endpoints)
	- [Site-to-Site VPN & Direct Connect](#site-to-site-vpn--direct-connect)
		- [Site-to-Site VPN](#site-to-site-vpn)
	- [AWS Client VPN](#aws-client-vpn)
	- [Transit Gateway (TGW) - Overview](#transit-gateway-tgw---overview)
		- [Network topologies can become complex](#network-topologies-can-become-complex)
		- [Solution: Transit Gateway](#solution-transit-gateway)
- [VPC & Networking - Summary](#vpc--networking---summary)
	- [NAT Instance vs NAT Gateway](#nat-instance-vs-nat-gateway)

# VPC - Crash Course
- VPC is something you should know in depth for the AWS Certified Solutions Architect Associate & AWS Certified SysOps Administrator
- At the AWS Certified Cloud Practitioner Level, you should know about:
	- VPC, Subnets, Internet Gateways & NAT Gateways
	- Security Groups, Network ACL (NACL), VPC Flow Logs
	- VPC Peering, VPC Endpoints
	- Site-to-Site VPN & Direct Connect
	- Transit Gateway

- I will just give you an overview, less than 1 or 2 questions at your exam
- We'll have a look at the "default VPC" (created by default by AWS for you)
- There is a summary lecture at the end. It's okay if you don't understand everything right now, we'll go through it again at the end.

## IP Addresses
- IPv4 - Internet Protocol version 4 (4.3 billion addresses)
	- Public IPv4 - can be used on the internet
	- EC2 instance gets a new public IP address everytime you stop then start it (default)
	- Private IPv4 - can be used on private network (LAN) such as internal AWS networking (eg. 192.168.1.1)
	- Private IPv4 is fixed for EC2 Instances even if you start/stop them
- Elastic IP - allows you to attach a fixed public IPv4 address to EC2 instance
	- Note: has ongoing cost if not attached to EC2 instance or if the EC2 instance is stopped
- IPv6 - Internet Protocol version 6 (3.4 x 10^38 addresses)
	- Every IP address is public (no private range)
	- Example: 2001:d08:0:0:0:ff:fe00:1234

## VPC & Subnets Primer
![](/assets/vpc_subnet_primer.png)

## VPC Diagram
![](/assets/vpc_diagram.png)

## Internet Gateway (IGW) & NAT Gateway
![](/assets/internet_gateway_and_nat_gateways.png)

## Network ACL (NACL) & Security Groups
![](/assets/network_acl_and_security_groups.png)

| Security Groups | Network ACL |
| --------------- | ----------- |
| Operates at the instance level (first layer of defense) | Operates at the subnet level (second layer of defense) |
| Supports allow rules only | Supports allow rules and deny rules |
| Is stateful: Return traffic is automatically allowed, regardless of any rules | Is stateless: Return traffic must be explicitly allowed by rules |
| We evaluate all rules before deciding whether to allow traffic | We process rules in number order when deciding whether to allow traffic |
| Applies to an instance only if someone specifies the security group when launching the instance, or associates the security group with the instance later on | Automatically applies to all instances in the subnets it's associated with (backup layer of defense, so that even if someone changes the security groups on the instance - an admin for example - the subnet NACL will still provide protection) |

## VPC Flow Logs
- Capture information about IP traffic going into your interfaces (eg. network interfaces attached to EC2 instances)
	- VPC Flow Logs
	- Subnet Flow Logs
	- Elastic Network Interface Flow Logs
- Helps to monitor & troubleshoot connectivity issues. Example:
	- Subnets to internet
	- Subnets to subnets
	- Internet to subnets
- Captures network information from AWS managed interfaces too: Elastic Load Balancers, ElastiCache, RDS, Aurora, etc...
- VPC Flow Logs data can go to S3, CloudWatch Logs or Kinesis Data Firehose

## VPC Peering
![](/assets/vpc_peering.png)

## VPC Endpoints
- VPC Endpoints is used to connect your AWS resources without using the public internet, but using the AWS private network
- For Amazon S3 and DynamoDB, use Gateway Endpoints
- For the rest, use Interface Endpoints (powered by AWS PrivateLink)
![](/assets/vpc_endpoints.png)

## AWS PrivateLink (Interface VPC Endpoints)
![](/assets/aws_privatelink.png)

## Site-to-Site VPN & Direct Connect
![](/assets/site_to_site_vpn_and_direct_connect.png)

### Site-to-Site VPN
![](/assets/site-to-site_vpn.png)

## AWS Client VPN
![](/assets/aws_client_vpn.png)

## Transit Gateway (TGW) - Overview
### Network topologies can become complex
![](/assets/network_topologies_can_become_complicated.png)

### Solution: Transit Gateway
![](/assets/transit_gateway.png)

# VPC & Networking - Summary
- VPC: Virtual Private Cloud
- Subnets: Tied to an AZ, Network partition of a VPC
- Internet Gateway: IGW, at the VPC level, provide internet access
- NAT Gateway / NAT Instance: Give internet access to private subnets
- NACL: Stateless, subnet rules for inbound and outbound
- Security Groups: Stateful, operate at the EC2 instance level or ENI (Elastic Network Interface) level
- VPC Peering: Connect 2 VPC with non overlapping IP ranges, non transitive (which means if VPC A can talk to VPC B, and VPC B can talk to VPC C, VPC A cannot talk to VPC C)
- Elastic IP: fixed public IPv4 for an instance, ongoing cost if not attached
- VPC Endpoints: Provide private access to AWS Services (Gateway Endpoints for S3 and DynamoDB, Interface Endpoints for the rest) within your VPC
- PrivateLink: Privately connect to a service in a 3rd party VPC
- VPC Flow Logs: network traffic logs at the VPC, Subnet or ENI level
- Site-to-Site VPN: VPN over public internet between on-premises DC and AWS. Quick to setup, but not private.
- Client VPN: OpenVPN connection from your computer into your VPC
- Direct Connect: Direct private connection to AWS (hybrid cloud). Slow to setup, but private.
- Transit Gateway: Transit hub to connect VPC and on-premises networks, connect thousands of VPC and on-premises using TGW

## NAT Instance vs NAT Gateway
- NAT Instance is for legacy reasons, don't use it anymore
- NAT Gateway is managed by AWS, highly available, no need to patch