=================
Ansible Discovery
=================



In this lab you will use the command line from your computer or from the one provided.

*************************************
Export your AWS API Key ID and Secret
*************************************

Open the terminal and enter paste the following commands. Add your Access Key ID and your Secret Access Key:

.. code-block:: console

    export AWS_ACCESS_KEY_ID=your-access-key-here
    export AWS_SECRET_ACCESS_KEY=your-secret-key-here


******************************
Deploy the lab using Terraform
******************************

.. code-block:: console

    terraform init

.. code-block:: console

    terraform plan

.. code-block:: console

    terraform apply

In the output get the firewall IP address.

**************************
Create your first playbook
**************************

Then go to the following directory to create your first terraform file.

.. code-block:: console

    cd ~/utd-automation/ansible

And create a :guilabel:`playbook.yml` file:

.. code-block:: console

    code playbook.yml
    # or
    subl playbook.yml


Add the provider
================


.. code-block:: ansible

    

You can find more details on `AWS provider here <https://registry.terraform.io/providers/hashicorp/aws/latest/docs>`_

Add a first instance
====================

.. code-block:: ansible

    

