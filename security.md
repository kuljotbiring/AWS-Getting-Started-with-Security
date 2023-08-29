  <p align="left">
  <a href="https://skillicons.dev">
     <img src="https://skillicons.dev/icons?i=aws" />
  </a>
</p>


## **Preface**
Recently, I completed my latest bade **AWS Badge - Getting Started with Security** via [AWS Educate](https://aws.amazon.com/education/awseducate/). This course and lab was by far my favorite of the badges due to how well the lectures and labs tied in to the learning material.

Since I have a great interest in cloud security, I am using this blog post to document my learning and experience. Hopefully it can also serve a primer for beginners or a refresher for the more seasoned readers.

In order to discuss security in AWS we need to get a few definitions out of way which will mold our discussion.

**Least privilege:** granting minimal privileges to a user based on their needs. Least privilege is an principal in cybersecurity that we should strive for in order to keep our systems safe.

**Authentication:** The process of verifying the identity of a user, system, or entity. It ensures that the claimed identity matches the actual identity.

**Authorization:** The process of granting or denying access rights and permissions to authenticated users or entities. It defines what actions or resources a user is allowed to access or manipulate based on their authenticated identity and roles.

## **IAM Overview:** 
Identity access management. IAM relies on authentication and authorization.

**Example:**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/20suj6byjtmvtd8b0w5q.png)

Now that we have some basic concepts out of the way, we need to discuss the layers of security and how they will tie into AWS cloud security.

## **The Four Layers of Security**
1. Perimeter layer - physical protections in place.
2. Environment layer - environmental factors for consideration and availability.
3. Infrastructure layer - building and hardware - power, climate control, monitoring, fire detection.
4. Data layer - most critical layer. Storage devices decommissioned using NIST techniques to destroy customer data, AWS is audited by external auditors that establish rules to obtain security certifications. AWS services can notify employee attempts to remove data

If you are thinking ahead, you must be wondering well if everything I have is being hosted in the cloud, then who is responsible for the security layers mentioned above? :man_shrugging:

This is a great segway into the shared responsibility model.

## **Shared Responsibility Model** :handshake:

In essence, the shared responsibility model (as it pertains to cloud security in AWS) basically states that AWS is responsible for security _**OF**_ the cloud - and the customer is responsible for security _**IN**_ the cloud.

Lets take a look at some examples of what AWS would be responsible for:

- Physical security of data centers.
- Hardware and software infrastructure.
- Network infrastructure.
- Virtualization infrastructure.

Now, lets look at some examples of what the customer is responsible for:

- Complete control of data and content.
- Manage security requirements of your content.
- Includes contents which you store, AWS services you use and who has access to that data. You also control how access rights are granted, managed and revoked.

## **AWS Identity and Access Management (IAM)**

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ojjovq5q0p6subugqd5x.png)

**IAM defined:** IAM is a service to determine who or what can access services.

**Identity:** A person or service that uses IAM to access AWS

We can intuitively determine what a service in AWS would be (there are many), but what exactly do we mean by user?

Keep the following in mind. A user will have the following attributes:

- name
- password
- up to two access keys (can be used via CLI or API).

Remember how we discussed least privilege earlier? Well the good news is that IAM has no permissions by default. The bad news is that you must manage permissions and attach IAM permissions via policies to users.

With respect to IAM, there are few terms that are essential to be familiar with so that policies can be properly applied. These terms are IAM policies, IAM groups, and IAM roles.

**IAM Policy**: is a JSON document the defines specific actions an entity can perform on specific AWS services or resources.

**IAM Group:** A group of IAM users with the same permissions. All users inherit permissions of the group they are assigned to.

**IAM Roles:** Short term method of assigning permission similar to a user, without having credentials or access keys. IAM Roles are not associated with a person but are for anyone who needs short term access.

Now that we have a high level overview of the pieces of IAM, lets dive deeper into each of these topics.

**What are the use cases of IAM?**

- Apply detailed permissions:
Create and apply permissions based on user attributes (eg job, role and team) by using attribute-based access control.

