AWS Automation Lab Guide
========================

.. image:: palo-alto-networks-logo.png
   :align: center

Welcome
-------

Welcome to the AWS Security Automation Lab!

In this lab we will be learning how to automate the deployment and configuration of infrastructure supporting a web application within AWS public cloud provider.  A key element of this infrastructure is the Palo Alto Networks NGFW.  

Following the deployment, we will automate the configuration of the firewall to support and protect protect the web application through a VPC Transit  

Lastly, we will ensure that the firewall is able to respond effectively to changes made to the application infrastructure.  You will deploying your application in Amazon Web Services (AWS).  


Objective
---------
The objective of this workshop is to deploy and secure a `WordPress <https://wordpress.org>`_ content management system in AWS.  This web application will be supported by an `Apache <https://httpd.apache.org>`_ web server and a database server residing in two separate subnets with different AZ.  

As part of our infrastructure deployment, a VM-Series NGFW in a specific VPC 'VPC-Transit' will be inserted between Internet and the web subnet, and the database subnet.  However, we will need to configure this virtual firewall to support its network environment and the applications it will be protecting.


Learning Outcomes
-----------------
- Understand the various methods for automating the deployment of Palo Alto Networks NGFW instances in cloud environments
- Learn to use industry-leading configuration management automation tools to implement changes to PAN-OS devices
- Learn how the Palo Alto Networks NGFW can automatically respond to changes in the network environment


============
Introduction
============

Automation Overview
-------------------

This training workshop provides hands-on exposure to the three primary
categories of infrastructure automation activities: **Build**, **Run**, and
**Respond**.

.. image:: build-run-respond.png

Build
    Build automation is the means by which a set of infrastructure elements are
    declared, instantiated, and orchestrated using automation tools and
    infrastructure APIs.  The result is a set of deployed infrastructure
    elements that are in production (or production-ready) with a "day one"
    configuration.

Run
    Run automation encompasses any API-based configuration management actions
    that occur once the infrastructure element is in production.  These are
    primarily scheduled changes that are made to support new requirements.  The
    input to these changes is manually defined in a variety of formats such as
    YAML, JSON, XML, etc.

Respond
    Response automation includes any automated actions that are triggered by an
    event.  These may be operational events such as changes to the
    infrastructure or security events such as of a new threat. Response actions
    are defined in advance but only initiated when a event matching its trigger
    criteria occurs.


Lab Topology
------------

.. figure:: topology.png

+--------------+--------------+-------------+
| Subnet       | Address      | Interface   |
+==============+==============+=============+
| ManagementFW1| 10.5.0.0/24  | Management  |
+--------------+--------------+-------------+
| ManagementFW2| 10.5.45.0/24 | Management  |
+--------------+--------------+-------------+
| WEB1         | 10.4.2.0/24  | web - 1     |
+--------------+--------------+-------------+
| WEB2         | 10.4.46.0/24 | web - 2     |
+--------------+--------------+-------------+
| SQL1         | 10.4.1.0/24  | sql - 1     |
+--------------+--------------+-------------+
| SQL2         | 10.4.1.0/24  | sql - 1     |
+--------------+--------------+-------------+
| Untrust FW1  | 10.5.2.0/24  | Unt - 1     |
+--------------+--------------+-------------+
| Untrust FW2  | 10.5.47.0/24 | Unt - 2     |
+--------------+--------------+-------------+
| trust FW1    | 10.5.1.0/24  | Tru - 1     |
+--------------+--------------+-------------+
| trust FW2    | 10.5.46.0/24 | Tru - 2     |
+--------------+--------------+-------------+

Lab Components
--------------

AWS Amazon Web Service
    Amazon Web Services is a subsidiary of Amazon that provides on-demand cloud
    computing platforms to individuals, companies and governments, on a metered
    pay-as-you-go basis.
    This lab is launched using AWS Account, which is gived by your prefered SE or you can use your own account (better)
    One login and password provides access to cloud-based lab environments. the account
    is temporary in the cloud provider in order to deploy and access the cloud infrastructure.

Visual Studio Code
    This lab can be perform with any text editor, but we advise to use visual studio code with ansible & terraform plugins
    Visual Studio code can be download under the MIT license without telemetry, `you can install it on your endpoint <https://vscodium.com/>`_
    Dont hesitate to scroll on vscodium webpage to understand how install it in a few second.

Launchpad VM
    An Ubuntu virtual machine can be deployed in a cloud environment for you to use as your primary workspace for the lab activities
    This VM will be provisioned with all the tools and libraries necessary for deploying and managing infrastructure in the cloud provider.
    But we advise to install on your endpoint the different tools to be autonomous after the lab.

Hashicorp Terraform
    Each cloud provider offers a mechanism that allow you to define a set of
    infrastructure element or services and orchestrate their instantiation.
    However, these tools and templates are specific to each cloud provider.
    We will be using Terraform to perform this function as it provides a
    common set of capabilities and a template formats acroos all cloud
    providers.

Red Hat Ansible
    Whereas Terraform excels at orchestrating deployment activities, Ansible is
    more effective at automating configuration management tasks.  We will be
    using both Terraform and Ansible to make configuration changes to the
    VM-Series firewall in order to illustrate their different capabilities.


.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: getting started

    00-getting-started/01-requirements
    00-getting-started/02-setup
    00-getting-started/03-aws-account

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: cloud discovery

    01-discovery/1-gcp-discovery/gcp-disc
    01-discovery/2-aws-discovery/aws-disc
    01-discovery/3-azure-discovery/azure-dis
    01-discovery/4-terraform-discovery/terraform-disc

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: 1st pan automation

    02-first-pan-automation/1-first-terraform/first-terraform

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: Automation Journey

    03-automation-journey/1-build/deploy
    03-automation-journey/2-run//run-terraform
    03-automation-journey/3-respond/respond
    03-automation-journey/4-cleanup/cleanup

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: Cloud Security

    04-cloud-security/1-deploy/deploy-aws
    04-cloud-security/2-prisma/prisma-cloud

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: summary

    05-summary/summary
    05-summary/comparison
    05-summary/moreinfo

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: appendix

    06-appendix/terraform-commit
    06-appendix/background-terraform
    06-appendix/background-ansible
