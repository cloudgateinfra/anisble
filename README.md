### Ansible Tower

1. **Infrastructure Management**:
   - Creates an AWS EC2 instance using Ansible. This includes:
     * Configuring key pairs
     * Setting up security groups with defined rules
     * Launching the instance with specified tags
     * Adding the new EC2 instance to a dynamic inventory
   This demonstrates how Ansible can be used to automate infrastructure provisioning tasks in the cloud.

2. **CI/CD Pipelines**:
   - Automates the deployment of a web application. This includes:
     * Cloning the latest code from a Git repository
     * Creating a virtual environment and installing dependencies
     * Running database migrations (if necessary)
     * Restarting the Apache server for changes to take effect
   This highlights Ansible's ability to integrate with CI/CD tools and streamline the software delivery process.

3. **Security Automation**:
   - Automates security updates on all hosts. This includes:
     * Updating the APT package cache
     * Upgrading all packages with distribution-level upgrade
     * Cleaning up unnecessary packages
     * Checking for system reboots if required
   This demonstrates Ansible's capabilities in automating critical security-related tasks to ensure systems are always up-to-date.

4. **Configuration Management**:
   - Configures an Nginx web server on target servers. This includes:
     * Installing Nginx
     * Copying optimized Nginx configurations from templates
     * Reloading the Nginx service for changes to apply
   This is an example of how Ansible excels in configuration management, ensuring systems are configured consistently and correctly across the board.

5. **Monitoring and Logging Integration**:
   - Deploys Prometheus and Grafana on monitoring servers. This includes:
     * Installing Docker
     * Pulling and running Prometheus and Grafana containers with specified ports
     * Setting up necessary configurations for harmonious interaction
   This showcases how Ansible can be integrated with monitoring solutions to provide visibility into infrastructure health and performance.

6. **Infrastructure as Code (IaC)**:
   - Provisions AWS infrastructure using Terraform. This includes:
     * Initializing Terraform in the specified directory
     * Applying the Terraform plan with auto-approval
     * Fetching outputs post provision for further tasks
   This reflects Ansible's compatibility with IaC tools and principles.

### Ansible Tasks

- **aws-dns-record-creation.yml**
  The provided Ansible playbook (`aws-dns-record-creation.yml`) is used to create DNS records in Amazon Route 53, a scalable and highly available cloud Domain Name System (DNS) web service provided by AWS. This playbook automates the process of creating A and CNAME DNS records. Here's a breakdown of what each part of the playbook does:

1. **Variable Prompts**: This section defines variables that will be prompted for when the playbook is run. These variables include:
   - `server_name`: The name for the A record (e.g., `chicago-ec2-01b`).
   - `server_a_record_value`: The IP address for the A record (e.g., `111.11.11.111`).
   - `monitor_name`: The name for the CNAME record (e.g., `testmon`).

   Users will be prompted to input values for these variables when running the playbook.

2. **Ensure Boto and Boto3 Modules**: This task ensures that the required Python modules (`boto3` and `botocore`) are installed. These modules are necessary for interacting with AWS services, including Route 53.
3. **Create DNS A Record**: This task uses the `community.aws.route53` module to create an A record in Route 53. It specifies various parameters, including AWS access key and secret key, the zone (domain) where the record should be created, the name and type of the record (A in this case), time to live (TTL), and the IP address (value). The `wait` parameter is set to "yes," which means the playbook will wait for the record to be created before continuing.
4. **Create DNS CNAME Record**: Similar to the previous task, this one creates a CNAME record in Route 53. It specifies the same parameters but for a CNAME record type.
5. (Optional) **Display Results**: This task is commented out, but if uncommented, it would display the results of the DNS record creation.

To use this playbook, you would run it using the `ansible-playbook` command, and it will interact with AWS Route 53 to create the specified DNS records. Make sure to provide the necessary AWS access key and secret key as variables (`amazon_route53_ansible_access_key_id` and `amazon_route53_ansible_secret_access_key`) when running the playbook. Additionally, replace the `zone`, `record`, and `value` parameters with the appropriate values for your specific use case.

Please note that this playbook assumes you have Ansible and the required AWS libraries installed, and it's configured to work with AWS Route 53. Always exercise caution when dealing with AWS resources and sensitive credentials.


- **awscli.yml**
sible playbook automates the setup of the AWS Command Line Interface (CLI) on a target system. It performs several tasks to ensure that the AWS CLI is installed and configured correctly:

