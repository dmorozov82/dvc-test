# Guide to Install and Set Up LocalStack on Ubuntu 22.04 (WSL)

## Prerequisites
- Ubuntu 22.04 running on WSL (Windows Subsystem for Linux)
- Basic familiarity with command line tools

## Installation Steps

### 1. Install Python and pip
```bash
sudo apt update
sudo apt install -y python3-pip python3-venv python-is-python3 jq zip curl
```

### 2. Install Docker
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 3. Install LocalStack
```bash
pip3 cache purge
pip install --upgrade pip setuptools
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

pip install --no-cache-dir localstack
localstack --version
```

### 4. Set Up LocalStack Authentication
1. Get your auth token from [LocalStack Cloud](https://app.localstack.cloud/settings/auth-tokens)
2. Configure the token:
```bash
localstack auth set-token YOUR_AUTH_TOKEN
```

### 5. Install AWS CLI Tools
```bash
# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Install awslocal
pip install awscli-local[ver1]
```

## Running LocalStack

### Start LocalStack in detached mode
```bash
localstack start -d
```

### Stop LocalStack
```bash
localstack stop
```

### Alternative: Run with specific services and ports
```bash
docker run -d \
  -p 4566:4566 \
  -p 8443:443 \
  -e "SERVICES=s3,dynamodb" \
  localstack/localstack
```

## Basic Usage Examples

### Create an S3 bucket
```bash
awslocal s3 mb s3://mlops
awslocal s3 ls
```

### Work with files in S3
```bash
# List bucket contents
awslocal s3 ls s3://mlops

# Upload a file
awslocal s3 cp file.txt s3://mlops/

# Access from Windows
curl http://localhost:4566/mlops
```

## Verification
Verify all installations:
```bash
python3 --version
pip3 --version
docker --version
localstack --version
```

This guide provides a complete setup for LocalStack on Ubuntu 22.04 running in WSL, including Docker, AWS CLI tools, and basic usage examples.