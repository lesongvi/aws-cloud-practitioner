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
- [Identity and Access Management (IAM)](#identity-and-access-management-iam)
	- [Quick about IAM Users, Groups, and Policies](#quick-about-iam-users-groups-and-policies)
		- [Tags](#tags)
	- [IAM Users & Groups](#iam-users--groups)
	- [IAM Policies](#iam-policies)
		- [IAM Policy inheritance](#iam-policy-inheritance)
		- [IAM Policy Structure](#iam-policy-structure)
		- [IAM MFAs](#iam-mfas)
		- [Password Policy](#password-policy)
		- [Multi-Factor Authentication (MFA)](#multi-factor-authentication-mfa)
	- [AWS Access Keys, CLI, and SDKs](#aws-access-keys-cli-and-sdks)
		- [AWS CLI](#aws-cli)
			- [AWS CloudShell](#aws-cloudshell)
	- [IAM Roles](#iam-roles)
	- [IAM Security Tools](#iam-security-tools)
	- [IAM Best Practices](#iam-best-practices)
	- [Shared Responsibility Model for IAM](#shared-responsibility-model-for-iam)
	- [IAM Summary](#iam-summary)

# Identity and Access Management (IAM)
- IAM is a global service, not a regional service.
- Root account is created when you first create your AWS account. It has complete Admin access. The only thing should be done with the root account is to create an IAM user with Admin access and use that IAM user for all future work, and lock away the root account credentials.

## Quick about IAM Users, Groups, and Policies
![](/assets/iam_user_group.png)
- Users are people within your organization, and can be grouped, users can be in multiple groups.
- Groups contain users only, not other groups, and can only contain users from the same AWS account.

![](/assets/iam_policies.png)
- Policies are made up of documents, called policy documents, that give permissions as to what the user/group is able to do.
- Users/Groups can be assigned JSON documents called policies. These policies are permission policies. You can attach multiple policies to users/groups.
- **In AWS you apply the least privilege principle.** This means that you only give your users the minimal amount of access required to do their job.

### Tags
- Tags are labels that you can apply to AWS resources.
- It's help to include some extra information about a resource but don't affect the resource function.

## IAM Users & Groups
![](/assets/iam_signin_alias.png)
- Can create alias to sign in link.

## IAM Policies
- IAM policies are written in JSON and have their own structure.
### IAM Policy inheritance
![](/assets/iam_policies_diagram.png)
- IAM policies are attached to users, groups, and roles.
- If you attach a policy to a group, all users in that group will get those permissions.
- If you attach a policy to a user, only that specific user gets the permissions (inline policy).

### IAM Policy Structure
![](/assets/iam_policies_struct.png)
- IAM policies consist of the following elements:
  - Version: The policy language version, always include "2012-10-17"
  - ID: An identifier for the policy (optional)
  - Statement: One or more individual statements. Each statement has the following elements:
	- Sid: An identifier for the statement (optional)
	- Effect: Whether the statement allows or denies access (always include "Allow" or "Deny")
	- Principal: The entity that is allowed or denied access to a resource. This can be a specific IAM user, a group of users, or a role. You can also specify all principals by using the wildcard character (*).
	- Action: The actions or operations that are allowed or denied. For example, "s3:ListBucket" or "s3:*"
	- Resource: The resources to which the actions apply. Statements must include either a Resource or a NotResource element. You specify a resource using its Amazon Resource Name (ARN).
	- Condition: Conditions that specify when a policy is in effect. For example, to control access to a particular Amazon S3 bucket, you could use a condition that requires the request to come from a specific IP address or range of IP addresses. For examples of conditions, see [AWS JSON Policy Elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) (optional).

### IAM MFAs
- MFA is a security feature that provides an additional layer of authentication for your AWS account. Instead of using only a user name and password to sign in, you also use a device, such as a smartphone, an authenticator app, or a hardware device to access your account.

#### Password Policy
- In AWS, you can setup a password policy to enforce password complexity, rotation, and reuse:
	- Set a minimum password length
	- Require specific character types
		- include uppercase letters
		- include lowercase letters
		- include numbers
		- include non-alphanumeric characters
	- Allow all IAM users to change their own passwords
	- Require users to change their password after a specified number of days (password expiration)
	- Prevent password reuse

#### Multi-Factor Authentication (MFA)
- User must setting up MFA for their account.
- MFA is a security feature that provides an additional layer of authentication for your AWS account. Instead of using only a user name and password to sign in, you also use a device, such as a smartphone, an authenticator app, or a hardware device to access your account.
- MFA = password you know (first factor) + authentication code or device you have (second factor)
- MFA devices options in AWS:
	- Virtual MFA device: An MFA device that is associated with a virtual device, such as a smartphone. The virtual device displays a constantly changing authentication code that you must type in addition to your user name and password to log in to AWS. (Google Authenticator, Authy, etc.) - Can be used for multiple accounts.
	- Universal 2nd Factor (U2F) security key: A physical device that plugs into the USB port of your computer and that requires a physical action (touch, press, etc.) to generate an authentication code. (YubiKey, etc.) - Can be used for multiple accounts.
	- Hardware key fob MFA device: A small, programmable hardware device that generates an authentication code. (Gemalto, etc.) - Can be used for multiple accounts. In the AWS GovCloud (US) Region, you can use only hardware key fob MFA devices.

### AWS Access Keys, CLI, and SDKs
- **Access Keys**: Access keys are long-term credentials for an IAM user or the AWS account root user. You can use access keys to sign programmatic requests to the AWS CLI or AWS API (directly or using the AWS SDK).
- Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS. If you don't have access keys, you can create them from the AWS Management Console. When you create access keys, the key pair is active by default, and you can use the pair right away.
- You can have a maximum of two access keys for each IAM user. You can't recover lost secret access keys, but you can create new access keys for your AWS account root user or IAM user at any time. You can also delete access keys that you no longer need.
- You can use access keys to sign programmatic requests to the AWS CLI or AWS API (directly or using the AWS SDK). For example, you can use AWS CLI commands or AWS API operations to create an Amazon S3 bucket, list your available buckets, or delete a bucket.

- Just remember:
	- Access Key ID ~= User ID
	- Secret Access Key ~= Password (so keep it secret!)

- **AWS CLI**: The AWS Command Line Interface (AWS CLI) is an open source tool that enables you to interact with AWS services using commands in your command-line shell. With minimal configuration, the AWS CLI enables you to start running commands that implement functionality equivalent to that provided by the browser-based AWS Management Console from the command prompt in your favorite terminal program.
- The AWS CLI provides direct access to the public API (Application Programming Interface) of AWS services. You can use the AWS CLI to automate AWS services or to write scripts to manage your AWS resources. For example, you can use the AWS CLI to create scripts that create Amazon EC2 instances, or to create bash scripts that manage your AWS resources.

- **AWS SDKs**: AWS SDKs are available for a variety of programming languages and platforms such as Java, JavaScript, .NET, iOS, and Android. The SDKs provide a convenient way to create programmatic access to AWS and other AWS services. For example, the SDKs take care of tasks such as cryptographically signing requests, retrying requests, and handling error responses, making it easier for you to develop applications that are safe to use in production.

#### AWS CLI
- The AWS CLI is a unified tool to manage your AWS services.
- With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.
- You need to install the AWS CLI on your computer and get your access keys to use it.

##### AWS CloudShell
- AWS CloudShell is a browser-based shell that makes it easy to securely manage, explore, and interact with your AWS resources.

### IAM Roles
- IAM roles are a secure way to grant permissions to entities that you trust. Examples of entities include the following:
	- IAM user in another account
	![](/assets/iam_roles_for_services.png)
	- Application code running on an EC2 instance that needs to perform actions on AWS resources
	- An AWS service that needs to act on resources in your account to provide its features

### IAM Security Tools
- IAM Credentials Report (account-level): The IAM credentials report provides you with detailed information about the IAM users and their associated access keys in your AWS account. This report helps you to track and manage the status of all your access keys and passwords. You can use this report to determine which users have passwords and access keys, when the access keys were last used, whether the keys are active, and if the keys are older than 90 days. You can also use this report to determine which users have MFA devices assigned.
- IAM Access Advisor (user-level): The IAM access advisor helps you follow the security best practice of granting least privilege. It analyzes IAM policies attached to an IAM entity and recommends least privilege policies. The access advisor helps you answer the following questions:
	- When was the last time that an IAM entity (a user, group, or role) accessed a specific service?
	- What services does a given IAM entity have access to?
	- Are there IAM entities that have access to a service that they don't need?
	- Are there IAM entities that have not accessed a service that they need to?

### IAM Best Practices
- Don't use the root account except for AWS account setup.
- One physical user = One AWS user.
- Assign users to groups and assign permissions to groups.
- Create a strong password policy.
- Use and enforce the use of multi-factor authentication (MFA).
- Create and use roles for giving permissions to AWS services.
- Use Access Keys for programmatic access (CLI/SDK).
- Audit permissions of your account with the IAM Credentials Report and Access Advisor.
- Never share IAM users & Access Keys.

### Shared Responsibility Model for IAM
![](/assets/iam_shared_responsibility_model.png)

### IAM Summary
- Users: mapped to a physical user, has a password to login to the AWS console, and can have access keys for programmatic access.
- Groups: contain users only, and can only contain users from the same AWS account.
- Policies: are made up of JSON documents, called policy documents, that give permissions as to what the user/group is able to do.
- Roles: are a secure way to grant permissions to entities that you trust. Examples of entities include the following:
	- IAM user in another account
	- Application code running on an EC2 instance that needs to perform actions on AWS resources
	- An AWS service that needs to act on resources in your account to provide its features
- Security: MFA + password policy
- AWS CLI: unified tool to manage your AWS services
- AWS SDKs: available for a variety of programming languages and platforms such as Java, JavaScript, .NET, iOS, and Android
- Access Keys: long-term credentials for an IAM user or the AWS account root user. You can use access keys to sign programmatic requests to the AWS CLI or AWS API (directly or using the AWS SDK).
- Audit: IAM Credentials Report (account-level) and IAM Access Advisor (user-level)