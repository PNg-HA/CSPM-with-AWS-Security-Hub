# Cloud Security Posture Management with AWS Security Hub
My Cloud Security Posture Management (CSPM) project in NT534 - Advanced security network subject at UIT.

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

## Demo detect virus in EC2 with GuardDuty
In this part, I use the "malware scan for EC2" feature of GuardDuty to scan an EC2 instance after setup many virus in that EC2 instance. Then observer the GuardDuty report and AWS Security Hub relative report.
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/74c783cd-8ae7-4ec2-8ad5-abe1ef2b0300)


## Automated Security Response on AWS
Refer to: https://github.com/aws-solutions/automated-security-response-on-aws
![image](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub/assets/93396414/4f7d120c-b2b1-4d4d-817b-ed6035206bdb)


