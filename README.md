# Cloud Security Posture Management with AWS Security Hub
My Cloud Security Posture Management (CSPM) project in NT534 - Advanced security network subject at UIT. The final presentation: [here](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/blob/main/final/ATMNC_DoAn_final.pptx).

## Introduction to CSPM
CSPM tools identify, remediate risks by:
 - visibility
 - uninterrupted monitoring
 - threat detection
 - remediation workflows

How? By searching for misconfigs in cloud environments/infrastructure (IaaS, PaaS, SaaS)

## Scope of work
Deploy features of AWS Security Hub related to CSPM, aimining to enhance security in AWS and AWS only by security check for misconfig in AWS resources and remediations for findings.
Focused standards: CIS AWS Foundation Benchmark v1.2.0, PCI DSS v3.2.1, NIST 800-53 R5.

## AWS Security Hub in the project
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/ad694793-4c02-4fef-bbcf-b086bd882488)

### Terminology
- AWS Config rule: an ideal config setting
- Scurity control: a representation of a rule -> in one or more security standards
- Finding: a potential security issue generated after security check
- Playbook: a set of remediation

### Security Control example
**Severity**: High

**AWS Config rule**: restricted-ssh
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/6de87c8c-683a-4613-8fe3-043ec71a15c4)

**Parameters**: None 

This control checks whether an Amazon EC2 security group allows ingress from 0.0.0.0/0 or ::/0 to port 22. The control fails if the security group allows ingress from 0.0.0.0/0 or ::/0 to port 22.
Security groups provide stateful filtering of ingress and egress network traffic to AWS resources. We recommend that no security group allow unrestricted ingress access to port 22. Removing unfettered connectivity to remote console services, such as SSH, reduces a server's exposure to risk.

**Remediation**: 
To prohibit ingress to port 22, remove the rule that allows such access for each security group associated with a VPC.

### Security control list
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/1eb301ef-3226-463f-b0e7-7036587cb691)

### Standalone Findings
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/6ced0d81-b308-48d7-b8ac-fc62a658c64a)

### Standard Security check
For PCI DSS v3.2.1
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/acd8d6e3-5ba7-49a9-b3cc-3be057f5a0c9)
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/9dc4d00b-6aa6-4934-bd11-11440e017edc)
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/174ae4da-6652-4488-bfff-ab7a21628aa0)

### Demo detect virus in EC2 with GuardDuty
In this part, I use the "malware scan for EC2" feature of GuardDuty to scan an EC2 instance after setup many virus in that EC2 instance. Then observer the GuardDuty report and AWS Security Hub relative report.
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/74c783cd-8ae7-4ec2-8ad5-abe1ef2b0300)

#### GuardDuty report
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/2b471a25-41db-4634-bdd0-c5f7f64bbaca)

#### Security Hub aggregates finding from GuardDuy
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/1af955b1-06a2-4413-a443-456452c1af65)
##### Console
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/43272332-5b7e-4824-b0db-8ea58b643e1c)
##### Json report
The detailed in [`securittyHub_detectVirus.json`](securittyHub_detectVirus.json), which list the detail of many detected virus.

## Automated Security Response on AWS
Refer to: https://github.com/aws-solutions/automated-security-response-on-aws
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/4f7d120c-b2b1-4d4d-817b-ed6035206bdb)

### Terminology
- Remediation runbook: An implementation of a set of steps that resolves a finding.
- Control runbook: SSM automation documents that Orchestrator uses to route an initiated remediation for a specific control to the correct remediation runbook.
- Playbook: a set of remediation

### Demo
There are 5 demos in this project. However, for shortly, only the first demo has the description about runbooks. All demos listed in: https://www.youtube.com/playlist?list=PL7IdJecfX87jHfO43NYd6MXL8mBYWBAIf
#### Prequesite
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/9c0f3edc-805b-40f7-b1fc-85c723a9eab0)

#### Orchestrator workflow
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/929db750-3807-4da9-af94-4f7bcf0a9dde)
#### Demo 1: Security groups should not allow ingress from 0.0.0.0/0 to port 22 – in member
Input: Member creates a security group that opens port 22 for all IPv4
=> violate control EC2.13

Acion: Monitor & remediate

Output: 
- Findings generated in admin-member AWS Security Hub
- Remediation status notification to mail 
- Successfull automated remediation

##### Control Runbook EC2.13
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/9bdfabbc-1838-40e0-8ffd-9a21742e1468)

**What does this document do?**

Removes public access to remove server administrative ports from an EC2 Security Group.

**Input parameters**

- Finding: Security Hub finding details JSON 
- AutomationAssumeRole: ARN of the role allows automation to perform on your behalf.

**Output parameters**

Remediation.Output: Output of AWS-DisablePublicAccessForSecurityGroup rubook.

###### Step 1: ParseInput
Run a python script and eventually extract:
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/771555d3-2bdf-4409-aa19-fa0569b97ba4)

###### Step 2: Remediation
Receive the GroupID and the AssumeRole ARN as inputs, then execute a remediation runbook name "AWS-DisablePublicAccessForSecurityGroup"
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/641c5c83-f836-46e0-b25c-46d7fbc2b327)

##### Remediation runbook:
Receive Security Group ID and IP permissions as inputs
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/a4c197a6-9925-462a-b8e7-057f7e47c25d)

![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/7bbc2023-d095-46cf-bb9d-fa18b7007aa7)
Then call the EC2 API RevokeSecurityGroupIngress:
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/6759f7c5-ee19-41f3-8300-ba3d44d816f8)

#### Demo 2: Ensure IAM password policy requires at least one number
Input: Member configs weak IAM password policies 
=> Fail control IAM.14

#### Demo 3: RDS DB clusters should be configured for multiple AZs
Input: Admin has a RDS DB instance in only one Availablity Zone.
=> Fail control RDS.5

#### Demo 4: EBS default encryption should be enabled
Input: Member has an unencrypted EBS volume.
=> Fail control EC2.7

#### Demo 5: S3 general purpose buckets should have block public access settings enabled
Input: Member has a S3 bucket with public acces.
=> Fail control S3.1





