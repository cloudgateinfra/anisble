# Ansible Tower

## Infrastructure Management

This section showcases Ansible's capabilities in automating infrastructure provisioning tasks in AWS EC2 instances. It includes:

- Creating AWS EC2 instances with:
  - Configuration of key pairs
  - Setting up security groups with defined rules
  - Launching instances with specified tags
  - Adding new EC2 instances to a dynamic inventory

## CI/CD Pipelines

Demonstrates Ansible's integration with CI/CD pipelines for automated deployment of web applications. It involves:

- Cloning the latest code from a Git repository
- Creating a virtual environment and installing dependencies
- Running database migrations if needed
- Restarting the Apache server for changes to take effect

## Security Automation

Automates critical security updates on all hosts. Tasks include:

- Updating the APT package cache
- Upgrading all packages with distribution-level upgrades
- Cleaning up unnecessary packages
- Checking for system reboots if required

## Configuration Management

Illustrates how Ansible excels in configuration management, specifically configuring Nginx web servers on target servers. This includes:

- Installing Nginx
- Copying optimized Nginx configurations from templates
- Reloading the Nginx service for changes to apply

## Monitoring and Logging Integration

Showcases Ansible's integration with monitoring solutions, deploying Prometheus and Grafana on monitoring servers. This involves:

- Installing Docker
- Pulling and running Prometheus and Grafana containers with specified ports
- Setting up necessary configurations for harmonious interaction

## Infrastructure as Code (IaC)

Reflects Ansible's compatibility with Infrastructure as Code (IaC) principles by provisioning AWS infrastructure using Terraform. This includes:

- Initializing Terraform in the specified directory
- Applying the Terraform plan with auto-approval
- Fetching outputs post-provision for further tasks

# Ansible Tasks

## aws-dns-record-creation.yml

This Ansible playbook automates the creation of DNS records in Amazon Route 53. It creates A and CNAME DNS records based on user-defined input. The playbook includes:

- Variable prompts for user input, including server name, A record value, and CNAME record name.
- Task to ensure the required Python modules (`boto3` and `botocore`) are installed.
- Tasks to create DNS A and CNAME records in Route 53.
- An optional task to display the results of the DNS record creation.

To use this playbook, run it with the `ansible-playbook` command and provide the necessary AWS access and secret keys as variables. Replace the zone, record, and value parameters with appropriate values for your use case.

## awscli.yml

This Ansible playbook automates the setup of the AWS Command Line Interface (CLI) on a target system. It includes tasks to:

- Include AWS credentials from an external file.
- Install required Python packages.
- Install the AWS CLI using pip.
- Update PATH environment variables.
- Create an AWS configuration folder.
- Add AWS CLI credentials to the configuration file.

Before running this playbook, ensure Ansible is installed on your local machine and AWS access and secret keys are stored in the provided external file.

## crontab.yml

This Ansible playbook modifies the cron schedule for daily, weekly, and monthly tasks by adjusting the hour at which these tasks are executed. It backs up the current crontab, sets the specified hours for these tasks, and applies the changes based on the `cron_daily_weekly_monthly_hour` variable defined in your inventory files.

To use this playbook, ensure Ansible is installed, customize the playbook as needed, and define the `cron_daily_weekly_monthly_hour` environment variable in your inventory files before running it.

## datadog.yml

This Ansible playbook configures Datadog monitoring for a specific application environment and server. It includes tasks to:

- Include Datadog configuration variables from an external file.
- Create a system probe configuration file.
- Start the Datadog system probe service.
- Change log directory permissions for Apache log monitoring.
- Restart the Datadog agent to apply configuration changes.

Before running this playbook, ensure Ansible is installed, customize the playbook with Datadog configuration settings, and run it with the desired `app_env` and `server_name` variables.

## disable-ipv6.yml

This Ansible playbook disables IPv6 configurations on Linode and Vultr servers. It includes tasks to:

- Remove invalid TCP settings added by Vultr on applicable servers.
- Disable IPv6 by modifying sysctl settings for Linode and Vultr servers.

To use this playbook, ensure Ansible is installed, customize it as needed, and run it with the `ansible-playbook` command.

## firewall.yml

This Ansible playbook configures the Uncomplicated Firewall (UFW) on Linode servers. It includes tasks to:

- Configure UFW for HTTP and HTTPS traffic.
- Configure UFW for SSH on port 22, allowing traffic only from specific source IP addresses.
- Configure UFW for SSH on port 9001, following similar source IP restrictions.
- Enable UFW with default deny policies for incoming traffic.

Before using this playbook, ensure Ansible is installed and customize it as required, especially the list of allowed IP addresses in the SSH rules.

## packages.yml

This Ansible task installs essential packages on the target system. Packages include various utilities and tools useful for system administration and development.

## percona-toolkit-install.yml

This Ansible task installs Percona Toolkit, a collection of command-line tools for MySQL and MongoDB database administrators. It uses the `apt` module to ensure the package is installed.

## python.yml

This Ansible task ensures Python is installed on the target system using the `raw` module. It checks for Python's presence, updates the package cache, and installs Python if needed.

## s3cmd.yml

This Ansible playbook includes tasks for installing and configuring `s3cmd`, a command-line tool for interacting with Amazon S3 and other cloud storage services. It installs `s3cmd` using the `apt` module and configures it using a template.
