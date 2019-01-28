Cluster Commands & API Reference
========================================

The typical IPFS peer is a resource hunger program. If you install IPFS daemon to your mobile device,
it will take up resources and slow down your device.
We are creating Hive project, which is a low resources consumption scenario.

Hive maintains a big IPFS pinset for sharing. It can provide numerous virtual IPFS peers with only one run a real IPFS peer.
Hive project distils from IPFS Cluster, but it will have many differences with the IPFS Cluster.

.. toctree::
   :maxdepth: 2

.. section-numbering::
   :depth: 5
   :start: 0

HTTP API
========

Cluster API vs Node API:

  Cluster APIs are used to manage HIVE cluster, Node APIs are used to interact with file objects.
  By default, Cluster APIs are accessed via port 9094, and Node APIs are accessed via port 9095.
  All of configurable options are saved in the file "service.json". Please refer: https://cluster.ipfs.io/documentation/configuration/

File API vs Files API:

  In the node APIs, which have two type of file API: file API and files API.

- file APIs are used to interact with object.
- files APIs are used to interact with virtual file system.
- In files APIs context, it must associate a uid to distinguish different filesystems.

Index
-----

=================================
`Cluster APIs`
=================================

* :ref:`/id`
* :ref:`/peers`
* :ref:`/peers/{peerID}`
* :ref:`/pins`
* :ref:`/pins/{cid}/sync`
* :ref:`/pins/{cid}/recover`
* :ref:`/pins/recover`

=======================
`Node APIs`
=======================

* :ref:`/api/v0/uid/new`
* :ref:`/api/v0/uid/login`
* :ref:`/api/v0/uid/info`
* :ref:`/api/v0/file/pin/add`
* :ref:`/api/v0/file/pin/ls`
* :ref:`/api/v0/file/pin/rm`
* :ref:`/api/v0/file/add`
* :ref:`/api/v0/file/get`
* :ref:`/api/v0/file/ls`
* :ref:`/api/v0/files/cp`
* :ref:`/api/v0/files/flush`
* :ref:`/api/v0/files/ls`
* :ref:`/api/v0/files/mkdir`
* :ref:`/api/v0/files/mv`
* :ref:`/api/v0/files/read`
* :ref:`/api/v0/files/rm`
* :ref:`/api/v0/files/stat`
* :ref:`/api/v0/files/write`
* :ref:`/api/v0/name/publish`
* :ref:`/api/v0/message/pub`
* :ref:`/api/v0/message/sub`
* :ref:`/version`

Cluster Server Managment
------------------------

===================
/id
===================

Show the cluster peers and its daemon information

:METHOD:
  GET

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - verbose
     - bool
     - no
     - display all extra information.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

	{
      "id": "<NodeId>",
      "cluster-peers":
      [
        "cluster_peers_addresses": [],
        "version": "<string>",
        "commit": "<string>",
        "rpc_protocol_version": "<string>",
        "error": "<string>",
        "ipfs":
        {
          "id": "<string>",
          "addresses": [],
          "error": "<string>",
        },
        "peername": "<string>",
      ]
	}

Example: curl http://10.10.165.11:9094/id

.. code-block:: json

    {
      "id": "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
      "addresses": [
        "/p2p-circuit/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
        "/ip4/127.0.0.1/tcp/9096/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
        "/ip4/10.10.165.11/tcp/9096/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
        "/ip4/58.246.225.150/tcp/23868/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ"
      ],
      "cluster_peers": [
        "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ"
      ],
      "cluster_peers_addresses": [],
      "version": "0.7.0+git13a161f056792cfb50c1ef53b6688716a211381f",
      "commit": "",
      "rpc_protocol_version": "/hivecluster/0.7/rpc",
      "error": "",
      "ipfs": {
        "id": "QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
        "addresses": [
          "/ip4/127.0.0.1/tcp/4001/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
          "/ip4/10.10.165.11/tcp/4001/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
          "/ip6/::1/tcp/4001/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
          "/ip4/58.246.225.150/tcp/43114/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG"
        ],
        "error": ""
      },
      "peername": "localhost.localdomain"
    }


===================
/peers
===================
List the cluster servers with open connections.

:METHOD: 
  GET

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - verbose
     - bool
     - no
     - display all extra information.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

	[
	  {
		"id": "<NodeId>",
		"addresses": [],
		"cluster-peers": [],
		"cluster_peers_addresses": [],
		"version": "<string>",
		"commit": "<string>",
		"rpc_protocol_version": "<string>",
		"error": "<string>",
		"ipfs":
		{
		  "id": "<string>",
		  "addresses": []	,
		  "error": "<string>",
		},
		"peername": "<string>", 
	  }
	]
  