1. **Include AWS Credentials**: The playbook includes AWS credentials from an external file (`vars/passwords/aws_keys.yml`). This file should contain your AWS access and secret keys.
2. **Install Required Python Packages**: It ensures that the necessary Python packages, including `python-setuptools` and `python-pip`, are installed on the target system.
3. **Install AWS CLI**: The playbook uses pip to install the AWS CLI.
4. **Update PATH**: It updates the user's `.profile` file to add the local bin directory and AWS CLI bin directory to the system PATH, making the AWS CLI commands easily accessible.
5. **Create AWS Configuration Folder**: It creates the `.aws` directory in the user's home directory to store AWS CLI configuration files.
6. **Add AWS CLI Credentials**: The playbook adds AWS CLI configuration to the user's AWS configuration file (`~/.aws/config`), including access and secret keys.

Prerequisites:

Before running this playbook, ensure that you have:
- Ansible installed on your local machine.
- AWS access and secret keys stored in `vars/passwords/aws_keys.yml`.

Usage:

1. Edit the `vars/passwords/aws_keys.yml` file and add your AWS access and secret keys.
2. Customize the playbook as needed for your specific use case.
3. Run the playbook using the `ansible-playbook` command:

   ```shell
   ansible-playbook aws-cli-setup.yml


- **crontab.yml**
This Ansible playbook is designed to make changes to the cron schedule for daily, weekly, and monthly tasks by modifying the hour at which these tasks are executed. The hour is configured based on the value set in the environment variable `cron_daily_weekly_monthly_hour` defined in your inventory files.

- **Backup Current Crontab**: The playbook starts by creating a backup of the current `/etc/crontab` file in the `/var/tmp` directory. This backup includes a timestamp to differentiate it from other backups.
- **Set Cron Daily Hour**: It then sets the hour for daily tasks (`cron.daily`) in the `/etc/crontab` file, using the value specified in the `cron_daily_weekly_monthly_hour` variable. The tasks will run every day at the specified hour.
- **Set Cron Weekly Hour**: Similarly, the playbook sets the hour for weekly tasks (`cron.weekly`) in the `/etc/crontab` file, ensuring that they run at the specified hour every Sunday.
- **Set Cron Monthly Hour**: Lastly, the playbook configures the hour for monthly tasks (`cron.monthly`) in the `/etc/crontab` file. These tasks will execute at the specified hour on the first day of every month.

Usage:

1. Make sure you have Ansible installed on your local machine.
2. Customize the playbook as needed.
3. Ensure that you have an environment variable named `cron_daily_weekly_monthly_hour` defined in your inventory files with the desired hour value.
4. Run the playbook using the `ansible-playbook` command:

   ```shell
   ansible-playbook cron-daily-weekly-monthly-setup.yml


- **datadog.yml**
This Ansible playbook is designed to configure Datadog monitoring for a specific application environment (`app_env`) and server (`server_name`). It performs the following tasks to set up Datadog monitoring:

- **Include Datadog Configuration**: The playbook starts by including Datadog configuration variables from an external file (`vars/configs/datadog/datadog_{{ app_env }}_{{ server_name }}.yml`). This file should contain your Datadog configuration settings tailored to the specified application environment and server.
- **Create Sysprobe Configuration**: The playbook uses the `blockinfile` module to create a system probe configuration file (`/etc/datadog-agent/system-probe.yaml`). This file contains configuration settings for the Datadog system probe. The configuration is based on the `datadog_sysprobe_config` variable.
- **Start Sysprobe Service**: It ensures that the Datadog system probe service (`datadog-agent-sysprobe`) is started. Monitoring with the system probe allows Datadog to collect additional system-level metrics.
- **Change Log Directory Permissions**: This task uses the `setfacl` command to modify access control lists (ACLs) for the Apache log directory (`/var/log/apache2/`). It grants read and execute permissions to the Datadog agent (`dd-agent`) for log monitoring purposes.
- **Restart Datadog Agent**: Finally, the playbook restarts the Datadog agent (`datadog-agent`) to apply the new configuration settings.

Usage:

1. Ensure that you have Ansible installed on your local machine.
2. Customize the playbook as needed, especially the Datadog configuration settings in the external configuration file (`datadog_{{ app_env }}_{{ server_name }}.yml`).
3. Run the playbook using the `ansible-playbook` command, passing the desired `app_env` and `server_name` as variables:

   ```shell
   ansible-playbook datadog-configuration.yml -e "app_env=your_app_env server_name=your_server_name"


- **disable-ivp6.yml**
This Ansible playbook is designed to disable IPv6 configurations on Linode and Vultr servers. It performs the following tasks to disable IPv6:

- **Remove Invalid TCP Settings (Vultr)**: The playbook checks if the server hostname contains "vlt" (indicating a Vultr server) and removes the invalid `net.ipv4.tcp_rmem` and `net.ipv4.tcp_wmem` settings added by Vultr's Ubuntu 18.04 ISO installation. These settings are removed to ensure proper network configurations.
- **Disable IPv6**: The playbook disables IPv6 by modifying the sysctl settings for Linode and Vultr servers. It sets the values of `net.ipv6.conf.all.disable_ipv6`, `net.ipv6.conf.default.disable_ipv6`, and `net.ipv6.conf.lo.disable_ipv6` to '1' to disable IPv6. The `sysctl` module is used to apply these changes.

Usage:

1. Ensure that you have Ansible installed on your local machine.
2. Customize the playbook as needed.
3. Run the playbook using the `ansible-playbook` command:

   ```shell
   ansible-playbook disable-ipv6.yml


