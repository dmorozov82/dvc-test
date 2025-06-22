# dvc-test
Test DVC VS code integration

# VS Code Tuning:
## Set Default Line Endings (LF) in VS Code Settings
Open VS Code settings:
> Ctrl + , (or go to File > Preferences > Settings)
> Search for "Eol"
Set: Eol = \n

# Configure .editorconfig (Recommended for Projects)
Create/modify .editorconfig in your project root:
```
[*]
end_of_line = lf
```

# Install chocolatey (admin elevated)
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install less (admin elevated)
choco install less

# Install Pyhton on windows (easiest way thru store)

# Instal DVC thru pip or VS Code extention 
pip install dvc
pip install "dvc[s3]" # or another storage option https://dvc.org/doc/user-guide/data-management/remote-storage

# Check installation:
dvc --version

# If fails - add it to path:
python -m pip show dvc
Name: dvc
Version: 3.60.1

Location: C:\Users\<YOUR_NAME>\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\LocalCache\local-packages\Python311\site-packages

> add this location to Path:
Press Win + R, type sysdm.cpl, and hit Enter.
Go to Advanced → Environment Variables.
Under System variables, find Path, click Edit.
Click New and paste:

C:\Users\<YOUR_NAME>\AppData\Local\Packages\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\LocalCache\local-packages\Python311\Scripts

> restart vscode or powershell

# Begin to work with DVC
dvc config cache.type hardlink,symlink
dvc config core.autostage true
dvc init
dvc remote add -d mlops s3://localstack-mlops
> Setting 'mlops' as a default remote.

dvc add .\data\test.txt  # this will add file to dvc repo, create cache, etc.. 

### Configure AWS Credentials for LocalStack
LocalStack typically uses dummy credentials. Add them to your environment:
``` PowerShell
$env:AWS_ACCESS_KEY_ID="test"
$env:AWS_SECRET_ACCESS_KEY="test"
$env:AWS_DEFAULT_REGION="us-east-1"
```
### Configure DVC to use LocalStack's endpoint
LocalStack runs on http://localhost:4566 by default. Tell DVC to use it:
``` powershell
dvc remote modify mlops endpointurl http://localhost:4566
dvc remote modify mlops --local use_ssl false  # Disable HTTPS (LocalStack uses HTTP)
```
### Try dvc push to s3
``` powershell
dvc push
```
> Output:
Collecting
Pushing
1 file pushed   

### Check remote s3
dvc list . --remote=mlops
curl http://localhost:4566/localstack-mlops
awslocal s3 ls s3://localstack-mlops/ --recursive --human-readable

### We will store our DATA in '/data' folder, so we must Set Up .gitignore for DVC
Add /data to .gitignore but keep tracking .dvc files in Git:

```powershell
# Create .gitignore if it doesn't exist
if (!(Test-Path .gitignore)) { New-Item .gitignore -ItemType File }
# Add rules to ignore /data/ but keep .dvc files
@"
# Ignore all files in /data/
/data/**
# Except .dvc files (tracked by Git)
!/data/**/*.dvc
"@ | Out-File -Append -Encoding utf8 .gitignore
```

```powershell
git add .gitignore
git commit -m "Ignore data folder but track .dvc files"
```

### Why This Works for DVC
- Git ignores raw data (e.g., data/file.jpg)
- Git tracks .dvc files (data/file.jpg.dvc)
- DVC uses .dvc files to manage the actual data in remote storage