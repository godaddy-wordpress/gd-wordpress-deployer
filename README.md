# GoDaddy GitHub Action for Managed WordPress Deployment

## Overview

This GitHub Action automates the deployment of your WordPress site by leveraging `rsync` over SSH. It packages only modified files into a tar archive, transfers them to your remote server, and executes a tailored deployment script. In addition, the action supports:
- Post-deployment commands
- WordPress health checks with automatic rollback upon failure
- Secure authentication using an SSH private key

## Features

- **Deploy Only Changed Files:** Uses `rsync --checksum` to efficiently update only the modified files.
- **Sync File Deletions:** Automatically removes files deleted from the repository on the server.
- **WordPress Health Checks:** Monitor site health and trigger a rollback on failure.
- **Secure SSH Authentication:** Ensures connectivity using your SSH private key.

## Usage

### 1. Add the Action to Your Workflow

Create a `.github/workflows/deploy.yml` file in your repository with the following content:

```yaml
name: Deploy WordPress

on:
  workflow_dispatch:
    inputs:
      deployment_dest:
        description: 'Target server directory; leave blank for the root directory'
        required: false
      enable_health_check:
        description: 'Enable WordPress health check?'
        type: choice
        required: false
        default: "yes"
        options:
          - "yes"
          - "no"

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Deploy via GoDaddy GitHub Action
        uses: godaddy-wordpress/git-deploy-poc@v0.0.1
        with:
          remote_host: 'http://your-wordpress-site.com'
          ssh_user: 'SSH_USERNAME_FROM_GODADDY_INTERFACE'
          ssh_private_key: ${{ secrets.PRIVATE_KEY }}
          deployment_dest: ${{ github.event.inputs.deployment_dest }}
          enable_health_check: ${{ github.event.inputs.enable_health_check }}
```

### 2. Configuration Inputs

| Name                    | Description                          | Required | Default |
| ----------------------- | ------------------------------------ | -------- | ------ |
| `remote_host`           | The remote server IP or domain       | ✅ Yes    | -      |
| `ssh_user`              | SSH username for authentication      | ✅ Yes    | -      |
| `ssh_private_key`       | SSH private key for authentication   | ✅ Yes    | -      |
| `deployment_dest`       | Remote WordPress directory           | ❌ No     | `''`     |
| `enable_health_check`   | Perform a WordPress health check     | ❌ No     | `yes`  |


### Requirements
- Git Deployment Enabled: Activate Git Deployment for your site via the GoDaddy control panel.
- GitHub Secrets: Ensure the following secrets are configured in your repository:
    - `PRIVATE_KEY`

### For additional troubleshooting:

- Double-check your remote host details.
- Consult the issues tab in this repository for similar problems and their resolution.

### License

This GitHub Action is licensed under the MIT License.

### Contributing

Contributions, bug reports, and ideas for improvements are welcome! Please open an issue or submit a pull request for discussion.

## Support
For additional help or support, please open an issue in this repository.