Example: curl http://10.10.165.11:9094/peers

.. code-block:: json

	[
	  {
		"id": "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		"addresses": [
		  "/p2p-circuit/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "/ip4/127.0.0.1/tcp/9096/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "/ip4/10.10.165.11/tcp/9096/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "/ip4/58.246.225.150/tcp/23868/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ"
		],
		"cluster_peers": [
		  "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn"
		],
		"cluster_peers_addresses": [
		  "/p2p-circuit/ipfs/QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
		  "/ip4/127.0.0.1/tcp/9096/ipfs/QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
		  "/ip4/10.10.165.12/tcp/9096/ipfs/QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn"
		],
		"version": "0.7.0+git13a161f056792cfb50c1ef53b6688716a211381f",
		"commit": "",
		"rpc_protocol_version": "/hivecluster/0.7/rpc",
		"error": "",
		"ipfs": {
		  "id": "QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
		  "addresses": [
			"/ip4/127.0.0.1/tcp/4001/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
			"/ip4/10.10.165.11/tcp/4001/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
			"/ip6/::1/tcp/4001/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
			"/ip4/58.246.225.150/tcp/43114/ipfs/QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG"
		  ],
		  "error": ""
		},
		"peername": "localhost.localdomain"
	  },
	  
	  {
		"id": "QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
		"addresses": [
		  "/p2p-circuit/ipfs/QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
		  "/ip4/127.0.0.1/tcp/9096/ipfs/QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
		  "/ip4/10.10.165.12/tcp/9096/ipfs/QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn"
		],
		"cluster_peers": [
		  "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn"
		],
		"cluster_peers_addresses": [
		  "/ip4/10.10.165.11/tcp/9096/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "/p2p-circuit/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "/ip4/127.0.0.1/tcp/9096/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "/ip4/58.246.225.150/tcp/23868/ipfs/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ"
		],
		"version": "0.7.0+git13a161f056792cfb50c1ef53b6688716a211381f",
		"commit": "",
		"rpc_protocol_version": "/hivecluster/0.7/rpc",
		"error": "",
		"ipfs": {
		  "id": "QmTcTuPCjkD8JwJmzYrERZNvn3ACyDwE78RbXuWXzozkY3",
		  "addresses": [
			"/ip4/127.0.0.1/tcp/4001/ipfs/QmTcTuPCjkD8JwJmzYrERZNvn3ACyDwE78RbXuWXzozkY3",
			"/ip4/10.10.165.12/tcp/4001/ipfs/QmTcTuPCjkD8JwJmzYrERZNvn3ACyDwE78RbXuWXzozkY3",
			"/ip6/::1/tcp/4001/ipfs/QmTcTuPCjkD8JwJmzYrERZNvn3ACyDwE78RbXuWXzozkY3"
		  ],
		  "error": ""
		},
		"peername": "localhost.localdomain"
	  }
	]

===================
/peers/{peerID}
===================
Remove a cluster peer from the cluster.

:METHOD:
  DELETE

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - peerID
     - string
     - yes
     - a string format of peer id.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

Example: curl -v -X DELETE http://i.storswift.com:9194/peers/QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ

.. code-block:: json

	status code 204 NO CONTENT

	*peer removed from Raft: QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7CN consensus.go:382*

===================
/pins
===================
list current status of pins in the cluster.

:METHOD: 
  GET

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - verbose
     - bool
     - no
     - Also write the hashes of non-broken pins.
   * - quiet
     - bool
     - no
     - Write just hashes of broken pins. default: no.
     
.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

.. code-block:: json

	[
	  {
		"Cid": "<string>",
		"peer_map":
		{
          "Peer ID String"
          {
			"cid": "<string>",
			"peer": "<string>",
			"peername": "<string>",
			"status": "<string>",
			"timestamp": "<string>",
			"error": "<string>"
		  }
	    }
      }
    ]
	  
Example: curl http://10.10.165.11:9094/pins

.. code-block:: json

	[
	  {
		"cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
		"peer_map": {
		  "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ": {
			"cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
			"peer": "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
			"peername": "localhost.localdomain",
			"status": "pinned",
			"timestamp": "2019-01-07T07:46:34Z",
			"error": ""
		  },
		  "QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn": {
			"cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
			"peer": "QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
			"peername": "localhost.localdomain",
			"status": "pinned",
			"timestamp": "2019-01-08T06:46:59Z",
			"error": ""
		  }
		}
	  }
	]


