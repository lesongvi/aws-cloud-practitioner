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

# Table of Contents
- [AWS STS (Security Token Service)](#aws-sts-security-token-service)
- [Amazon Cognito (simplified)](#amazon-cognito-simplified)
- [Microsoft Active Directory (AD)](#microsoft-active-directory-ad)
	- [What is Microsoft Active Directory (AD)?](#what-is-microsoft-active-directory-ad)
	- [AWS Directory Services](#aws-directory-services)
- [AWS IAM Identity Center (successor to AWS Single Sign-On)](#aws-iam-identity-center-successor-to-aws-single-sign-on)
	- [AWS IAM Identity Center - Login Flow](#aws-iam-identity-center---login-flow)
- [Advanced Identity summary](#advanced-identity-summary)

# AWS STS (Security Token Service)
![](/assets/aws_sts.png)

# Amazon Cognito (simplified)
![](/assets/amazon_cognito.png)

# Microsoft Active Directory (AD)
## What is Microsoft Active Directory (AD)?
![](/assets/what_is_microsoft_active_directory.png)

## AWS Directory Services
- This is Solution Architect Associate level topic.
  
![](/assets/aws_directory_services.png)

# AWS IAM Identity Center (successor to AWS Single Sign-On)
![](/assets/aws_iam_identity_center.png)

## AWS IAM Identity Center - Login Flow
![](/assets/aws_iam_identity_center_login_flow.png)

# Advanced Identity summary
- **IAM**:
	- Identity and Access Management inside your AWS account
	- For users that you trust and belong to your organization
- **Organization**: Manage multiple accounts
- **Security Token Service (STS)**: Temporary, limited-privilege credentials to access AWS resources
- **Cognito**: Create a database of users for your web and mobile applications
- **Directory Services**: Integrate Microsoft Active Directory with AWS
- **IAM Identity Center**: One login for multiple AWS accounts and applications