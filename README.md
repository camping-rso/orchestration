# Camping microservices orchestration and automation with xOpera with Ansible

## Table of Contents
  - [Purpose](#purpose)
  - [Functionality](#functionality)
  - [Quick test and deploy](#quick-test-and-deploy)
  - [Running with xOpera](#running-with-xopera)
    - [Setting inputs](#inputs)
    - [Deploy](#deploy)
    - [Undeploy](#undeploy)

## Purpose
The repository implements the orchestration and automation of camping microservices on managed Microsoft Azure Kubernetes
instance. The orchestration is done using [xOpera TOSCA orchestrator](https://github.com/xlab-si/xopera-opera) and 
[Ansible](https://www.ansible.com/) is used for the automation. 

The solution does the following:

- login to the Azure account
- create a new Azure Resource Group
- create a new Azure Kubernetes Service (AKS) cluster
- deploy camping microservices on AKS
- create an ingress controller on AKS
- establish monitoring with Datadog

## Functionality
The main functionality of image-resize is to create thumbnails from the source image. Source image must be uploaded into
source bucket and then three thumbnails will be created and saved to another bucket.

## Quick test and deploy
If you want to test this solution immediately run the following commands:

```shell script
# clone this repo
git clone git@github.com:camping-rso/orchestration.git
cd orchestration

# install prerequisites into Python virtualenv
python3 -m venv .venv && . .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

# run xOpera deployment (make sure to setup the inputs in inputs.yml file)
opera deploy -i inputs.yml service_template.yml

# run undeployment if you wish to tear down the deployed services
opera undeploy
```

## Running with xOpera

This topic explains orchestration of the prepared TOSCA YAML artifact using xOpera.

## Inputs

The artifact gets deployed from an existing virtual env that should be created before. 
To begin the orchestration you have to install xOpera to your virtualenv with pip. 
Parameters are passed to xOpera via `inputs.yml` file that can be modified. 
Currently the following values can be modified:
                 
| Variable | Description | Example
|:-------------|:-------------|:-------------|
| `host_ip` | Host ip for the deployment | localhost |
| `azure_credentials` | Azure credentials, is of type map which should include both Azure username and password | { username: user, password: secret } |
| `location` | The location for the Azure resources | westeurope |
| `resource_group_name` | The name of the testing resource group | CampingResourceGroup |
| `aks_name` | The name for the new AKS cluster | camping |
| `camping_overview_ms_db_connection_string` | Connection string to PostgreSQL | "Host=localhost;Port=5432;Database=camping;Username=postgres;Password=potgres;Pooling=true;" |
| `camping_reservations_ms_db_connection_string` | Connection string to PostgreSQL | "Host=localhost;Port=5432;Database=camping;Username=postgres;Password=potgres;Pooling=true;" |
| `camping_opinions_ms_db_connection_string` | Connection string to PostgreSQL | "Host=localhost;Port=5432;Database=camping;Username=postgres;Password=potgres;Pooling=true;" |
| `camping_images_ms_postgres_credentials` | PostgreSQL credentials | { username: postgres, password: postgres } |
| `datadog_api_key` | Datadog API key | dkwferf4g89r4g89rg4r4t84 |

## Deploy
To deploy the prepared blueprint run `opera deploy -i inputs.yml service_template.yml`
If the deploy has finished successfully your AKS was created and is prepared for further usage.

### Undeploy
Run `opera undeploy` to undeploy the solution. 
This will remove the created Azure Kubernetes service from the Azure portal along with prepared environment (resource group 
and other resources).

