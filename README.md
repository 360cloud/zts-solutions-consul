# Running Consul On OpenShift

This Readme guide will walk you through the steps required to run Enterprise Consul on OpenShift.

## Prerequisites

- Access to an OpenShift cluster
- Knowledge of basic OpenShift concepts (pods, services, routes, etc.)
- oc command line tool installed and configured

For setting up Redhat Openshift on Microsoft Azure follow these instructions: <https://learn.microsoft.com/en-us/azure/openshift/tutorial-create-cluster>
Reference Tutorial from HashiCorp : <https://developer.hashicorp.com/consul/tutorials/kubernetes/kubernetes-openshift-red-hat#create-a-network-attachment-definition>

Video Instructions: <https://youtu.be/05u-DbJ7pJo>


### Step 1 - Create a new project ( k8s namespace) consul

Create a new project in OpenShift where you will deploy Consul.

`$ oc new-project my-consul-project`

### Step 2 - Apply Enterprise License:

Apply Consul Enterprise License.

### Step 3 - Apply Helm Chart

`helm install sm hashicorp/consul --values openshift/helm/ocphcp.yaml --create-namespace --namespace consul --wait`

### Step 4 - Create a dev project (k8s namespace)

`oc new-project dev`  or `kubectl create ns dev`

Apply network attachment definition  into dev namespace 

`kubectl apply -f networkattachementdefinition.yaml -n dev`

Apply security context for dev namespace:

`oc adm policy add-scc-to-group anyuid system:serviceaccounts:dev`


### Deploy Service Mesh Applications 

`cd openshift/app-deploy`

`kubectl apply counting.yaml -n dev`

`kubectl apply dashboard.yaml -n dev`

## Port Forward and Check Dashboard UI  

Since ACL's are enabled in this helm Chart by default dashboard and counting applications won't be able to communicate with each other

Apply Intention for allowing traffic from Dashboard to Counting application:


`kubectl apply intentions/dashboard-to-counting.yaml -n dev`

Dashboard UI should work fine now.