=================
/pins/{CID}/sync
=================
Sync local status from IPFS

:METHOD: 
  POST

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - arg
     - string
     - no
     - the object CID that need sync.
     
.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

.. code-block:: json

	[
	  {
		"Cid": "<string>",
		"peer_map":
		{
		  "Peer ID String"
          {
			"cid": "<string>",
			"peer": "<string>",
			"peername": "<string>",
			"status": "<string>",
			"timestamp": "<string>",
			"error": "<string>"
		  }
	    }
      }
	]

Example: curl -X POST  http://10.10.165.11:9094/pins/QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF/sync

.. code-block:: json

	{
	  "cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
	  "peer_map": {
		"QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ": 
		{
		  "cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
		  "peer": "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "peername": "localhost.localdomain",
		  "status": "pinned",
		  "timestamp": "2019-01-07T07:46:34Z",
		  "error": ""
		},
		"QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn": 
		{
		  "cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
		  "peer": "QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
		  "peername": "localhost.localdomain",
		  "status": "pinned",
		  "timestamp": "2019-01-08T06:46:59Z",
		  "error": ""
		}
	  }
	}
	

====================
/pins/{cid}/recover
====================
Recover a CID

:METHOD:
  POST

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - cid
     - string
     - no
     - the object CID that need sync.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

.. code-block:: json

	[
	  {
		"Cid": "<string>",
		"peer_map":
		{
		  "Peer ID String"
		  {
			"cid": "<string>",
			"peer": "<string>",
			"peername": "<string>",
			"status": "<string>",
			"timestamp": "<string>",
			"error": "<string>"
		  }
		}
	  } 
	]
  
Example: curl -X POST http://10.10.165.11:9094/pins/QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF/recover

.. code-block:: json

	{
	  "cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
	  "peer_map": {
		"QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ": {
		  "cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
		  "peer": "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
		  "peername": "localhost.localdomain",
		  "status": "pinned",
		  "timestamp": "2019-01-07T07:46:34Z",
		  "error": ""
		},
		"QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn": {
		  "cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
		  "peer": "QmW8vdqrNj86rbW6RnWTUnFFD1XcHrUws3rv4GGbiNR7cn",
		  "peername": "localhost.localdomain",
		  "status": "pinned",
		  "timestamp": "2019-01-08T06:46:59Z",
		  "error": ""
		}
	  }
	}
  
  
================
/pins/recover
================
Attempt to re-pin/unpin CIDs in error state

:METHOD: 
  POST

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - arg
     - string
     - no
     - the object CID that need sync.
   * - local
     - bool
     - no
     - sync

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

.. code-block:: json

	[
	  {
		"Cid": "<string>",
		"peer_map":
		{
          "Peer ID String"
          {
			"cid": "<string>",
			"peer": "<string>",
			"peername": "<string>",
			"status": "<string>",
			"timestamp": "<string>",
			"error": "<string>"
		  }
		}
	  }
	]

Example: curl -X POST http://10.10.165.11:9094/pins/recover?local=true

.. code-block:: json

	[
	  {
		"cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
		"peer_map": {
		  "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ": {
			"cid": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
			"peer": "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
			"peername": "localhost.localdomain",
			"status": "pinned",
			"timestamp": "2019-01-07T07:46:34Z",
			"error": ""
		  }
		}
	  },
	  {
		"cid": "QmULKig5Fxrs2sC4qt9nNduucXfb92AFYQ6Hi3YRqDmrYC",
		"peer_map": {
		  "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ": {
			"cid": "QmULKig5Fxrs2sC4qt9nNduucXfb92AFYQ6Hi3YRqDmrYC",
			"peer": "QmSSwDPUL18NE6VYibm4nXfEZRxSZwV9xGDtEKRagnkWUQ",
			"peername": "localhost.localdomain",
			"status": "pinned",
			"timestamp": "2019-01-08T07:16:31Z",
			"error": ""
		  }
		}
	  }
	]
	
Example without local=true : curl -X POST http://10.10.165.11:9094/pins/recover

.. code-block:: json

	{
	  "code": 400,
	  "message": "only requests with parameter local=true are supported"
	}	


======================
/api/v0/uid/new
======================
Create a unique UID and peer ID pair from Hive cluster. The UID can be used to identify endpoints in communication，
the PeerID is a virtual IPFS peer ID.

.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 0

 * - Arguments
   - Type
   - Required
   - Description