- Manage per-account and application access:
Ability to provide multi-account access and application management across AWS.

- Establish organization-wide guardrails on AWS.
Use service control policies to establish permissions for IAM users and roles and implement a data perimeter around your accounts.

- Set, verify, and right-size permissions.
Control permissions in accordance to least-privilege. Streamline permission management and use cross-account findings to set, verify and tune policies.

As defined earlier, to work with IAM you can create; Users, Groups and Roles. It should be noted that each of entities will require a specific IAM policy. More on policies later.

To use IAM you can use one of the following:

* AWS Management Console:
A GUI to launch, configure and manage AWS services.

* AWS CLI:
A Unified tool to manage AWS services. Ability to download, configure, provision and control multiple AWS services from CLI and automate them via scripts.

* AWS SDK:
Provides language-specific APIs to launch services using a wide variety of programming languages.

**IAM Role**
A role is an entity that you can create in your account that has a specific permissions. You can attach a policy to a role. They can be assigned to an IAM user, an AWS service or an external service.

When a user or service assumes a role, it inherits the permissions temporarily. When the role is returned, the user or service no longer has the permissions unless the role is assumed again. The benefit of a role is specified specified security credentials not being tied to an entity

Users and services don't automatically have access to roles. This needs to be configured in the rust policy of the role. The trust policy is a JSON document where you define the principal(s) - entities which have access to the roles) which can perform actions and have access to resources. 

Principals can be:
- Root user
- IAM user
- Role

**Some key takeaways on Roles:**

- Roles eliminate the need for creating accounts for temporary tasks.
- When a service has a role it need to have adequate permissions to perform work on your behalf. You’ll need to define the role for the service to use. These as known as service linked roles. Service linked roles are commonly used with AWS EC2 and AWS Labmda.
- Roles are useful for when you have an identity provided that allows employees to log into work laptops, rather than making hundreds IAM accounts you could just use IAM roles to grant access to AWS through existing identities from your entity’s user directory (federated users).

**The Root User** :evergreen_tree: 

When you create an AWS account you are the _ROOT USER_

The ROOT User has full access to all AWS resources. It is imperative to secure this account. To secure the account you can do the following:

- Create a strong password
- Enable MFA for root user
- Disable to delete root user access keys
- Create a power IAM user (full access to AWS account except for billing) for administrative tasks


## **IAM POLICIES** :page_with_curl:

Recall that we mentioned that each in your AWS environment each entity must be assigned a policy. An IAM policy is a JSON document with the following features:

- A formal set of statement of permissions granted to an entity.
- Effect when user requests access to resources. 
- What actions are allowed.
- What resources to allow those actions on.
- Can be applied to users, groups and roles.
- Multiple policies can be attached to entities.
- Order in which policies are evaluated has no effect on outcome. Action is either allowed or denied.
- When policies conflict, the most restrictive policy take precedence.

IAM Policy Example:

```
{
    “Version”: “2012-10-17”,
    “Statement”:[{
        “Effect”:”Allow”,
        “Action”:[“DynamoDB:*”,”s3:*”],
        “Resource”:[
            “arn:aws:dynamodb:region:account-number-without- 
            hyphens:table/table-name”,
	    “arn:aws:s3:::bucket-name”,
	    “arn:aws:s3:::bucket-name/*”]
}

```
Lets break down the contents of this policy:

- <u>Effect</u> = required field and specifies whether the statement results in allow of explicit deny. Values here can only be Allow or Deny.

- <u>Action</u> = the specific action(s) to be allowed/denied. Using a wildcard (*) gives access to all the action the specific AWS product offers. Here the user can perform any action on Amazon DynamoDB and Amazon S3 for the listed resources

- <u>Resource</u> = resource elements the object(s) the statement covers. Resources are sacrifice using an Amazon Resource Name (ARN). Even though the actions used a wildcard (*) which gave the user full control, the full control only applies to the ARN listed. If however, the resource element used the wildcard(*), then the user would have access to every resource of the services listed.

While this is a simple example, keep in mind that actual policies you encounter will likely be much more complex. A key to remember is that when there are multiple statements in a policy, an explicit deny will take precedence over an allow.

