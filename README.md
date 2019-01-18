Elastos.NET.Hive.DevDocs
=============================

## Introduction

Hive is the decentralized storage layer of Elastos.  Hive is designed for large scale storage systems, based on the IPFS and IPFS Cluster projects. The purpose of this repository is to collect all documents over the whole entire developing process of Hive project.

The layout of directory structure is defined as below:

* manual: Introduce how to use Elastos Hive. There are API references and deployment guides for the Hive Project. 
* tech: There are some design documents and technical references for the Hive project.

## Directories

### manual

This directory contains several types of important documents:

* API references: HTTP API introduction for Hive storage system. These APIs are based on IPFS HTTP API and IPFS Cluster API.
* Deployment Documents: Introduce how to deploy a Hive cluster.

In order to build the manual, please install Sphinx at first. It can be got from the official site:  https://www.sphinx-doc.org. The version of Sphinx  should be 1.8 or higher.

Please enter the directory of manual, and run this command to create the HTML format document.
```shell
$ make html
```

### tech

Some design documents and technical materials for Hive are included in this directory. From the documents, we can understand the architecture of Hive and know how it works.
