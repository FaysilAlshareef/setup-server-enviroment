# Ansible Server Setup

This project provides an automated setup for server environments using Ansible. It handles the configuration of both development and production servers with essential services including Docker and PostgreSQL running in containers.

## Project Structure

The repository is organized as follows:

- **[playbooks/](playbooks/)**: Contains the playbooks for server provisioning
  - [main.yml](playbooks/main.yml): Master playbook that orchestrates the entire setup process
  - [base-setup.yml](playbooks/base-setup.yml): Configures basic server settings through the common role
  - [docker-setup.yml](playbooks/docker-setup.yml): Handles Docker installation and configuration
  - [postgresql-setup.yml](playbooks/postgresql-setup.yml): Deploys PostgreSQL as a Docker container

- **[roles/](roles/)**: Modular roles that handle specific aspects of configuration
  - **[common/](roles/common/)**: Base server configuration
    - [tasks/main.yml](roles/common/tasks/main.yml): Updates packages, configures firewall, creates deploy user
    - [handlers/main.yml](roles/common/handlers/main.yml): Service restart handlers
    - [defaults/main.yml](roles/common/defaults/main.yml): Default variables for common configuration
  
  - **[docker/](roles/docker/)**: Docker installation and setup
    - [tasks/main.yml](roles/docker/tasks/main.yml): Installs Docker Engine, enables the service, adds user to docker group
    - [handlers/main.yml](roles/docker/handlers/main.yml): Docker service restart handlers
  
  - **[postgresql/](roles/postgresql/)**: PostgreSQL container deployment
    - [tasks/main.yml](roles/postgresql/tasks/main.yml): Pulls PostgreSQL image, creates volumes and network, runs container
    - [handlers/main.yml](roles/postgresql/handlers/main.yml): Container restart handlers
    - [defaults/main.yml](roles/postgresql/defaults/main.yml): Default PostgreSQL configuration variables

- **[inventory/](inventory/)**: Environment-specific configurations
  - [hosts](inventory/hosts): Server inventory organized by development and production
  - **[group_vars/](inventory/group_vars/)**: Environment-level variables
    - [all.yml](inventory/group_vars/all.yml): Variables applied to all environments
    - [development.yml](inventory/group_vars/development.yml): Development-specific variables
    - [production.yml](inventory/group_vars/production.yml): Production-specific variables
  - **[host_vars/](inventory/host_vars/)**: Host-specific variables
    - [dev-server.yml](inventory/host_vars/dev-server.yml): Configuration for development server
    - [prod-server.yml](inventory/host_vars/prod-server.yml): Configuration for production server

- [ansible.cfg](ansible.cfg): Ansible configuration settings defining inventory location, SSH options, and privilege escalation

## Setup Instructions

1. **Clone the Repository**: 
   ```bash
   git clone <repository-url>
   cd ansible
   ```

2. **Update Inventory**: Modify the `inventory/hosts` file to include your server details.

3. **Configure Variables**: Update the `inventory/group_vars/development.yml` and `inventory/group_vars/production.yml` files with environment-specific variables.

4. **Run the Playbook**: Execute the main playbook to set up the servers.
   ```bash
   ansible-playbook playbooks/main.yml -i inventory/hosts
   ```

## Usage Guidelines

- Ensure that Ansible is installed on your local machine.
- Modify the playbooks and roles as necessary to fit your specific requirements.
- The default user is `ubuntu` as specified in `inventory/group_vars/all.yml`
- Use environment-specific options when needed:
  ```
  ansible-playbook playbooks/main.yml --limit development
  ```
