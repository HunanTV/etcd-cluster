ETCD Cluster with ERU
=====================

Build an etcd cluster with 3 nodes, across 2 server rooms.

Steps:

1. Set eru environments

    	nbe app:setenv prod ETCD_INITIAL_CLUSTER=node0=http://10.100.1.145:3001,node1=http://10.100.1.155:3001,node2=http://10.1.201.110:3001
    	nbe app:setenv prod ETCD_INITIAL_CLUSTER_STATE=new
    	nbe app:setenv recover ETCD_INITIAL_CLUSTER=node0=http://10.100.1.145:3001,node1=http://10.100.1.155:3001,node2=http://10.1.201.110:3001
    	nbe app:setenv recover ETCD_INITIAL_CLUSTER_STATE=existing

2. Make sure `/data/etcd-cluster` is on server and has the right privilege.

3. Deploy

    * node0

        	nbe app:dpri platform dns node0-host --image ${etcd-image} --raw --host ${145}

    * node1

        	nbe app:dpri platform prod node1-host --image ${etcd-image} --raw --host ${155}

    * node2

        	nbe app:dpri platform app node2-host --image ${etcd-image} --raw --host ${110}

4. Recover

    If one node is down and then needs to be recovered, deploy with `recover` environment. Make sure `/data/etcd-cluster` is still available.
