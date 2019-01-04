Cluster Commands & API Reference
========================================
Cluster Commands & API Reference

.. toctree::
   :maxdepth: 2

.. section-numbering::
   :depth: 5
   :start: 0

HTTP API
========

The typical IPFS peer is a resource hunger program. If you install IPFS daemon to your mobile device, it will take up resources and slow down your device.
We are creating Hive project, which is a low resources consumption scenario.

Hive maintains a big IPFS pinset for sharing. It can provide numerous virtual IPFS peers with only one run a real IPFS peer.
Hive project distils from IPFS Cluster, but it will have many differences with the IPFS Cluster.


:file API vs files API:

  In the endpoints APIs, which have two type of file API: file API and files API.

- file APIs are used to interact with object.
- files APIs are used to interact with virtual file system.
- In files APIs context, we must keep a uid for to distinguish different filesystems.

Index
-----

=================================
`Cluster Server Managment`
=================================

* :ref:`/cluster/id`
* :ref:`/cluster/peers/ls`
* :ref:`/cluster/peers/add`
* :ref:`/cluster/peers/rm`
* :ref:`/cluster/status`
* :ref:`/cluster/sync`
* :ref:`/cluster/recover`

=======================
`Endpoints APIs`
=======================

* :ref:`/api/v0/uid/new`
* :ref:`/api/v0/uid/clone`
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
/cluster/id
===================
Show the cluster peers and its daemon information

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
  	"peers": [
  		"/ip4/104.236.176.52/tcp/4001",
  		"/ip4/104.236.176.52/tcp/4001"
  	]
  }

===================
/cluster/peers/ls
===================
List the cluster servers with open connections.

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
  	"data": [
  	  "/ip4/104.236.176.52/tcp/4001",
  	  "/ip4/104.236.176.52/tcp/4001"
  	]
  }

===================
/cluster/peers/add
===================
Add a node to the cluster

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - NodeId
     - string
     - yes
     - a string format[##TODO##] of node id.

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

===================
/cluster/peers/rm
===================
Remove a node from the cluster.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
   * - NodeId
     - string
     - yes
     - a string format[##TODO##] of node id.

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

===================
/cluster/status
===================
list current status of pins in the cluster.

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

  {
  	"Cid": "<string>",
  	"PinStatus": {
  		"Ok": "<bool>",
  		"BadNodes": [{
  			"Cid": "<string>",
  			"Err": "<string>"
  		}]
  	}
  } 

=============
/cluster/sync
=============
Re-sync seen status against status reported.

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

  {
  	"Cid": "<string>",
  	"PinStatus": {
  		"Ok": "<bool>",
  		"BadNodes": [{
  			"Cid": "<string>",
  			"Err": "<string>"
  		}]
  	}
  }

================
/cluster/recover
================
Attempt to re-pin/unpin CIDs in error state

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

  {
  	"Cid": "<string>",
  	"PinStatus": {
  		"Ok": "<bool>",
  		"BadNodes": [{
  			"Cid": "<string>",
  			"Err": "<string>"
  		}]
  	}
  }


======================
/api/v0/uid/new
======================
Create a unique UID from Hive cluster. UID can be used to distinguish different endpoints.

.. list-table:: Arguments
 :widths: 15 10 10 30
 :header-rows: 1

 * - Arguments
   - Type
   - Required
   - Description
 * - name
   - string
   - yes
   - name of UID to create.

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
        "PriKey": "<string>",
        "PubKey": "<string>"
    }

======================
/api/v0/uid/clone
======================
Create a new UID from the old one.

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
   - old uid to be used to clone.
 * - arg
   - string
   - yes
   - new uid to create.

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
      "Was": "<string>",
      "Now": "<string>",
      "Id": "<string>",
      "Overwrite": "<bool>"
    }

====================
/api/v0/file/pin/add
====================
Pin objects in the cluster

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

===================
/api/v0/file/pin/ls
===================
List objects that pinned to the cluster.

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
  	"Keys": {
  		"<Object CID>": {
  			"Type": "<string>"
  		}
  	}
  }

===================
/api/v0/file/pin/rm
===================
Remove pinned objects from the cluster.

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

=================
/api/v0/file/add
=================
Add a file or directory to cluster.

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
      "Bytes": "<int64>",
      "Size": "<string>"
  }


=================
/api/v0/file/get
=================
Download files from the cluster.

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

======================
/api/v0/file/ls
======================
List directory contents for Unix filesystem objects.

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

======================
/api/v0/files/cp
======================
Copy files among clusters.

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

On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/files/flush
======================
Flush a given path’s data to cluster.

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


On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/files/ls
======================
List directories in the private mutable namespace.

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

=========================
/api/v0/files/mkdir
=========================
Create directories.

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
     - string
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

On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/files/mv
======================
Move files.

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


On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/files/read
======================
Read a file in a given path.

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

On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/files/rm
======================
Remove a file.

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
     - yes
     -  Recursively remove directories. 

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


On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/files/stat
======================
Display file status.

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
  	"WithLocality": "<bool>",
  	"Local": "<bool>",
  	"SizeLocal": "<uint64>"
  }

======================
/api/v0/files/write
======================
Write to a mutable file in a given filesystem.

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

On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/name/publish
======================
Publish user context file or directory to public.

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
 * - ipfs file object
   - string
   - yes
   - the file object to be published.
 
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

======================
/api/v0/message/pub
======================
Publish a message to a given pubsub topic.

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

On success, the call to this endpoint will return with 200 and the following optional body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/api/v0/message/sub
======================
Subscribe to message to a given topic.

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
