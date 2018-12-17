Hive Cluster Deployment
========================================
About Hive Cluster, which is a toolchain that allows multiple IPFS nodes to work together to manage the same IPFS pinset. We called  it Hive Cluster Group, which are IPFS nodes work together. In Hive Cluster, the most purpose of using it is to build up Hive Cluster group.

The following steps will set up a hive cluster group, which has 3 IPFS clusters and every IPFS cluster has 1 IPFS peer. 

.. toctree::
   :maxdepth: 2
   
.. section-numbering::
   :depth: 5
   :start: 0

Prerequisites
--------------
Three x86_64 based computers, install the latest Linux distribution (recommended Ubuntu 18.04, or CentOS 7.5). Each computer has more than 2GB RAM. 

Install IPFS daemon
-------------------

Install hive-cluster binraries
******************************
Extract the hive-cluster package to a Linux path, and add the path string to system 'PATH' environment.

.. code-block:: bash

   $ cp hive-cluster/* /usr/local/bin
   # make sure '/usr/local/bin' is in the system PATH.
   $ export PATH=/usr/local/bin:$PATH

Initialize ipfs peers
*********************
run follows command to initialize ipfs repository

.. code-block:: bash

   # initialize ipfs repository.
   $ ipfs init
   ...
   # run the ipfs program to background
   $ ipfs daemon &
   ...
   # get the IPFS peer id
   $ ipfs id
   {
   	"ID": "QmbJM3KxEpcBgW11eURQK5MUmSZN6Ts6KwdKQtLXdYizrM",
   	"PublicKey": "CAASpgIwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDN3nMnpETXvE2XwW9m/OKtl+Kys3wbQGn2n7y3zC7lnIP85wjkvap8jwrrTPWR/ZJDrj9erhObYf6FteJEPBnRNorqe00AOPb8w36sbcrqEtPWdTEuegddouwYrSvvkjAk9PhVvAlfv9U2Xr8Wgl9PaV1qRSEjOyBu0FfS1IYhU61jbAKRJpPkYjp4dDwsMkIJgljPoGF66h4p/mu2ybiF5F4ZRUka6G/DJf+l+X4O8O3+5f8nvk1ku1IwDQrRWPSGmeiCTNDYHDlJnDbCJ7TT0igejT8h8Lpf9m+quuRFgxehzTzH9qs+gRg0C3Ya0m00l3QW9xuPAgkpmpWdfkWbAgMBAAE=",
   	"Addresses": [
   		"/ip4/127.0.0.1/tcp/4001/ipfs/QmbJM3KxEpcBgW11eURQK5MUmSZN6Ts6KwdKQtLXdYizrM",
   		"/ip4/222.222.222.222/tcp/4001/ipfs/QmbJM3KxEpcBgW11eURQK5MUmSZN6Ts6KwdKQtLXdYizrM",
   		"/ip4/192.168.0.177/tcp/4001/ipfs/QmbJM3KxEpcBgW11eURQK5MUmSZN6Ts6KwdKQtLXdYizrM",
   		"/ip6/::1/tcp/4001/ipfs/QmbJM3KxEpcBgW11eURQK5MUmSZN6Ts6KwdKQtLXdYizrM",
   		"/ip6/2408:8025:2a0:dd1:216:3eff:fe8a:e522/tcp/4001/ipfs/QmbJM3KxEpcBgW11eURQK5MUmSZN6Ts6KwdKQtLXdYizrM"
   	],
   	"AgentVersion": "go-ipfs/0.4.18/6f19dd506",
   	"ProtocolVersion": "ipfs/0.1.0"
   }
   
Change default bootstrap peers
*******************************************************
The default ipfs peer has a bootstrap peers list. When ipfs daemon starts, it will search and connect some of them. However, we will set up a private cluster group in Hive Cluster project. So we MUST remove all of default bootstrap peers in ipfs configuration, and add our IPFS peers in it.

Please do follow steps in the every IPFS peer.

.. code-block:: bash

   # remove all of default bootstrap peers
   $ ipfs bootstrap rm --all
   # add our own ipfs peers
   $ ipfs bootstrap add /ip4/222.222.222.222/tcp/4001/ipfs/QmbJM3KxEpcBgW11eURQK5MUmSZN6Ts6KwdKQtLXdYizrM"

Please note the upward. We should add an external network IP address (the upward IP is '222.222.222.222'). Otherwise, the other IPFS daemon should not connect to it.

Generate swarm.key and share it among the Hive peers
*******************************************************
In IPFS, the swarm is the net of peers connection. There is a swarm key(swarm.key file) is shared by the net. If ipfs daemon does not locate the swarm.key file, it will connect to global IPFS network. If we provide a swarm key, the IPFS daemon will try to use it to connect a private secret network.

.. code-block:: bash

   # generate swarm.key
   $ ipfs-swarm-key-gen > swarm.key
   # add our own ipfs peers to the ipfs config folder
   $ cp swarm.key $HIVE_IPFS_HOME/
   # copy the same swarm key to Hive cluster peers.
   $ scp swarm.key $HIVE_HOST:$HIVE_IPFS_HOME/

Install IPFS-cluster daemon
----------------------------
The IPFS-cluster is the helper daemon for to make ipfs peer join to cluster group. 

Every IPFS daemon has a IPFS-cluster helper. IPFS-cluster helper communicates with others by using P2P protocol either. The IPFS-cluster P2P fundamental(based on libp2p) is as same as IPFS daemon.

Initialize IPFS cluster
*******************************************************
The IPFS-cluster daemon initialization is as similar as ipfs daemon.

Before ipfs-cluster daemon runs, Please run 'ipfs-cluster init' first. This step will create configuration folder(default location is '~/.ipfs-cluster/').

.. code-block:: bash

   # create ipfs-cluster configurations
   $ ipfs-daemon init

Change the secret strings to same in all ipfs-cluster
********************************************************
After 'ipfs-cluster init' running, there is a cluster configuration file (service.json) is created in the configuration folder.  This step will change a secret string and share it in all ipfs-cluster daemon.

.. code-block:: bash

    $ cd ~/.ipfs-cluster
    $ cat service.json
    ...
    "cluster": {
       "id": "QmNTD6ZbhdaoDmjQqGJrp8dKEPvtBQGzRxwWHNcmvNYsbK",
       "peername": "YIMAC",
       "private_key": ...,
       "secret": "65becb3215c9a14d3e44bc3be7b4e7b7b9ceac9a580eb332498f72ef6acb3911",
       "leave_on_shutdown": true,
       ...
    },

Please record the 'secret' string, we MUST copy it to other cluster peers in this ipfs-cluster group.

Set master cluster and add other slave clusters from it
***********************************************************
Set a master cluster is very simple, run directly 'ipfs-cluster-service daemon' command.

.. code-block:: bash

   # ipfs-cluster runs
   $ ipfs-cluster-service daemon &

While ipfs-cluster-service running, we can add other slave clusters in any time. 

.. code-block:: bash

   # add slave cluster from master.
   $ ipfs-cluster-ctl peers add /ip4/222.222.223/tcp/9096/ipfs/QmZUAfeyWagrTAyubVgdXamsj8ca1MbegqZPVXbVtxgQcQ
   
Please note, the slave cluster secret string MUST keep it as same as master.
   
Slave ipfs-cluster join existing hive-cluster group
*****************************************************
We can also add cluster peers from a slave node. Please use the following commands.

.. code-block:: bash

   # Launch a peer and join existing cluster.
   $ ipfs-cluster-service daemon --bootstrap /ip4/222.222.222.222/tcp/9096/ipfs/QmNTD6ZbhdaoDmjQqGJrp8dKEPvtBQGzRxwWHNcmvNYsbK
