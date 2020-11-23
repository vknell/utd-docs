===================
Terraform Discovery
===================

HashiCorp's Terraform is a tool to help you to define infrastructure either locally, in a private cloud or to different public cloud providers. Infrastructure-as-a-code or IaC allows to manage your infrastructure configuration as you would do for regular code. You can have one source of truth and to use every automation concepts that were introduced by software developpers (CI/CD, git...), track changes, prevent accidental misconfiguration and the ability to deploy/redeploy and scale at will.

Terraform is an immutable and declarative language. Meaning that you can describe the desired state and Terraform will figure out how to achieve this state.

In this lab you will use the command line from your computer or from the one provided.

*************************************
Export your AWS API Key ID and Secret
*************************************

Open the terminal and enter paste the following commands. Add your Access Key ID and your Secret Access Key:

.. code-block:: console

    export AWS_ACCESS_KEY_ID=your-access-key-here
    export AWS_SECRET_ACCESS_KEY=your-secret-key-here


*******************
Create your main.tf
*******************

Then go to the following directory to create your first terraform file.

.. code-block:: console

    cd ~/utd-automation/terraform

And create a :guilabel:`main.tf` file:

.. code-block:: console

    code main.tf
    # or
    subl main.tf


Add the provider
================

First you need to declare a provider in Terraform. The provider state what type of deployment you want. Is can be a solution like Palo Alto Networks' NGFW or Prisma Cloud, Kubernetes, or a public Cloud provider like AWS, GCP and Azure. HashiCorp provides a list of provider and third party providers can developp their own integrations.
You can find a list of providers on `HashiCorp website <https://registry.terraform.io/browse/providers>`_.

For AWS we will declare it as a provider and select the region (eu-west-3 or Paris here):

.. code-block:: terraform

    provider "aws" {
      region = "eu-west-3"
    }

You can find more details on `AWS provider here <https://registry.terraform.io/providers/hashicorp/aws/latest/docs>`_

Add a first instance
====================

.. code-block:: terraform

    resource "aws_instance" "aws_instance_discovery" {
      ami           = "ami-078db6d55a16afc82"
      instance_type = "t2.micro"
    }

https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance

Use terraform to deploy your instance
=====================================

Terraform will check the provider or providers used and download all the up to date resources to be able to deploy 

.. code-block:: console

    terraform init

.. code-block:: console

    terraform plan

You can review the operations about to be performed by Terrafom.

.. code-block:: console

    terraform apply

Review your changes and when asked, enter :guilabel:`yes`

Use terraform to deploy your instance
=====================================

Some changes wont affect your infrastructure and some will require an instance to be destroyed and re-created.

For example adding a name wont change your instance, add this tag to your resource :guilabel:`aws_instance:guilabel:`.

.. code-block:: terraform

    tags = {
      Name = "tf-disc-web1"
    }

Your resource should look like this:

.. code-block:: terraform

    resource "aws_instance" "web1" {
      ami           = "ami-078db6d55a16afc82"
      instance_type = "t2.micro"
    
      tags = {
        Name = "tf-disc-web1"
      }
    }

Enter the terraform apply command:

.. code-block:: console

    terraform apply

Enter :guilabel:`yes` in the terminal when asked.

Adding or changing the content of the user-data field will require your instance to be destroyed. A new one will be spawned by terraform. Add the following snippet to your :guilabel:`aws_instance:guilabel:`:

.. code-block:: terraform

    user_data = <<-EOF
                #!/bin/bash
                echo "Pew Pew Pew" > index.html
                nohup python3 -m http.server 8080 &
                EOF

The aws_instance web1 shoulw now look like this:

.. code-block:: terraform

    resource "aws_instance" "web1" {
      ami           = "ami-078db6d55a16afc82"
      instance_type = "t2.micro"
      # cinquième étape
      vpc_security_group_ids = [aws_security_group.sg1.id]
    
      user_data = <<-EOF
                  #!/bin/bash
                  echo "Pew Pew Pew" > index.html
                  nohup python3 -m http.server 8080 &
                  EOF
    
      tags = {
        Name = "tf-disc-web1"
      }
    }

Enter the terraform apply command:

.. code-block:: console

    terraform apply

Enter :guilabel:`yes` in the terminal when asked.

Add a security group
====================

.. code-block:: terraform

    resource "aws_security_group" "sg1" {
      name = "tf-disc-sg1"
      ingress {
        from_port = 8080
        to_port = 8080
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
      }
    }

Do not forget to add the link to this security group in your resource. We can link the object that will be tracked by terraform as the ``object type.object name.id`` in the aws instance web1. All the resources will be created in the right order and tracked by terraform.

.. code-block:: terraform

vpc_security_group_ids = [aws_security_group.sg1.id]

The thow objects will now be linked.

.. code-block:: console

    terraform apply

Enter :guilabel:`yes` in the terminal when asked.

Open your browser and enter the following address:
http://<your-ip>:8080
or 

.. code-block:: console

    curl http://<your-ip>:8080






We encourage you to read more on Terraform by reading the `blog post from Yevgeniy Brikman from Gruntwork <https://blog.gruntwork.io/an-introduction-to-terraform-f17df9c6d180#a9b0>`_.

.. code-block:: terraform

    provider "aws" {
      region = "eu-west-3"
    }
    
    resource "aws_instance" "web1" {
      ami           = "ami-078db6d55a16afc82"
      instance_type = "t2.micro"
      vpc_security_group_ids = [aws_security_group.sg1.id]
    
      user_data = <<-EOF
                  #!/bin/bash
                  echo "Pew Pew Pew" > index.html
                  nohup python3 -m http.server 8080 &
                  EOF
    
      tags = {
        Name = "tf-disc-web1"
      }
    }
    
    resource "aws_security_group" "sg1" {
      name = "tf-disc-sg1"
      ingress {
        from_port = 8080
        to_port = 8080
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
      }
    }


with variables

.. code-block:: terraform

    provider "aws" {
      region = "eu-west-3"
    }
    
    resource "aws_instance" "web1" {
      ami           = "ami-078db6d55a16afc82"
      instance_type = "t2.micro"
      vpc_security_group_ids = [aws_security_group.sg1.id]
    
      user_data = <<-EOF
                  #!/bin/bash
                  echo "Pew Pew Pew" > index.html
                  nohup python3 -m http.server "${var.server_port}" &
                  EOF
    
      tags = {
        Name = "tf-disc-web1"
      }
    }
    
    resource "aws_security_group" "sg1" {
      name = "tf-disc-sg1"
      ingress {
        from_port = var.server_port
        to_port = var.server_port
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
      }
    }
    
    variable "server_port" {
      description = "The port the server will use for HTTP requests"
      type        = number
      default     = 8080
    }

************************
Log into the AWS console
************************

Predeployed Pod
===============


:guilabel:`Users`

``Ethernet1/2``

.. code-block:: console

    configure
    set mgt-config users admin password


.. note:: While it is a security best practice to use SSH keys to authenticate
          to VM instances in the cloud, we have defined a static password for
          the firewall's admin account in this lab (specifically, in the 
          bootstrap package).  This is because the PAN-OS XML API cannot utilize SSH keys and requires a
          username/password or API key for authentication.