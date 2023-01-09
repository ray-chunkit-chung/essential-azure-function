# essential-azure-function

essential-azure-function

<https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=v4%2Clinux%2Cpython%2Cportal%2Cbash#local-settings-file>

<https://learn.microsoft.com/en-us/azure/azure-functions/create-first-function-vs-code-python?pivots=python-mode-decorators>

## Step 1 Download azure function core

Download core

```bash
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
```

## Step 2 Set up the APT source list

Check distro ver

```bash
/etc/*-release files

# Ubunto
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'

# Debian
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/debian/$(lsb_release -rs | cut -d'.' -f 1)/prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
```

## Step 3 Install function core

```bash
sudo apt-get update
sudo apt-get install azure-functions-core-tools-4
```

## Step 4 Install .net sdk

```bash
wget https://packages.microsoft.com/config/debian/11/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb
sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-7.0
```

## Step 5 Init/Start function (Python)

```bash
mkdir EventGridTriggerMigrateData
func init EventGridTriggerMigrateData
```

choose python (or other languages)

**(Optional)** One can connect to existing storage account/function

```bash
# Download all settings from an existing function app:
func azure functionapp fetch-app-settings <FunctionAppName>

# Get the Connection string for a specific storage account:
func azure storage fetch-connection-string test3raychunkitchung
```

**(Optional)** Install python & pip

```bash
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

```bash
func new --template "Http Trigger" --name MyHttpTrigger
func start
```

Check in browser <http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks>

## Step 6 Setup unit test (Python)

Coming soon...

## Step 7 Create a function app

We need a function app to publish the function
<https://learn.microsoft.com/en-us/azure/azure-functions/scripts/functions-cli-create-serverless>

Install azure cli

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Set app variables by saving in .env file

```bash
export SUBSCRIPTION="xxx"
export TENANT="xxx"
export LOCATION="eastus"
export RESOURCE_GROUP="xxx"
export STORAGE_ACCOUNT="xxx"
export SKU_STORAGE="Standard_LRS"
export FUNCTION_APP="xxx"
export FUNCTION_VERSION="4"
export PYTHON_VERSION="3.9"
```

Set subscription

```bash
source .env
az login --tenant $TENANT
az account set -s $SUBSCRIPTION
```

Create resource group, storage account, and functionapp service plan

```bash
source .env
az group create --name $RESOURCE_GROUP \
                --location $LOCATION
az storage account create --name $STORAGE_ACCOUNT \
                          --location $LOCATION \
                          --resource-group $RESOURCE_GROUP \
                          --sku $SKU_STORAGE
az functionapp create --name $FUNCTION_APP \
                      --storage-account $STORAGE_ACCOUNT \
                      --consumption-plan-location $LOCATION \
                      --resource-group $RESOURCE_GROUP \
                      --functions-version $FUNCTION_VERSION \
                      --os-type Linux \
                      --runtime python \
                      --runtime-version $PYTHON_VERSION
```

## Step 8 Publish function to function app

```bash
func azure functionapp publish $FUNCTION_APP
```

test in browser <https://xxxx.azurewebsites.net/api/myhttptrigger?name=yeaeeaeaehh>

## Finally Delete resources

After experiment, delete all resources to avoid charging a lot of money

```bash
az group delete --name $RESOURCE_GROUP
```
