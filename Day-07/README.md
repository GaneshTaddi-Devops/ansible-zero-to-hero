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

Following the discovery that only two instances were created, the narrator explains the core concept of Ansible Idempotency (24:43) and demonstrates how to resolve the provisioning issue.

Understanding Ansible Idempotency (24:01 - 26:35)

Definition: Idempotency ensures that an operation produces the same result regardless of how many times it is executed. If a task reaches the desired state (e.g., a file exists or a server is running), Ansible skips re-running the command to avoid errors or duplicate resources.

The Problem: In the previous iteration, Ansible saw that the instance definition—the AMI ID and configuration—was identical for the first two loop items. Because the first instance already existed, Ansible treated the second identical request as a redundant task and ignored it, resulting in only two instances instead of three.

Comparison: Unlike traditional Shell scripting, which might fail or create duplicate resources upon a second execution, Ansible intentionally avoids performing redundant actions if the target state is already met.

Resolving the Loop Issue (26:37 - 31:00)

Dynamic Variables: To ensure Ansible creates three distinct resources, the narrator updates the Loop syntax. Instead of passing only an AMI ID, the loop is modified to accept a dictionary containing both the image ID and a unique name for each instance.

Implementation: By updating the name field to use item.name (27:22), each iteration of the loop creates a unique identity for the virtual machine, effectively bypassing the idempotency check that was causing the failure.

Refined Task: The narrator refines the Playbook to include these variables. After clearing the existing infrastructure (terminating the running instances), the new script is executed (28:16).

Verification: By inspecting the AWS EC2 dashboard (29:35), the narrator confirms that all three instances (Manage Node 1, Manage Node 2, and Manage Node 3) are successfully provisioned. The video notes an additional troubleshooting step: using the tags field within the Ansible task to ensure the AWS console properly reflects the specific names assigned via the loop (30:02).