- **firewall.yml**

This Ansible playbook is designed to configure the Uncomplicated Firewall (UFW) on Linode servers. It sets up firewall rules to allow specific incoming traffic to the server. The playbook includes the following tasks:

- **Configure Firewall for HTTP and HTTPS**: This task configures UFW rules to allow incoming traffic on ports 80 (HTTP) and 443 (HTTPS). It enables web server access.
- **Configure Firewall for SSH Port 22**: This task allows incoming SSH traffic on port 22, but only from specific source IP addresses. It whitelists a set of trusted IP addresses for SSH access.
- **Configure Firewall for SSH Port 9001**: Similar to the previous task, this task allows incoming SSH traffic, but on port 9001, and only from specific source IP addresses. It provides an additional SSH access point.
- **Enable UFW**: This task enables UFW, sets the default policy to deny incoming traffic, and configures incoming traffic direction.

Usage:

1. Ensure that you have Ansible installed on your local machine.
2. Customize the playbook as needed, especially the list of allowed IP addresses in the SSH rules.
3. Run the playbook using the `ansible-playbook` command:

   ```shell
   ansible-playbook configure-ufw-firewall.yml

- **packages.yml**

This Ansible task is part of a larger playbook and is responsible for installing a set of essential packages on the target system. The task utilizes the `apt` module to ensure that these packages are installed and available for use.

- **Install Main Packages**: This task uses the `apt` module to install the following essential packages on the target system:

   - `vim`: A popular text editor.
   - `curl`: A command-line tool for transferring data with URLs.
   - `less`: A pager program for viewing text files.
   - `git`: A distributed version control system.
   - `make`: A build automation tool.
   - `bash-completion`: Bash shell tab-completion support.
   - `tmux`: A terminal multiplexer for managing multiple terminal sessions.
   - `htop`: An interactive process viewer.
   - `atop`: An advanced system and process monitor.
   - `ncdu`: A disk usage analyzer with an interactive interface.
   - `iotop`: A utility for monitoring I/O usage of processes.
   - `dstat`: A versatile resource statistics tool.
   - `fail2ban`: An intrusion prevention framework.
   - `imagemagick`: A software suite for creating, editing, and composing bitmap images.
   - `sysstat`: A collection of performance monitoring tools.
   - `ranger`: A console-based file manager.

The task ensures that these packages are in a "present" state on the target system.

- **percona-toolkit-install.yml**
This Ansible task is responsible for installing Percona Toolkit on the target system using the `apt` module. Percona Toolkit is a collection of advanced command-line tools for MySQL and MongoDB database administrators.

- **Install Percona Toolkit**: This task uses the `apt` module to install Percona Toolkit (`percona-toolkit`) on the target system. It specifies the package name (`pkg`) and ensures that it is in a "present" state. Additionally, it updates the package cache to ensure that the latest version is installed.
- **Uninstall Percona Toolkit and Dependencies**: The commented-out lines below the installation task show how to uninstall Percona Toolkit and its associated packages, including `libdbd-mysql-perl`, `libdbi-perl`, and `libterm-readkey-perl`, and then perform an autoremove operation to clean up any remaining dependencies. These lines are not part of the installation task but are provided for reference.

- **python.yml**
This Ansible task is responsible for ensuring that Python is installed on the target system. Python is a widely used programming language and is often required for various system tasks and Ansible automation.

- **Install Python**: This task uses the `raw` module to check if Python (`/usr/bin/python`) is already installed on the target system. If Python is not found, it updates the package cache (`apt -qqy update`) and installs Python using `apt` (`apt install -y python`). The task registers the output (`output`) to determine if any changes were made.
- **Change Detection**: The `changed_when` attribute is set to `output.stdout != ""` to indicate that the task is considered changed (i.e., executed) if the `output` contains any stdout. This ensures that the task runs only when Python needs to be installed.

- **s3cmd.yml**
This Ansible playbook includes tasks for installing and configuring `s3cmd`, a command-line tool for interacting with Amazon S3 and other cloud storage services.

- **Install s3cmd**: This task uses the `apt` module to install `s3cmd` on the target system. It specifies the package name (`pkg`) as `s3cmd` and ensures that it is in a "present" state. If `s3cmd` already exists, the task will not make any changes.
- **Configure s3cmd**: The second task uses the `template` module to configure `s3cmd`. It copies the source configuration file (`s3/s3cfg`) to the user's home directory (`~/.s3cfg`). This configuration file is used to store settings and credentials for `s3cmd`.

