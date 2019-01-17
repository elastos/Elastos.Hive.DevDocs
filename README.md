Elastos Hive Dev Document
=======================

### Overview:
Elastos Hive is a basic service infrastructure that provide storage capabilities to dApps with decentralized characteristics, which leveraged standard IPFS/Cluster open source projects with some necessary refits.

The main purpose of this repository to collect certain documents or developer guides about Hive storage and make a shortcut reference portal for community users.

### Layout
The conent layout of this repository is structured as below:

* manual: Http Restful APIs for Hive node/cluster, and guides about **HOWTO** deploy Hive node/cluster.
* tech: Under hook technical documents.

### Build API and Deployment Document
Currently, building **manual** document can only work on GNU/Linux hosts. MacOS has a bug issue with python, which would cause build process failure.  The whole build process has been verified on Ubuntu-16.04.

To build document, download sources using Git at first:

```shell
$ git clone https://github.com/elastos/Elastos.NET.Hive.DevDocs
```

####1. Install Pre-Requirements
And you also need to install necessary packages with following commands  before building:

```shell
$ sudo apt-get update
$ sudo apt-get install python-sphinx graphviz
$ curl -L -o /tmp/get-pip.py https://bootstrap.pypa.io/get-pip.py
$ sudo python /tmp/get-pip.py
$ sudo pip install breathe
```

####2. Build

Then, **cd** to **manul** subdirectory to make build process as below:

```shell
$ cd YOUR-PATH-DEVDOCS/manual
$ make html
```

After building with success,  an HTML-based document under directory **build/html** should be generated.

```shell
$ ls build
doctrees  html
$ ls build/html
_sources	_static		api.html	deployment.html	genindex.html	index.html	objects.inv	search.html	searchindex.js
```

Now, use browser, for example,  use "FireFox" to open **index.html** file under **html** directory.

#### 3. Usage

There are two categories of content in document by now:

* **API References**: HTTP API introduction for Hive storage system. These APIs are based on IPFS HTTP API and IPFS Cluster API
* **Deployment Documents**:  Introduction about how to deploy a Hive cluster

### Technical Documents:

Some design documents and technical materials for Hive are included in this directory. From the documents, we can understand the architecture of Hive and know how it works.

###Contribution

We welcome contributions to enrich docouments about Elastos Hive Projects.

### License

This project is licensed under the terms of the [MIT license](https://github.com/elastos/Elastos.NET.Carrier.Native.SDK/blob/master/LICENSE).