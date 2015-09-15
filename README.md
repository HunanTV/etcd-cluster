ETCD Cluster with ERU
=====================

Build an etcd cluster with 3 nodes, across 2 server rooms.

Steps:

1. Set eru environments

    	nbe app:setenv prod ETCD_INITIAL_CLUSTER=node0=http://10.100.1.145:3001,node1=http://10.100.1.155:3001,node2=http://10.1.201.110:3001
    	nbe app:setenv prod ETCD_INITIAL_CLUSTER_STATE=new

2. Make sure `/data/etcd-cluster` is on server and has the right privilege.

3. Deploy

    * node0

        	nbe app:dpri platform dns node0-host --image ${etcd-image} --raw --host ${145}

    * node1

        	nbe app:dpri platform prod node1-host --image ${etcd-image} --raw --host ${155}

    * node2

        	nbe app:dpri platform app node2-host --image ${etcd-image} --raw --host ${110}

4. Add a node

    * Set new environments

    	    nbe app:setenv add ETCD_INITIAL_CLUSTER=node0=http://10.100.1.145:3001,node1=http://10.100.1.155:3001,node2=http://10.1.201.110:3001,node3=${node3-url}
    	    nbe app:setenv add ETCD_INITIAL_CLUSTER_STATE=existing

    * Add member

            etcdctl member add node3 ${node3-url}

    * Deploy with environment `add`, also see [here](https://github.com/coreos/etcd/blob/master/Documentation/runtime-configuration.md#add-a-new-member).

5. Remove a node

    Use `etcdctl` to remove a member, see [here](https://github.com/coreos/etcd/blob/master/Documentation/runtime-configuration.md#remove-a-member).

6. Recover

    Just restart the container. If this doesn't work, remove the node first and then add it again.
