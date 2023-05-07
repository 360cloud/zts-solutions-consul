# Running Consul On OpenShift

This Readme guide will walk you through the steps required to run Enterprise Consul on OpenShift.

## Prerequisites

- Access to an OpenShift cluster
- Knowledge of basic OpenShift concepts (pods, services, routes, etc.)
- oc command line tool installed and configured

For setting up Redhat Openshift on Microsoft Azure follow these instructions: [OpenShift on Microsoft Azure](https://learn.microsoft.com/en-us/azure/openshift/tutorial-create-cluster)

**Deploy Consul On Redhat OpenShift:**
[Reference Tutorial from HashiCorp](https://developer.hashicorp.com/consul/tutorials/kubernetes/kubernetes-openshift-red-hat)

**Video Instruction Tutorial:**
[Youtube Link](https://youtu.be/05u-DbJ7pJo)


### Step 1 - Create a new project ( k8s namespace) consul

Create a new project in OpenShift for deploying Consul Enterprise. OpenShift projects and Kubernetes namespaces serve a similar purpose, but OpenShift projects offer additional functionality and policy enforcement that are not available in Kubernetes namespaces.

`$ oc new-project my-consul-project`

### Step 2 - Apply Consul Enterprise License. To obtain a license for HashiCorp's enterprise Consul product, you will need to contact HashiCorp directly. You can do this by visiting their website and filling out a form to request more information about enterprise licensing. Alternatively, you can contact their sales team directly via email or phone.

[Apply Consul Enterprise License](https://developer.hashicorp.com/consul/docs/k8s/deployment-configurations/consul-enterprise)

### Step 3 - Apply Helm Chart

`helm install sm hashicorp/consul --values openshift/helm/ocphcp.yaml --create-namespace --namespace consul --wait`

### Step 4 - For Installing and Configuring Service Mesh on Openshift. Create a dev project (k8s namespace)

`oc new-project dev`  or `kubectl create ns dev`

### Step-4 - In OpenShift, a Network Attached Definition (NAD) is a custom resource that allows you to define and manage network interfaces for containers running within a pod. Apply network attachment definition into dev namespace.

`kubectl apply -f networkattachementdefinition.yaml -n dev`

**Apply security context for dev namespace:**

`oc adm policy add-scc-to-group anyuid system:serviceaccounts:dev`


### Deploy Service Mesh Applications into Dev namespace.

`cd openshift/app-deploy`

`kubectl apply counting.yaml -n dev`

`kubectl apply dashboard.yaml -n dev`

### Port Forward and Check Dashboard UI. 

Since ACL's are enabled in this helm Chart by default dashboard and counting applications won't be able to communicate with each other

Apply Intention for allowing traffic from Dashboard to Counting application:

`kubectl apply intentions/dashboard-to-counting.yaml -n dev`

Dashboard UI should work fine now.
