# FastAPI Deployment with GitHub Actions

This project demonstrates automated deployment of a FastAPI application using GitHub Actions.

## Project Structure

- `main.py` - FastAPI application running on port 9001
- `.github/workflows/deploy.yml` - GitHub Actions workflow for deployment
- `pyproject.toml` - Python project configuration with dependencies

## Deployment Process

The GitHub Action automates the following steps:

1. **Copy files to server**: `scp -r . alertli_ai@13.215.192.207:~/hello/`
2. **Attach to screen session**: `screen -r hello`
3. **Stop existing server**: Stop any process running on port 9001
4. **Run application**: `uv run main.py`

## Setup Instructions

### 1. GitHub Secrets Configuration

To use this GitHub Action, you need to configure the following secret in your GitHub repository:

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add the following secret:

**Secret Name**: `SSH_PRIVATE_KEY`
**Secret Value**: Your private SSH key content

### 2. SSH Key Setup

If you don't have SSH keys set up:

1. Generate a new SSH key pair:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

2. Copy the public key to your server:
   ```bash
   ssh-copy-id alertli_ai@13.215.192.207
   ```

3. Copy the private key content and add it as the `SSH_PRIVATE_KEY` secret in GitHub

**Important**: When copying the private key content:
- Include the entire key including the `-----BEGIN OPENSSH PRIVATE KEY-----` and `-----END OPENSSH PRIVATE KEY-----` lines
- Make sure there are no extra spaces or line breaks
- The key should be in OpenSSH format (default for newer ssh-keygen versions)

### 3. Server Preparation

Ensure your server has:
- Python 3.13+ installed
- `uv` package manager installed
- Screen session named "hello" created
- Proper permissions for the `alertli_ai` user

## Workflow Triggers

The deployment workflow runs:
- Automatically on every push to the `main` branch
- Manually via the GitHub Actions tab (workflow_dispatch)

## Application

The FastAPI application provides a simple "Hello, World!" endpoint at the root URL (`/`) and runs on port 9001.

## Verification

After deployment, the workflow automatically verifies the deployment by making a request to `http://13.215.192.207:9001/`.