:METHOD:
  GET/POST
   
.. list-table:: HTTP Response
 :widths: 15 10 10 30
 :header-rows: 1

 * - Argument
   - Type
   - Required
   - Description
 * - http error
   - integer
   - yes
   - error code.
 * - http body
   - Json
   - no
   - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

    {
        "UID": "<string>",
        "PeerID": "<string>"
    }

Example: curl http://10.10.165.11:9095/api/v0/uid/new

.. code-block:: json

	{
	  "UID": "uid-ef26d276-48c4-4371-b136-3c06d2d6ebab",
	  "PeerID": "Qmawxf1opQr1873WwJdxidtpKSffM7R5knZbU9idb8rjEF"
	}
	
======================
/api/v0/uid/login
======================
Log in to Hive Cluster using the UID you created earlier.

.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 1

 * - Arguments
   - Type
   - Required
   - Description
 * - uid
   - string
   - yes
   - The UID you created earlier.

:METHOD:
  GET/POST   
   
.. list-table:: HTTP Response
 :widths: 15 10 10 30
 :header-rows: 1

 * - Argument
   - Type
   - Required
   - Description
 * - http error
   - integer
   - yes
   - error code.
 * - http body
   - Json
   - no
   - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

    {
      "UID": "<string>",
      "OldUID": "<string>",
      "PeerID": "<string>"
    }

Example: curl http://127.0.0.1:9095/api/v0/uid/login?uid=uid-b7edf82e-dd51-4adc-9def-8eb06f81630b

.. code-block:: json

	{
	  "OldUID": "uid-b7edf82e-dd51-4adc-9def-8eb06f81630b",
	  "UID": "uid-1c4f7ca3-b77f-42e7-82c7-2a8fdf017623",
	  "PeerID": "QmYf9Q1SBnuzUYphpn6Bs1Zom3uz3qc12MinHseE3MyX9R"
	}

<<<<<<< HEAD
Example: curl http://127.0.0.1:9095/api/v0/uid/login?uid=x
	
.. code-block:: json

	{
	  "Message": "IPFS unsuccessful: 500: no key named x was found"
	}	

======================
/api/v0/uid/info
======================
Get the uid information from server.

.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 1

 * - Arguments
   - Type
   - Required
   - Description
 * - uid
   - string
   - yes
   - The UID you created earlier.

:METHOD:
  GET/POST   
   
.. list-table:: HTTP Response
 :widths: 15 10 10 30
 :header-rows: 1

 * - Argument
   - Type
   - Required
   - Description
 * - http error
   - integer
   - yes
   - error code.
 * - http body
   - Json
   - no
   - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

    {
      "UID": "<string>",
      "PeerID": "<string>"
    }

Example: curl http://127.0.0.1:9095/api/v0/uid/info?uid=uid-5b9745dc-7714-47ff-aa6c-4e817a39cfa6

.. code-block:: json

    {
      "UID":"uid-5b9745dc-7714-47ff-aa6c-4e817a39cfa6",
      "PeerID":"QmdSBdjRoJY7YQDWkT2XTYDToswcr8LHz1TfLpXVYhUiZK"
    }

====================
/api/v0/file/pin/add
====================
Pin objects in the cluster

:METHOD:
  GET/POST

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - arg
     - string
     - yes
     - Path to object(s) to be pinned. Required: yes.
   * - recursive
     - bool
     - yes
     - Recursively pin the object linked to by the specified object(s). Default: “true”. 
   * - progress
     - bool
     - no
     - Show progress. Default: "true"

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

.. code-block:: json

  {
  	"Pins": [
  		"<CID>"
  	],
  	"Progress": "<int>"
  }
  
Example:   curl "http://i.storswift.com:9195/api/v0/pin/add?arg=QmYZmegdpcRzVa2JQskN8trD8XUvdvd9FqSATRXpGr1DTP"
  
.. code-block:: json
  
  {
	"Pins":[
		"QmYZmegdpcRzVa2JQskN8trD8XUvdvd9FqSATRXpGr1DTP"
	]
  }

===================
/api/v0/file/pin/ls
===================
List objects that pinned to the cluster.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - arg
     - string
     - yes
     - Path to object(s) to be pinned. Required: yes.
   * - type
     - string
     - no
     - The type of pinned keys to list. Can be “direct”, “indirect”, “recursive”, or “all”. Default: “all”. 
   * - quiet
     - bool
     - no
     - Write just hashes of objects.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

