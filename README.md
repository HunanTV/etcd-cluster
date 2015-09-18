ETCD Cluster with ERU
=====================

Build an etcd cluster with 3 nodes, across 2 server rooms.

Steps:

1. Set eru environments

    	nbe app:setenv prod ETCD_INITIAL_CLUSTER=node0=${uri0},node1=${uri1},node2=${uri2}
    	nbe app:setenv prod ETCD_INITIAL_CLUSTER_STATE=new

2. Make sure `/data/etcd-cluster` is on server and has the right privilege.

3. Deploy

    * node0

        	nbe app:dpri platform dns node0-host --image ${etcd-image} --raw --host ${url0}

    * node1

        	nbe app:dpri platform prod node1-host --image ${etcd-image} --raw --host ${url1}

    * node2

        	nbe app:dpri platform app node2-host --image ${etcd-image} --raw --host ${url2}

4. Add a node

    * Set new environments

    	    nbe app:setenv add ETCD_INITIAL_CLUSTER=node0=${uri0},node1=${uri1},node2=${uri2},node3=${uri3}
    	    nbe app:setenv add ETCD_INITIAL_CLUSTER_STATE=existing

    * Add member

            etcdctl member add node3 ${uri3}

    * Deploy with environment `add`, also see [here](https://github.com/coreos/etcd/blob/master/Documentation/runtime-configuration.md#add-a-new-member).

5. Remove a node

    Use `etcdctl` to remove a member, see [here](https://github.com/coreos/etcd/blob/master/Documentation/runtime-configuration.md#remove-a-member).

6. Recover

    Just restart the container. If this doesn't work, remove the node first and then add it again.
