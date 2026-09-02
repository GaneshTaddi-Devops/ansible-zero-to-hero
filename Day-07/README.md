# Ansible Realtime project

## Task 1

Create three(3) EC2 instances on AWS using Ansible loops
- 2 Instances with Ubuntu Distribution
- 1 Instance with Centos Distribution

Hint: Use `connection: local` on Ansible Control node.

## Task 2

Set up passwordless authentication between Ansible control node and newly created 
instances.

## Task 3

Automate the shutdown of Ubuntu Instances only using Ansible Conditionals

Hint: Use `when` condition on ansible `gather_facts`


========================================= Youtube notes =====================================


This video is part 7 of a 14-episode Ansible Zero to Hero series. In this session, the focus is on a real-world project assignment involving provisioning and configuration management using Ansible.

Project Overview and Objectives (0:07 - 7:54)
The assignment consists of three primary tasks:

Provisioning: Create three AWS EC2 instances (one Amazon Linux and two Ubuntu) using Ansible loops.
Authentication: Set up passwordless SSH authentication between the control node (local machine) and the managed EC2 nodes.

Configuration Management: Automate the shutdown of only the Ubuntu instances using Ansible conditionals.

Prerequisites and Setup (7:57 - 15:02)
To interact with the AWS API, the narrator sets up an IAM user with EC2 Full Access permissions and retrieves the necessary access keys.

VS Code Extensions: The narrator recommends installing the YAML extension by Red Hat and the Ansible extension for better syntax support.

Tools: It is required to have boto3 installed to allow Ansible to communicate with AWS services.

Security: Sensitive credentials (access keys) are protected using Ansible Vault. The narrator demonstrates creating a base64 encoded file to encrypt a password.yaml file to ensure the API credentials are not exposed in plain text.

Task 1: Provisioning EC2 Instances (15:05 - 24:00)
The narrator creates a playbook named ec2_create.yaml to provision the resources.

Local Execution: Since the playbook is provisioning cloud resources rather than configuring an existing server, the host is set to localhost and the connection type is set to local (15:24).
Looping: To avoid manual repetition, the Ansible loop module is used. The narrator initially encounters a limitation where only two instances are created due to Ansible's Idempotency (24:00). Because Ansible checks the state of the target, if the task definition (including the image ID and name) remains identical for subsequent loop iterations, it recognizes the infrastructure as already existing and skips redundant creation. To solve this, unique tags are needed for each instance in the loop.







