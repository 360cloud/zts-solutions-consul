## Cluster Peering between two Openshift Cluster:

[Reference for Cluster peering](https://developer.hashicorp.com/consul/tutorials/developer-mesh/cluster-peering)

Consul cluster peering works on both Enterprise and OSS versions of Consul. On Consul OSS, you can only peer clusters between the default partitions. On Consul Enterprise, you can peer clusters between any partition.

We will now peer the two data centers to enable services in DC1 to communicate to product-api in DC2. Consul cluster peering works by defining two cluster roles:

We will be peering serevices running in two Openshift projects ( k8s namespaces) demo between two Consul Datacentres.

DC1 - A peering acceptor is the cluster that generates a peering token and accepts an incoming peering connection.

DC2 - A peering dialer is the cluster that uses a peering token to make an outbound peering connection with the cluster that generated the token

### Step 1 - Create dev project in Openshift Cluster 1 and 2 .

Apply and configures the Consul clusters to use mesh gateways for cluster peering: 

In DC1 and DC2:
`cd peering; kubectl apply -f meshgateway.yaml -n dev`

### Step 2 - Apply Proxy Default settings for DC1 and DC2:
In DC1 and DC2:
`cd peering; kubectl apply -f proxy-default.yaml -n dev`


### Step 3 - Create a peering token on DC1:

In DC1:
`cd peering/dc1; kubectl apply -f acceptor.yaml -n dev`

### Step 4 - Import the peering token generated in DC1 into DC2


### Step 5 - Establish Peering - Establish a connection between clusters

Configure a PeeringDialer role for DC2. This will create a peering connection from the second datacenter towards the first one.


Verify the Peering Connection:
