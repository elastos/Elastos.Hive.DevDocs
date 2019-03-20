:orphan:

.. toctree::
   :maxdepth: 2

.. section-numbering::
   :depth: 5
   :start: 0

Usage sample of the files API
==============================

To make convenience for users to store UnixFS format files. In the Hive cluster servers, users can create virtual home directories for themselves.

Users can log in one of Hive cluster servers by 'uid' to store files, and after a while, log into another Hive server by 'uid' to modify them.

To use UnixFS feature, users need to use the 'files API'. Please follow the below procedures to use 'files API'.

=================
Index of Content
=================

* :ref:`Calling processes`
* :ref:`procedure for new user`
* :ref:`procedure for existing user`

* :ref:`Usage samples`
* :ref:`sample for new user`
* :ref:`sample for existing user`

=================================
Calling processes
=================================

-----------------------
procedure for new user
-----------------------

'uid' that is the Hive clusters distinguish different users data is for. for a new user, which has to generate a new uid in the Hive cluster before they use 'files' API.

.. graphviz::

    digraph {
      "(generate uid)\n uid/new" -> 
      "(invoke files APIs) \n files/write?uid=... \nfiles/read?uid=... \n ..." ->
      "(get and save the home directory hash)[2] \n files/stat?uid=...&path=/" ->
      "(logout) \n ...";
    }

----------------------------
procedure for existing user
----------------------------

For existing users, before they use the 'files API', they need to invoke 'uid/login' API, This API requires to provide two parameters: uid and home HASH. Otherwise, his/her home directory should be scratched state.

.. graphviz::

    digraph {
      "(login) \n uid/login?uid=...&hash=..." -> 
      "(invoke files APIs) \n files/write?uid=... \nfiles/read?uid=... \n ..." ->
      "(get and save the home directory hash)[2] \n files/stat?uid=...&path=/" ->
      "(logout) \n ...";
    }

==============================
Usage samples
==============================

---------------------
sample for new user
---------------------

.. code-block:: bash

   ## generate a new uid
   $ curl http://HOST:9095/api/v0/uid/new
   {
	  "UID": "uid-ef26d276-48c4-4371-b136-3c06d2d6ebab",
	  "PeerID": "Qmawxf1opQr1873WwJdxidtpKSffM7R5knZbU9idb8rjEF"
   }

   ## get a uid is 'uid-ef26d276-48c4-4371-b136-3c06d2d6ebab'
   #  upload testfile to user home directory, and named it "/test"
   $curl -F "file=@testfile"  '''http://HOST:9095/api/v0/files/write?uid=uid-ef26d276-48c4-4371-b136-3c06d2d6ebab&path=/test&create=true'''

   ## do read/write/mkdir actions
   $ ... ... 

   ## now, user want to leave this server. get and record the home hash.
   $curl '''http://HOST:9095/api/v0/files/stat?uid=uid-ef26d276-48c4-4371-b136-3c06d2d6ebab&path=/'''
   {"Hash":"QmcueZxPtS728RVkHXHsitVWGsFnMwJEpbQcEqrjfs7cVL","Size":0,"CumulativeSize":10141,"Blocks":1,"Type":"directory","WithLocality":false,"Local":false,"SizeLocal":0}

   ## got the home hash is 'QmcueZxPtS728RVkHXHsitVWGsFnMwJEpbQcEqrjfs7cVL', and save it 
   export HOME_HASH=QmcueZxPtS728RVkHXHsitVWGsFnMwJEpbQcEqrjfs7cVL


-------------------------
sample for existing user
-------------------------

.. code-block:: bash

   export HOME_HASH=QmcueZxPtS728RVkHXHsitVWGsFnMwJEpbQcEqrjfs7cVL

   ## logging in with the existing uid 'uid-ef26d276-48c4-4371-b136-3c06d2d6ebab' and recover previous data to current host
   ## Please note: hash string MUST add prefix '/ipfs/', which explicitly set the source path is s IPFS object.
   $curl '''http://HOST:9095/api/v0/uid/login?uid=uid-ef26d276-48c4-4371-b136-3c06d2d6ebab&hash=/ipfs/QmcueZxPtS728RVkHXHsitVWGsFnMwJEpbQcEqrjfs7cVL'''

   ## do read/write/mkdir actions
   $ ... ... 

   ## Now, the user wants to leave this server. Get and record the home hash.
   $curl '''http://HOST:9095/api/v0/files/stat?uid=uid-ef26d276-48c4-4371-b136-3c06d2d6ebab&path=/'''
   {"Hash":"QmanREuWoDVuHtEgdW44F8ytoP6SDMekCH3afHCiwYEsV8","Size":0,"CumulativeSize":10141,"Blocks":1,"Type":"directory","WithLocality":false,"Local":false,"SizeLocal":0}

   ## got new the home hash is 'QmanREuWoDVuHtEgdW44F8ytoP6SDMekCH3afHCiwYEsV8', and save it.
   export HOME_HASH=QmanREuWoDVuHtEgdW44F8ytoP6SDMekCH3afHCiwYEsV8
