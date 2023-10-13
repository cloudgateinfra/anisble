### Ansible Playbooks

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
