# essential-azure-function
essential-azure-function


https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=v4%2Clinux%2Cpython%2Cportal%2Cbash#local-settings-file


https://learn.microsoft.com/en-us/azure/azure-functions/create-first-function-vs-code-python?pivots=python-mode-decorators


# Step 1 Download azure function core

Download core
```
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
```

# Step 2 Set up the APT source list
Check distro ver
```
/etc/*-release files

# Ubunto
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'

# Debian
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/debian/$(lsb_release -rs | cut -d'.' -f 1)/prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
```

# Step 3 Install function core

```
sudo apt-get update
sudo apt-get install azure-functions-core-tools-4
```

# Step 4 Install .net sdk

```
wget https://packages.microsoft.com/config/debian/11/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-7.0
```


# Step 5 Init/Start function (Python)

```
mkdir EventGridTriggerMigrateData
func init EventGridTriggerMigrateData
```
choose python (or other languages)


**(Optional)** One can connect to existing storage account/function
```
# Download all settings from an existing function app:
func azure functionapp fetch-app-settings <FunctionAppName>

# Get the Connection string for a specific storage account:
func azure storage fetch-connection-string test3raychunkitchung
```

**(Optional)** Install python & pip
```
# Install python
sudo apt-get install python3 python3-dev
sudo ln -sf /usr/bin/python3 /usr/bin/python
export PYTHONPATH=/usr/bin/python
python --version

# Install pip
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python get-pip.py
```

Create a new example function
```
func new --template "Http Trigger" --name MyHttpTrigger
func start
```
Check in browser http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks


# Step 6 Setup unit test (Python)
Coming soon...

# Step 7 

