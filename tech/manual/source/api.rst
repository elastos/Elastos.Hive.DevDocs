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

##TODO## Introduction 

Index
-----

=================================
`Cluster Server Managment`
=================================

* :ref:`/cluster/node/list`
* :ref:`/cluster/node/add`
* :ref:`/cluster/node/rm`

=======================
`Endpoints APIs`
=======================

* :ref:`/file/add`
* :ref:`/file/get`
* :ref:`/file/ls`
* :ref:`/files/chcid`
* :ref:`/files/cp`
* :ref:`/files/flush`
* :ref:`/files/ls`
* :ref:`/files/mkdir`
* :ref:`/files/mv`
* :ref:`/files/read`
* :ref:`/files/rm`
* :ref:`/files/stat`
* :ref:`/files/write`
* :ref:`/filestore/dups`
* :ref:`/filestore/ls`
* :ref:`/filestore/verify`
* :ref:`/key/gen`
* :ref:`/key/list`
* :ref:`/key/rename`
* :ref:`/key/rm` 
* :ref:`/certificate/token/new`
* :ref:`/certificate/token/pubKey`
* :ref:`/certificate/token/privateKey`
* :ref:`/version`

Cluster Server Managment
------------------------
     
===================
/cluster/node/list
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
/cluster/node/add
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
/cluster/node/rm
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

=================
/file/add
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
/file/get
=================

.. list-table:: Arguments
   :widths: 15 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Description
   * - Albatross
     - 2.99
     - On a stick!
   * - Crunchy Frog
     - 1.49
     - If we took the bones out, it wouldn't be
       crunchy, now would it?
   * - Gannet Ripple
     - 1.99
     - On a stick!

.. list-table:: Output
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - error
     - integer
     - yes
     - error code.  

======================
/file/ls
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
/files/chcid
======================
Change the cid version or hash function of the root node of a given path.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Required
     - Description
   * - path
     - string
     - no
     - Path to change. Default: ‘/’.

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
    "desciption": "<string>"
  }

======================
/files/cp
======================
Copy files among clusters.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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


On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/files/flush
======================
Flush a given path’s data to cluster.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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


On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/files/ls
======================
List directories in the private mutable namespace.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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
  	"body": {
  		"Entries": [{
  			"Name": "<string>",
  			"Type": "<int>",
  			"Size": "<int64>",
  			"Hash": "<string>"
  		}]
  	},
  	"desciption": "<string>"
  }

======================
/files/mkdir
======================
Create directories.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/files/mv
======================
Move files.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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


On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/files/read
======================
Read a file in a given path.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/files/rm
======================
Remove a file.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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


On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/files/stat
======================
Display file status.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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
/files/write
======================
Write to a mutable file in a given filesystem.

.. list-table:: Arguments
   :widths: 15 10 10 30
   :header-rows: 1

   * - Arguments
     - Type
     - Required
     - Description
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

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

  {
    "desciption": "<string>"
  }

======================
/filestore/dups
======================
List blocks that are both in the filestore and standard block storage.

.. list-table:: Arguments
   :widths: 80
   :header-rows: 0

   * - This endpoint takes no arguments.

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

On success, the call to this endpoint will return with 200 and the following body:

.. code-block:: json

 {
 	"data": {
 		"Ref": "<string>",
 		"Err": "<string>"
 	},
 	"desciption": "<string>"
 }
 
======================
/filestore/ls
======================
List objects in filestore.

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
     - Cid of objects to list.

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
  	"data": {
  		"Status": "<int32>",
  		"ErrorMsg": "<string>",
  		"Key": {
  			"/": "<cid-string>"
  		},
  		"FilePath": "<string>",
  		"Offset": "<uint64>",
  		"Size": "<uint64>"
  	},
  	"desciption": "<string>"
  }

======================
/filestore/verify
======================
Verify objects in filestore.

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
     - Cid of objects to verify. 
   * - file-order
     - bool
     - no
     - verify the objects based on the order of the backing file. 

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
  	"data": {
  		"Status": "<int32>",
  		"ErrorMsg": "<string>",
  		"Key": {
  			"/": "<cid-string>"
  		},
  		"FilePath": "<string>",
  		"Offset": "<uint64>",
  		"Size": "<uint64>"
  	},
  	"desciption": "<string>"
  }
  
======================
/key/gen
======================
Create a new keypair

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
   - name of key to create.
 * - type
   - string
   - no
   - type of the key to create [rsa, ed25519]. 
 * - size
   - int
   - no
   - size of the key to generate. 

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
    	"data": {
            "Name": "<string>",
            "Id": "<string>"
    	},
    	"desciption": "<string>"
    }

======================
/key/list
======================
List all keypairs in the given peer 

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
   - name of key to create.
 * - type
   - string
   - no
   - type of the key to create [rsa, ed25519]. 
 * - size
   - int
   - no
   - size of the key to generate. 

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
    	"data": {
            "Name": "<string>",
            "Id": "<string>"
    	},
    	"desciption": "<string>"
    }

======================
/key/rename
======================
Rename a keypair

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
   - name of key to create.
 * - newName
   - string
   - yes
   - name of key to create.
 * - force
   - bool
   - no
   - Allow to overwrite an existing key. 

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
    	"data": {
    		"Was": "<string>",
    		"Now": "<string>",
    		"Id": "<string>",
    		"Overwrite": "<bool>"
    	},
    	"desciption": "<string>"
    }

======================
/key/rm
======================
remove a keypair

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
   - name of key to remove.

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
    	"data": {
    		"Name": "<string>",
    		"Id": "<string>"
    	},
    	"desciption": "<string>"
    }
    
=========================
/certificate/token/new
=========================

.. list-table:: Arguments
   :widths: 15 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Description
   * - Albatross
     - 2.99
     - On a stick!
   * - Crunchy Frog
     - 1.49
     - If we took the bones out, it wouldn't be
       crunchy, now would it?
   * - Gannet Ripple
     - 1.99
     - On a stick!
     
=========================
/certificate/token/pubKey
=========================

.. list-table:: Arguments
   :widths: 15 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Description
   * - Albatross
     - 2.99
     - On a stick!
   * - Crunchy Frog
     - 1.49
     - If we took the bones out, it wouldn't be
       crunchy, now would it?
   * - Gannet Ripple
     - 1.99
     - On a stick!

=============================
/certificate/token/privateKey
=============================

.. list-table:: Arguments
   :widths: 15 10 30
   :header-rows: 1

   * - Argument
     - Type
     - Description
   * - Albatross
     - 2.99
     - On a stick!
   * - Crunchy Frog
     - 1.49
     - If we took the bones out, it wouldn't be
       crunchy, now would it?
   * - Gannet Ripple
     - 1.99
     - On a stick!

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
    	"data": {
            "Version": "<string>",
            "Commit": "<string>",
            "Repo": "<string>",
            "System": "<string>",
        },
    	"desciption": "<string>"
    }
    