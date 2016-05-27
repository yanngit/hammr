.. Copyright (c) 2007-2016 UShareSoft, All rights reserved

.. _builder-outscale:

Outscale
========

Builder type: ``outscale``
Require Cloud Account: Yes
`outscale.com <http://outscale.com>`_

The Outscale builder provides information for building and publishing the machine image for Outscale cloud platform. The Outscale builder requires cloud account information to upload and register the machine image to Outscale cloud.

The Outscale builder section has the following definition:

.. code-block:: javascript

	{
	  "builders": [
	    {
	      "type": "outscale",
	      ...the rest of the definition goes here.
	    }
	  ]
	}

Building a Machine Image
------------------------

For building an image, the valid keys are:

* ``installation`` (optional): an object providing low-level installation or first boot options. These override any installation options in the :ref:`template-stack` section. The following valid keys for installation are:
	* ``diskSize`` (mandatory): an integer providing the disk size of the machine image to create. Note, this overrides any disk size information in the stack. If the machine image is to be stored in Amazon S3, the maximum disk size is 10GB, otherwise if this is an EBS-backed machine image the maximum disk size is 1TB.

Publishing a Machine Image
--------------------------

To publish an image, the valid keys are:

* ``account`` (mandatory): an object providing the AWS cloud account information required to publish the built machine image.
* ``zone`` (mandatory): a string providing the region where to publish the machine image. See below for valid regions.
* ``type`` (mandatory): the builder type: ``outscale``

Valid Regions
-------------

The following regions are supported:

* ``ap-northeast-1``: Asia Pacific (Tokyo) Region
* ``ap-southeast-1``: Asia Pacific (Singapore) Region
* ``eu-west-1``: EU (Ireland) Region
* ``sa-east-1``: South America (Sao Paulo) Region
* ``us-east-1``: US East (North Virginia) Region
* ``us-west-1``: US West (North california) Region
* ``us-west-2``: US West (Oregon) Region

Outscale Cloud Account
----------------------

Key: ``account``
Used to authenticate to Outscale cloud platform.

The Outscale cloud account has the following valid keys:

* ``name`` (mandatory): a string providing the name of the cloud account. This name can be used in a ``builder`` section to reference the rest of the cloud account information.
* ``secretAccessKey`` (mandatory): A string providing your Outscale secret access key
* ``accessKey`` (mandatory): A string providing your Outscale access key id
* ``type`` (mandatory): a string providing the cloud account type: ``outscale``.

.. note:: In the case where ``name`` or ``file`` is used to reference a cloud account, all the other keys are no longer required in the account definition for the builder.

Example
-------

The following example shows an amazon builder with all the information to build and publish a machine image to Amazon EC2.

.. code-block:: json

	{
	  "builders": [
	    {
	      "type": "outscale",
	      "account": {
	        "type": "outscale",
	        "name": "My Outscale Account",        
	        "accessKey": "789456123ajdiewjd",
	        "secretAccessKey": "ks30hPeH1xWqilJ04"
	      },
	      "installation": {
	        "diskSize": 10240
	      },
	      "zone": "eu-west-2",
	      "description": "centos-template"
	    }
	  ]
	}

Referencing the Cloud Account
-----------------------------

To help with security, the cloud account information can be referenced by the builder section. This example is the same as the previous example but with the account information in another file. Create a json file ``outscale-account.json``.

.. code-block:: json

	{
	  "accounts": [
	    {
	      "type": "outscale",
	      "name": "My Outscale Account",        
	      "accessKey": "789456123ajdiewjd",
	      "secretAccessKey": "ks30hPeH1xWqilJ04"
	    }
	  ]
	}

The builder section can either reference by using ``file`` or ``name``.

Reference by file:

.. code-block:: json

	{
	  "builders": [
	    {
	      "type": "outscale",
	      "account": {
	        "file": "/home/joris/accounts/outscale-account.json"
	      },
	      "installation": {
	        "diskSize": 10240
	      },
	      "region": "eu-west-2",
	      "s3bucket": "centos-template"
	    }
	  ]
	}

Reference by name, note the cloud account must already be created by using ``account create``.

.. code-block:: json

	{
	  "builders": [
	    {
	      "type": "outscale",
	      "account": {
	        "name": "My Outscale Account"
	      },
	      "installation": {
	        "diskSize": 10240
	      },
	      "region": "eu-west-2",
	      "s3bucket": "centos-template"
	    }
	  ]
	}