:orphan:


====================
Hive Troubleshooting
====================

1) It seems all hive cluster hosts are on activities, but HTTP client unable to fetch, upload or write data to them.

  ipfs daemon is on the hive cluster backend, it manages data repository and gives responses for user data access(via cluster HTTP API). Please check the all ipfs daemons in cluster hosts whether they are still on serving. If any ipfs daemon process dead or froze, hive cluster could not work correctly.

  For example, in below cluster have five hosts, but an ipfs of them is in a connection refused state(see: IPFS ERROR). Please kill the old ipfs process and restart it. Furthermore information, please read the ipfs documents.

.. code-block:: bash

  ## check cluster hosts states.
  $ ipfs-cluster-ctl peers ls
  QmNTD6ZbhdaoDmjQqGJrp8dKEPvtBQGzRxwWHNcmvNYsbK | YIMAC | Sees 4 other peers
    > Addresses:
      - /ip4/10.10.1.20/tcp/62878/ipfs/QmNTD6ZbhdaoDmjQqGJrp8dKEPvtBQGzRxwWHNcmvNYsbK
      - /ip4/127.0.0.1/tcp/9096/ipfs/QmNTD6ZbhdaoDmjQqGJrp8dKEPvtBQGzRxwWHNcmvNYsbK
      - /p2p-circuit/ipfs/QmNTD6ZbhdaoDmjQqGJrp8dKEPvtBQGzRxwWHNcmvNYsbK
    > IPFS ERROR: Post http://127.0.0.1:5001/api/v0/id: dial tcp 127.0.0.1:5001: connect: connection refused
    ... ...

2) ipfs unable to connect to swarm.

  In hive cluster, ipfs is a privately built version that removed bootstrap peers. Please make it join ipfs swarm by manual, besides, please bootstrap id to an ipfs config file.

.. code-block:: bash

  ### manually connect a peer
  $ ipfs swarm connet /ip4/222.222.1.20/tcp/62878/ipfs/QmNTD6ZbhdaoDmjQqGJrp8dKEPvtBQGzRxwWHNcmvNYsbK

  About how to add a peer to the configuration, please locate the config file(usually, it is at "/var/lib/hive/ipfs/config") and insert peer id to "Bootstrap".

3) ipfs unable bind an external ip address

  Sometimes an ipfs listener can not directly bind to its external address. It is, e.g. the case with NAT, where it binds to an IP address in the local network, while other peers dial it using the local router's public IP address. If you knew current peer mapped external "ip and port", you can change the ipfs addresses "Announce"(usually, it is at "/var/lib/hive/ipfs/config").
 
.. code-block:: json

  "Addresses": {
    "Announce": ["/ip4/222.222.147.205/tcp/4001"],
  }

4) Hive host disconnects from cluster group, but it does not re-join into the cluster group.

  In currently, cluster use raft protocol to maintain whole group hosts. Sometimes, if a host drop from a group, and at the same time, the cluster master changes to another host, the new cluster master maybe not recognize the former member. 
  It is indeed a big trouble in currently, but it will be resolved. Now Please delete the "raft" directory in the cluster config, and rebuild hosts to a cluster.