Example: 
Imagine a identity based policy grants a user ability to list and read objects access to a photos S3 bucket. However, another resources-based policy explicitly denies the same user access to list and read objects to the same S3 bucket. The explicit deny always takes precedence and the user will not be able to access the S3 photos bucket.

Why does this happen? Look below

Rules ordering precedence

**Explicit deny (overrides explicit allow)
Explicit allow (overrides implicit deny)
Implicit deny (default) - when user is created** 

This flowchart is a great visual to help understand the overall outcome of a policy.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4677dhqnozokhc1vb4oc.png)
_Source: msp360.com_

**Two Types of Policies**

<u>I<u>dentity based policies</u></u>: permission based that you can attached to a principal or identity. Determine what the entity can do and under what conditions. Three categories exist for this type of policy.

1. <u>AWS-managed policies</u> - created and manged by AWS. IAM has a library of over 1,000 AWS managed policies for quick use.

2. <u>Customer managed policies</u> - policies that you create. Offer more precise control over you policies than AWS-managed polices. Can be created in visual editor or creating a JSON policy directly.

3. <u>Inline policies</u> - policies that you create and manage which are embedded into a single user, group or role. _Not usually recommended due to high maintenance_.

<u>_Resource based policy_</u>: attached to a resource. These policies specify who can access the resource and what actions can be performed on the resource.

_Note: Resource based policies are defined inline only._


## **Monitoring IAM** 👀

Once IAM has permissions set, you will need to constantly fine tune and tweak permissions in order to keep your resources secure. Fortunately AWS has some tools which can assist in covering gaps.

**IAM Access Analyzer:**

An AWS service to identify unintended access to your AWS resources. It analyzes resource-based policies (such as S3 bucket policies, IAM policies, and KMS key policies) to identify any potential security risks or vulnerabilities in your permissions setup. It also previews how policy affects access to resources and helps review auditing and compliance efforts.

**IAM Access Analyzer Cycle:**

<u>SET</u> - Set or grant detailed permissions: generates detailed policy based on access activity that is captured in your logs. After an application is built, you can generate policies that only grant the required permissions to operate the application. IAM access analyzer has more than 100 policy checks that you can use to create new policies or validate existing policies

<u>VERIFY</u> - Verify who can access what: analyze all access paths and provides a comprehensive analysis of external access to your resources. It continuously monitors for new or updated resource permissions that grant public and cross-account access.

<u>REFINE</u> - Refine by removing overly broad access: using last-accessed information provided information about when AWS services were last used. This helps tighten permissions. You can compare permission that have been granted with when those permission were last access in order to remove unused access and further refine permissions.


**AWS Credential Reports**

These reports list all users and status of their credentials eg. passwords, access keys, and MFA devices. Credential reports include the following details: 

* Access Key Information: The report includes details about each AWS access key associated with your IAM users, such as the key's ID, status (active or inactive), creation date, and last usage date.

* User Information: The report provides information about your IAM users, including their usernames, assigned policies, and whether MFA (multi-factor authentication) is enabled.

* Access Key Age: The report might indicate the age of each access key, helping you identify keys that may need to be rotated for security reasons.

* Usage Patterns: By reviewing last usage dates, you can identify keys that haven't been used in a while, potentially indicating that they can be safely deactivated.

* Can be accessed my management  console and SDK.

* Helps with compliance efforts, effects of credential lifecycle requirements, external audits.


**Cost Effectiveness**
IAM sounds great. However, services in the cloud usually cost money. Well, not IAM..it is free! You only have to pay for services which IAM has access to. To further clarify, you are only charged for billable actions that a user might perform in the cloud eg. provisioning an EC2 instance.

## **Allowing Access** :vertical_traffic_light:

Remember how we discussed that by default, no privileges are assigned by default? Well, now that you want to create a user, we are going to need to know how to give them access to resources. There are two methods of achieving this:

1. AWS Management Console
The user is given a username and password. IAM will let you manage passwords by configuring a password policy. A password policy is a set of rules that define the type of password a user can set.