.. code-block:: json

  {
  	"Keys": 
	{
  		"<Object CID>": 
		{
  		  "Type": "<string>"
  		}
  	}
  }

Example: curl "http://i.storswift.com:9195/api/v0/pin/ls"

.. code-block:: json  
    
    {
        "Keys":
        {
            "QmYZmegdpcRzVa2JQskN8trD8XUvdvd9FqSATRXpGr1DTP":
            {
                Type":"recursive"
            }
        }
    }

===================
/api/v0/file/pin/rm
===================
Remove pinned objects from the cluster.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - arg
     - string
     - yes
     - Path to object(s) to be pinned. Required: yes.
   * - recursive
     - bool
     - no
     - Recursively unpin the object linked to by the specified object(s). Default: “true”. 
     
.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.
   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200.

.. code-block:: json

  {
  	"Pins": [
  		"<Object CID>"
  	]
  }

.. code-block:: json

Example: curl http://i.storswift.com:9195/api/v0/pin/rm?arg=QmYZmegdpcRzVa2JQskN8trD8XUvdvd9FqSATRXpGr1DTP

.. code-block:: json

	{
	  "Pins": [
		"QmYZmegdpcRzVa2JQskN8trD8XUvdvd9FqSATRXpGr1DTP"
	  ]
	} 
 
=================
/api/v0/file/add
=================
Add a file or directory to cluster.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - path
     - file
     - yes
     - The path to a file to be added to the cluster.
   * - recursive
     - bool
     - no
     - Add directory paths recursively.
       Default: “false”. Required: no.
   * - hidden
     - bool
     - no
     - Include files that are hidden. Only takes effect on recursive add.
       Default: “false”.
   * - pin
     - bool
     - no
     - Pin this object when adding.
       Default: “true”.

.. list-table:: Request Body
   :widths: 80

   * - “path” is of file type. This endpoint expects a file in the body of the request as ‘multipart/form-data’.

.. list-table:: Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code. On success, the call to this endpoint will return with 200 and the following body:

   * - http body
     - Json
     - no
     - Json string is following


.. code-block:: json

  {
      "Name": "<string>",
      "Hash": "<string>",
      "Size": "<string>"
  }

Example: curl -F file=conf.py "http://i.storswift.com:9195/api/v0/add?recursive=true"

.. code-block:: json

    {
        "Name": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
        "Hash": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
        "Size": "15"
    }


=================
/api/v0/file/get
=================
Download files from the cluster.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - arg
     - string
     - yes
     - The path to the file(s) to be download from the cluster.
   * - output
     - string
     - no
     - The path where the output should be stored. Default: the endpoint current directory.
   * - archive
     - bool
     - no
     - Output a TAR archive.
       Default: “false”. Required: no.
   * - compress
     - bool
     - no
     - Compress the output with GZIP compression. 
       Default: “false”, Required: no.
   * - compression-level
     - integer
     - no
     - The level of compression (1-9). Required: no.

.. list-table:: Http Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code. On success, the call to this endpoint will return with 200 and the following body:
   * - http body
     - text/plain
     - no
     - This endpoint returns a `text/plain` response body.

Example: curl -v "http://i.storswift.com:9195/api/v0/get?arg=QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF"

