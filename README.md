# Running Consul On OpenShift

This guide will walk you through the steps required to run Enterprise Consul on OpenShift.

## Prerequisites

- Access to an OpenShift cluster
- Knowledge of basic OpenShift concepts (pods, services, routes, etc.)
- oc command line tool installed and configured

For this environment I have used Microsoft Azure <https://learn.microsoft.com/en-us/azure/openshift/tutorial-create-cluster>


### Step 1 - Create a new project

Create a new project in OpenShift where you will deploy Consul.

`$ oc new-project my-consul-project`


### Step 2 - Apply Enterprise License 

Apply Consul Enterprise License.

### Step 3 - Apply Helm Chart

`helm install sm hashicorp/consul --values openshift/helm/ocphcp.yaml --create-namespace --namespace consul --wait`

### Step 4 - Create a dev project 

`oc new-project dev`

###