Some options to include for a password policy:

- password length
- require at least one uppercase letter
- require at least one lowercase letter
- require at least one number
- require at least one non-alphanumeric character
- enable password expiration
- password expiration require administrator reset
- allow users to change their own password
- prevent password re-use

2. Programmatic

- User gets their own set of access keys
- Allows user to make programmatic calls to AWS using CLI, AWS SDK or using direct HTTPS calls using API for individual AWS services.
- Access keys used to digitally sign API calls made to AWS services.
- Each access key is comprised of access key ID and secret key
- Each user can have two active access keys (useful when you need to royal user’s access keys or revoke permissions)

**MFA** :calling:
Multi-factor authentication is another layer that can be used for accessing resources in AWS. Using MFA is using more than one authentication factor before granting access.

**Note**: If MFA is enabled, a user <u>MUST</u> login with their credentials and entering the MFA temporary code. Only if MFA is removed, is the user not required to enter both.

**Interesting Fact**: Some services will have credentials that are unique to operating the service but not required for provisioning the service.

**Example with AWS EC2:**
<u>IAM User	</u>			    <u>Key Pair</u>
Create instance				- Patch instance
Modify instance configuration		- Install software
Terminate instance			- Add/remove files

**Security warning:** :warning: 
AWS EC2 pairs do not provide any tracking as to who used the keys. Therefore, this method is not recommended for daily access.

Continue reading if you want to learn how to monitor access...

## **Monitoring Access**

**AWS IAM Identity Center**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pt6utcj1k10hde4qdr4u.png)

IAM Identity Center provides one place where you can create or connect workforce users and centrally manage their access across all their AWS accounts and applications. You can use multi-account permissions to assign your workforce users access to AWS accounts. AWS IAM Identity Center also allows you securely create or connect your workforce identities.

## **Additional AWS Security Services**

**AWS Cognito**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/whjyml60fmm18pyudaob.png)

Amazon Cognito is a fully managed service that provides authentication and authorization for web and mobile apps. Users can sign in themselves or via a third party identifier service. It is a IAM service. However unlike IAM, Amazon cognito is for mobile/web application users who will interact via a web interface (eg retail or gaming app).

AWS Cognito has two main components:

<u>User pools</u> - directories that provide sign-up and sign-in options for your users. Users can sign in to web/mobile app through Amazon Cognito or federate through a third-party identity provider. All members of the pool have directory profiles that can be accessed via an SDK.

- Sign-up and sign-in services.
- Built in customizable web UI to sign in users.
- Social sign-in with FB, Google and login with Amazon.
- User directory management and user profiles.
- Security features such as MFA.

<u>Identify pools</u> - federated identities that enable creation of unique identities and federate them with identity provides. With a pool, you can obtain temporary. Limited-privilege AWS credentials to access other AWS services.

**How does this work?**

1. User signs in through user pool via web/mobile application.
2. User is successfully authenticated.
3. User receives user pool tokens.
4. The app exchanges user pool tokens for AWS credentials via an identity pool.
5. Finally app user uses those credentials to access other AWS services.

**AWS Key Management Service**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uqimmsmywppua9zjapwt.png)

Amazon Web Services (AWS) Key Management Service (KMS) is a managed service that provides centralized control and management of cryptographic keys used to encrypt data within AWS services and applications. KMS allows you to create, manage, and secure cryptographic keys and control their usage for data encryption and decryption. AWS CloudTrail can be used to provide logs of all key usage. 

There are three different categories of keys:

1. AWS-owned keys - collection of keys an AWS service owns and manages for use with multiple AWS accounts. Drawback if you are required to audit or control encryption key that protects your resources - otherwise its a good choice.

2. AWS-managed keys - keys in your accounts that are created, managed and used on your behalf by and AWS service integrated with AWS KMS. Unless you’re required to control encryption key that protects your resources its a good choice. You don’t have to create or maintain the key or its key policy. There is no monthly fee for an AWS-managed key.

3. Customer-managed keys - keys that you create. You have full control including:

