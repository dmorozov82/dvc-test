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
dvc init
dvc remote add -d mlops s3://localstack-mlops
> Setting 'mlops' as a default remote.

dvc add .\data\test.txt
