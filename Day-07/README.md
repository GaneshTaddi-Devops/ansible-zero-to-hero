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

The segment from (31:01 - 34:20) focuses on setting up passwordless authentication for the previously provisioned EC2 instances. This is a crucial prerequisite for subsequent configuration management tasks.

Key Steps for Passwordless Authentication:
Recommended Method: The instructor demonstrates using ssh-copy-id (31:36), which is considered the most popular and straightforward approach for managing SSH keys.

Command Execution: The command used is ssh-copy-id -f -i @ (31:49).
The -f flag is used to force the copying of the identity file.
The user must provide the specific .pem file (e.g., ABI_AWS_key.pem) generated during instance creation.

Targeting Instances:
For the Amazon Linux instance, the connection is established using ec2-user at the instance's IP address (32:45).
For the Ubuntu instances, the connection is established using the ubuntu username (33:32).

Verification: Once the keys are copied, the instructor verifies the setup by connecting to the instances via SSH without needing the .pem file or a password (33:10, 33:46).

Scaling for Production:
The instructor notes that this manual process can be scaled for large numbers of instances by using looping constructs (such as a for loop in bash) or by utilizing SSH agents to simplify authentication management (33:57 - 34:13).

Dynamic Variables: To ensure Ansible creates three distinct resources, the narrator updates the Loop syntax. Instead of passing only an AMI ID, the loop is modified to accept a dictionary containing both the image ID and a unique name for each instance.

Implementation: By updating the name field to use item.name (27:22), each iteration of the loop creates a unique identity for the virtual machine, effectively bypassing the idempotency check that was causing the failure.

Refined Task: The narrator refines the Playbook to include these variables. After clearing the existing infrastructure (terminating the running instances), the new script is executed (28:16).

Verification: By inspecting the AWS EC2 dashboard (29:35), the narrator confirms that all three instances (Manage Node 1, Manage Node 2, and Manage Node 3) are successfully provisioned. The video notes an additional troubleshooting step: using the tags field within the Ansible task to ensure the AWS console properly reflects the specific names assigned via the loop (30:02).

####  The final segment of the video (34:21 - 47:27) focuses on automating the shutdown process for specific EC2 instances using Ansible. This task demonstrates the power of conditional logic in infrastructure management.

Automating Shutdown with Conditionals:

Inventory Setup (35:30): An inventory file is created to manage the three EC2 instances, allowing Ansible to target the specific nodes.

The Playbook Strategy (36:13): The goal is to shut down only the Ubuntu instances while leaving the Amazon Linux instance running. Since the script runs on all nodes, a conditional check is required to differentiate the operating systems.

Using 'When' Conditions (38:59): The instructor employs the when keyword to apply logic. The initial attempt used ansible_facts.os_family == 'Debian', but the instructor later identifies the correct variable to be ansible_os_family (43:08).

Gathering Facts (42:13): The instructor demonstrates the use of the debug module to inspect gathered facts from the managed nodes. This is a critical debugging practice to verify the correct variable names (e.g., os_family) provided by Ansible during the initial fact-gathering phase.

Execution and Verification (44:43): By running the updated playbook with the correct variable, Ansible successfully identifies the Ubuntu (Debian-based) instances and executes the shutdown command, while skipping the Amazon Linux (Red Hat-based) instance.

Key Takeaways for Automation:

Idempotency: The process respects the state of the machines, ensuring that the shutdown command is only applied where the condition is met.

Graceful Handling: The shutdown command includes a timing element, and the instructor notes that Elastic IP addresses are recommended for production environments to maintain connectivity after reboots (46:44).

Conditional Logic: The ability to target subsets of infrastructure based on system-level facts is essential for scalable configuration management.
########################################################################################################