- Establishing and maintaining key policies.
- IAM policies.
- Enabling and disabling keys.
- Rotating their crypographic material.
- Creating aliases that refer to the KMS keys.
- Scheduling the KMS keys for deletion.
- Choose symmetric  or asymmetric key. 

**Defining AWS KMS permissions**

1. Key administrator - one ore more IAM users that manage the key.
2. Key policy - JSON document that contains the permission for what key can be used to do.
3. Key user - user that can use key to perform actions listed in key policy.

**AWS Secrets Manager**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xs93qu1w3ppr87xkc7bs.png)

AWS Secrets Manager is a managed service provided by Amazon Web Services (AWS) that helps you securely store, manage, and retrieve sensitive information such as passwords, API keys, and other credentials. It centralizes the management of secrets and provides a secure and scalable solution for handling sensitive data in your applications and services.

<u>Benefits of Secrets Manager</u>:

- Replace hard coded keys and retrieve them via API call to Secrets manager.
- Secrets Manager can rotate secrets automatically according to a schedule you specify.
- Control access to secrets with IAM policies.
- Audit and monitor your secrets usage.

**AWS Shield**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/7cji8dxwccmiv3w8tuyj.png)

AWS Shield is a managed Distributed Denial of Service (DDoS) protection service provided by Amazon Web Services (AWS). It helps safeguard web applications and websites against malicious attacks that aim to overwhelm the resources and disrupt the availability of your applications.

AWS Shield offers two tiers of protection:

1. AWS Shield Standard: This is automatically included at no extra cost for all AWS customers. AWS Shield Standard provides protection against common, most frequently observed network and transport layer DDoS attacks. It is designed to protect against attacks that target the network and transport layers, such as SYN/ACK floods and DNS query floods.

2. AWS Shield Advanced: This is a subscription-based service that offers enhanced DDoS protection beyond what's available with Shield Standard. Shield Advanced provides protection against more sophisticated and larger-scale attacks that target not only the network and transport layers but also the application layer. It includes features such as:

- <u>Advanced Threat Intelligence</u>: Shield Advanced uses AWS threat intelligence to identify and mitigate DDoS attacks more effectively.
- <u>Real-time Attack Visibility</u>: It provides detailed attack telemetry and reporting, helping you understand and respond to ongoing attacks.
- <u>24/7 DDoS Response Team (DRT)</u>: Shield Advanced customers have access to a team of AWS DDoS experts who can assist during attacks and provide guidance on mitigation strategies.
- <u>Application Layer Protection</u>: Shield Advanced protects against attacks that target the application layer, such as HTTP floods and slow POST attacks.

**AWS Inspector**
Amazon Inspector is an automated security assessment service offered by Amazon Web Services (AWS). It helps you identify vulnerabilities and security issues in your AWS resources and applications by analyzing their configurations and behavior. Amazon Inspector is designed to provide actionable insights and recommendations to improve the security posture of your applications and infrastructure.

AWS Inspector Benefits:

- Auto scans and accesses applications from exposures, vulnerabilities and deviations for best practices.
- Produces a detailed list of security findings that are listed by level of severity.
- Offers quick discovery
- Enables you to prioritize remediation
- Helps you meet compliance regulations

**Amazon GuardDuty**
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/npy96kdsjkzlxq8mxw6a.png)

Amazon GuardDuty is a managed threat detection service provided by Amazon Web Services (AWS). It helps you protect your AWS resources and workloads by continuously monitoring for malicious activity, unauthorized behavior, and potential security threats. GuardDuty uses machine learning, anomaly detection, and integrated threat intelligence to identify and alert you about suspicious or malicious activities in your AWS environment.

<u>Amazon GuardDuty Benefits</u>:

- Security visibility: gain insight of compromised credentials, unusual data access, suspicious logins and API calls from malicious IP addresses.
- Find malware: Scans Amazon Elastic Block Store for files that might have malware creating suspicious behavior on instance and container workloads running on Amazon EC2.
- Route security findings to preferred operations tools using integrations with AWS security Hub and Amazon EventBridge.