::

  * About to connect() to i.storswift.com port 9195 (#0)
  *   Trying 58.246.225.150...
  * Connected to i.storswift.com (58.246.225.150) port 9195 (#0)
  > GET /api/v0/get?arg=QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF HTTP/1.1
  > User-Agent: curl/7.29.0
  > Host: i.storswift.com:9195
  > Accept: */*
  > 
  < HTTP/1.1 200 OK
  < Access-Control-Allow-Headers: X-Stream-Output, X-Chunked-Output, X-Content-Length
  < Access-Control-Expose-Headers: X-Stream-Output, X-Chunked-Output, X-Content-Length
  < Content-Type: text/plain
  < Date: Tue, 08 Jan 2019 06:59:16 GMT
  < Server: go-ipfs/0.4.18
  < Trailer: X-Stream-Error
  < Vary: Origin
  < X-Content-Length: 15
  < X-Stream-Output: 1
  < Transfer-Encoding: chunked
  < 
  QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF0000644000000000000000000000000713415045104017157 
  0ustar0000000000000000conf.py* Connection #0 to host i.storswift.com left intact

Example: curl --connect-timeout 10 -m 10 -o /dev/null -s -w %{http_code} "http://10.10.88.88:9095/api/v0/file/get?arg=QmZ2MbiCS5y7V5P5HDXus4XXaK7yJCdqWpm5u5XamZzqzL&compress=1"

::

    200


======================
/api/v0/file/ls
======================
List directory contents for Unix filesystem objects.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - path
     - file
     - yes
     - The path to the IPFS object(s) to list links from.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following


On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
  	"Arguments": {
  		"<string>": "<string>"
  	},
  	"Objects": {
  		"<string>": {
  			"Hash": "<string>",
  			"Size": "<uint64>",
  			"Type": "<string>",
  			"Links": [{
  				"Name": "<string>",
  				"Hash": "<string>",
  				"Size": "<uint64>",
  				"Type": "<string>"
  			}]
  		}
  	}
  }
  
Example: curl http://10.10.165.11:9095/api/v0/file/ls?arg=QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF

.. code-block:: json

	{
	  "Arguments": {
		"QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF"
	  },
	  "Objects": {
		"QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF": {
		  "Hash": "QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF",
		  "Size": 7,
		  "Type": "File",
		  "Links": null
		}
	  }
	}

======================
/api/v0/files/cp
======================
Copy files among clusters.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - source
     - string
     - no
     - Source object to copy.
   * - dest
     - string
     - no
     - Destination to copy object to.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }
  
  
Example: curl http://i.storswift.com:9195/api/v0/files/cp?uid=uid-6f243b6f-ab49-4b53-8d87-c411702d0754&source=/ipfs/QmcFUHo1DBpydnTSeuXszKgQqnXQCJq6Wu1BUdzEfuRahM&dest=/002

.. code-block:: json

    []

Example: curl --connect-timeout 10 -m 10 -o /dev/null -s -w %{http_code} "http://10.10.88.88:9095/api/v0/files/cp?uid=uid-e5aa1bbf-0e37-4f45-a3e3-59bd3554521e&source=/&dest=/WzLhzhvT"

::

    200


======================
/api/v0/files/flush
======================
Flush a given path’s data to cluster.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - path
     - string
     - no
     - Path to flush. Default: ‘/’.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following


On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }

Example: curl http://i.storswift.com:9195/api/v0/files/flush?uid=uid-6f243b6f-ab49-4b53-8d87-c411702d0754&path=/001

::

    200

======================
/api/v0/files/ls
======================
List directories in the private mutable namespace.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - path
     - string
     - no
     - Path to show listing for. Defaults to ‘/’.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  	{
  		"Entries": [{
  			"Name": "<string>",
  			"Type": "<int>",
  			"Size": "<int64>",
  			"Hash": "<string>"
  		}]
  	}

Example: curl http://i.storswift.com:9195/api/v0/files/ls?uid=uid-f996965a-f916-463c-8d4e-f809b590b783&path=/

.. code-block:: json

	{
	  "Entries": [
		{
		  "Name": "suxx",
		  "Type": 0,
		  "Size": 0,
		  "Hash": ""
		},
		{
		  "Name": "suxx2",
		  "Type": 0,
		  "Size": 0,
		  "Hash": ""
		}
	  ]
	}

=========================
/api/v0/files/mkdir
=========================
Create directories.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - path
     - string
     - yes
     - Path to dir to make.
   * - parents
     - bool
     - no
     - The parent directories. No error if existing, make parent directories as needed.
     
.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }

Example curl http://10.10.165.11:9095/api/v0/files/mkdir?arg=/suxx&uid=uid-1c4f7ca3-b77f-42e7-82c7-2a8fdf017623

.. code-block:: json

  []
	
Example curl http://10.10.165.11:9095/api/v0/files/mkdir?arg=/suxx  

.. code-block:: json

  {
	"Message": "error reading request: /api/v0/files/mkdir?arg=/suxx"
  }

Example: curl --connect-timeout 10 -m 10 -o /dev/null -s -w %{http_code} "http://10.10.88.88:9095/api/v0/files/mkdir?uid=48905246768"

::

    200

Example: curl --connect-timeout 10 -m 10 -o /dev/null -s -w %{http_code} "http://10.10.88.88:9095/api/v0/files/mkdir?uid=48905246768"

.. code-block:: json

    {
      "Message": "IPFS unsuccessful: 500: file already exists"
    }


======================
/api/v0/files/mv
======================
Move files.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - source
     - string
     - yes
     - Source file to move.
   * - dest
     - string
     - yes
     - Destination path for file to be moved to. 

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following


On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }

Example: curl http://10.10.165.11:9095/api/v0/files/mv  

.. code-block:: json

  {
	"Message": "error reading request: /api/v0/files/mv"
  }

Example: curl http://10.10.165.11:9095/api/v0/files/mv?uid=uid-1c4f7ca3-b77f-42e7-82c7-2a8fdf017623&source=/dir1&dest=/dir2

.. code-block:: json
  
  []
  
======================
/api/v0/files/read
======================
Read a file in a given path.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - path
     - string
     - yes
     - Path to file to be read.
   * - offset
     - integer
     - no
     - Byte offset to begin reading from.
   * - count
     - integer
     - no
     - Maximum number of bytes to read.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }

Example: curl --connect-timeout 10 -m 10 -o /dev/null -s -w %{http_code} "http://10.10.88.88:9095/api/v0/files/read?path=/nE4HT6XI&uid=uid-85b66c45-34af-4914-8084-5e296d3ac4ff"

::

    200


======================
/api/v0/files/rm
======================
Remove a file.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - path
     - string
     - yes
     - File to remove.
   * - recursive
     - bool
     - no
     - Recursively remove directories. default: false

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following


On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }

Example: curl --connect-timeout 10 -m 10 -o /dev/null -s -w %{http_code} "http://10.10.88.88:9095/api/v0/files/rm?path=/BI9qjhul&uid=uid-472ecd0e-cf11-482a-aaea-66c2c883e550"

::

    200


======================
/api/v0/files/stat
======================
Display file status.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - path
     - string
     - yes
     - Path to node to stat. 
   * - format
     - string
     - no
     - Print statistics in given format. Allowed tokens: . Conflicts with other format options. Default: Size: CumulativeSize: ChildBlocks: Type: . Default: “ Size: CumulativeSize: ChildBlocks: Type: ”. 
   * - hash
     - string
     - no
     - Print only hash. Implies ‘–format=’. Conflicts with other format options. Required: no.
   * - with-local
     - string
     - no
     - Compute the amount of the dag that is local, and if possible the total size. 
     
.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following


On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
  	"Hash": "<string>",
  	"Size": "<uint64>",
  	"CumulativeSize": "<uint64>",
  	"Blocks": "<int>",
  	"Type": "<string>",
  }

Example: curl http://10.10.165.11:9095/api/v0/files/stat?arg=/

.. code-block:: json

	{
	  "Hash": "QmaLfcyshh1PDiGSxVPspqA7BdjKX1GWp53C1GUHpD9PvF",
	  "Size": 0,
	  "CumulativeSize": 105,
	  "Blocks": 2,
	  "Type": "directory"
	}

======================
/api/v0/files/write
======================
Write to a mutable file in a given filesystem.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - uid
     - string
     - yes
     - a uid for to identify a filesystem context.
   * - path
     - string
     - yes
     - Path to write to.
   * - file
     - file
     - yes
     - Data to write.
   * - offset
     - integer
     - no
     - Byte offset to begin writing at. Default: 0.
   * - create
     - bool
     - no
     - Create the file if it does not exist. 
   * - truncate
     - bool
     - yes
     - Truncate the file to size zero before writing.
   * - count
     - int
     - no
     - Maximum number of bytes to read.

.. list-table:: Request Body
   :widths: 80

   * - Argument “data” is of file type. This endpoint expects a file in the body of the request as ‘multipart/form-data’.

.. list-table:: HTTP Response
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - http error
     - integer
     - yes
     - error code.

   * - http body
     - Json
     - no
     - Json string is following

On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }

Example: curl http://10.10.88.88:9095/api/v0/files/write?create=true&path=/HGjZpvvO&uid=uid-6e461d9e-3880-4ebb-8b10-d71c6b526485& offset=0

.. code-block:: json

    {
      "Message": "request Content-Type isn't multipart/form-data"
    }

Example: curl --connect-timeout 10 -m 10 -v -F file=@HGjZpvvO "http://10.10.88.88:9095/api/v0/files/write?create=true&path=/HGjZpvvO&uid=uid-6e461d9e-3880-4ebb-8b10-d71c6b526485&
offset=0"

::

    2019-01-28 14:16:12,295 - root - INFO - *   Trying 10.10.88.88...
    * TCP_NODELAY set
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current

                                     Dload  Upload   Total   Spent    Left  Speed
      0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to 10.10.88.88 (10.10.88.88) port 9095 (#0)
    > POST /api/v0/files/write?create=true&path=/HGjZpvvO&uid=uid-6e461d9e-3880-4ebb-8b10-d71c6b526485& HTTP/1.1
    > Host: 10.10.88.88:9095
    > User-Agent: curl/7.61.1
    > Accept: */*
    > Content-Length: 223
    > Content-Type: multipart/form-data; boundary=------------------------ad8f4d152019ec2c
    >
    } [223 bytes data]
    < HTTP/1.1 200 OK
    < Access-Control-Expose-Headers: X-Stream-Output, X-Chunked-Output, X-Content-Length
    < Content-Type: application/json
    < Server: ipfs-cluster/ipfsproxy/0.8.0+git0581a90b0ece7858198b6fc82a369cd61b65e407
    < Vary: Origin
    < Vary: Access-Control-Request-Method
    < Vary: Access-Control-Request-Headers
    < Date: Mon, 28 Jan 2019 06:16:11 GMT
    < Content-Length: 0
    <

  
======================
/api/v0/name/publish
======================
Publish user context file or directory to public.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 1

 * - Arguments
   - Type
   - Required
   - Description
 * - uid
   - string
   - yes
   - a uid for to identify a filesystem context.
 * - lifetime
   - string
   - no
   - Time duration that the record will be valid for. This accepts durations such as “300s”, “1.5h” or “2h45m”. Valid time units are “ns”, “us” (or “µs”), “ms”, “s”, “m”, “h”. Default: “24h”. Required: no.
 * - path
   - string
   - yes
   - the file object(IPFS path) to be published.
 
.. list-table:: HTTP Response
 :widths: 15 10 10 30
 :header-rows: 1

 * - Argument
   - Type
   - Required
   - Description
 * - http error
   - integer
   - yes
   - error code.
 * - http body
   - Json
   - no
   - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

    {
        "Name": "<string>"
        "Value": "<string>"
    }

Example: curl http://i.storswift.com:9195/api/v0/name/publish?arg=QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF

.. code-block:: json

	{
	  "Name": "QmdFxnZoS84UvUCiWmGX8ynvJ31iSHQgibKtb9ob6oZeoG",
	  "Value": "/ipfs/QmXbB1Ad9TWt9SqcyiG6iAW6SpKxvupv1YaUPNFyDYRPxF"
	}
    
======================
/api/v0/message/pub
======================
Publish a message to a given pubsub topic.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 1

 * - Arguments
   - Type
   - Required
   - Description
 * - topic
   - string
   - yes
   - name of topic to publish.
 * - message
   - string
   - yes
   - message or file hash .

.. list-table:: HTTP Response
 :widths: 15 10 10 30
 :header-rows: 1

 * - Argument
   - Type
   - Required
   - Description
 * - http error
   - integer
   - yes
   - error code.
 * - http body
   - Json
   - no
   - Json string is following

On success, the call to this endpoint will return with 200 or the following optional body:

.. code-block:: json

  {
    "Message": "<string>"
  }

======================
/api/v0/message/sub
======================
Subscribe to message to a given topic.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 1

 * - Arguments
   - Type
   - Required
   - Description
 * - topic
   - string
   - yes
   - name of topic to be subscribed.

.. list-table:: HTTP Response
 :widths: 15 10 10 30
 :header-rows: 1

 * - Argument
   - Type
   - Required
   - Description
 * - http error
   - integer
   - yes
   - error code.
 * - http body
   - Json
   - no
   - Json string is following

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

    {
      "Message": {
        "From": [
          "<uint8>"
        ],
        "Data": [
          "<uint8>"
        ],
        "Seqno": [
          "<uint8>"
        ],
        "TopicIDs": [
          "<string>"
        ],
        "XXX_unrecognized": [
          "<uint8>"
        ]
      }
    }

=================
/version
=================

Show cluster version information.

:METHOD:
  GET/POST
  
.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 1

 * - Arguments
   - Type
   - Required
   - Description
 * - number
   - bool
   - no
   - Only show the version number.
 * - commit
   - bool
   - no
   - Only show the commit version number.
 * - repo
   - bool
   - no
   - Only show the repo version number.
 * - all
   - bool
   - no
   - Only show all version strings.

.. list-table:: HTTP Response
 :widths: 15 10 10 30
 :header-rows: 1

 * - Argument
   - Type
   - Required
   - Description
 * - http error
   - integer
   - yes
   - error code.
 * - http body
   - Json
   - no
   - Json string is following
    
On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

    {
        "Version": "<string>",
        "Commit": "<string>",
        "Repo": "<string>",
        "System": "<string>",
    }

Example:	curl http://10.10.165.11:9094/version

.. code-block:: json

	{
	  "Version": "0.7.0+git13a161f056792cfb50c1ef53b6688716a211381f"
	}