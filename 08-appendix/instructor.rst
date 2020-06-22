============================
Activity for UTD Preparation
============================

As instructor you need to deploy student'Environnement composed of Ubuntu servers with all tools used in this UTD.

If you have already installed Terraform, you can use terroform script to provision UTD environmemt in following methode 1.
If not, you can directly install Terraform and extensions on Ubuntu 18.04 LTS in following methode 2 or install on other device (Windows/Mac/Linux) 
to install tools into by following methode 3.


Methode 1: Terraform and extensions are already installed
---------------------------------------------------------
- Clone repositori as shown :

.. code-block:: bash

        cd ~/
        mkdir ./code
        git clone https://github.com/vknell/utd-automation-instructor ~/code/utd-automation-instructor  !!!!!  revoir avec Victor


- Deploy environmemt for students 


In order for Terraform to do this it will need to authenticate to AWS using the
AWS Access Key and Secret Key values that were presented to you or you can created in 
AWS console when the lab was started.  Rather than write these as Terraform variables, we
will use environment variables. Because in production you will upload your terraform to a repo,
if you hardcode your access key and secret access to any file and push with github to a public repo
you will create a security issue.

In using Terminal, create the environment variables by typing:

.. code-block:: bash

    $ export AWS_ACCESS_KEY_ID=your-access-key-here
    $ export AWS_SECRET_ACCESS_KEY=your-secret-key-here


(Optional) Adapt Number of POD '(default is 5)' :
-------------------------------------------------

.. code-block:: bash

    cd ~/code/utd-automation-instructor

in variables.tf file you can change number of POD you want to deploy :

.. code-block:: bash

        variable "instance_count" {
        description = "number of instances to create"
        default = "5"
        }



Initialize the AWS Terraform provider
-------------------------------------
Once you've updated the ``variables.tf`` file and populated it with the
variables and values you are now ready to initialize the Terraform providers.
For this initial deployment we will only be using the
`AWS Provider <https://www.terraform.io/docs/providers/aws/index.html>`_.
This initialization process will download all the software, modules, and
plugins needed for working in a particular environment.

.. code-block:: bash

    $ terraform init


Deploy the lab infrastucture plan
---------------------------------
We are now ready to deploy our lab infrastructure plan.  We should first
perform a dry-run of the deployment process and validate the contents of the
plan files and module dependencies.

for windows :
just before please update your module/bootstrap/main.tf line "dev/null" with "NUL"

.. code-block:: bash

    $ terraform plan

If there are no errors and the plan output looks good, let's go ahead and
perform the deployment.

.. code-block:: bash

    $ terraform apply -auto-approve

Retriev IP which used by students to connect on UTD environmemt.
---------------------------------------------------------------

After "terraform apply -auto-approve" action is finished, you can view all IP address for each POD you have deployed.






Methode 2: You want install Terraform and extensions on Ubuntu 18.04 LTS
------------------------------------------------------------------------

- Deploy Ubuntu 18.04 LTS instance on AWS Cloud Provider or hyperviser like VMWare workstation, Fusion, ESxi...

- Install VS-Codium and  extension from your Ubuntu 18.04 LTS :

## Update and upgrade

.. code-block:: bash




        apt update
        apt upgrade -y

## Install snap packages VS Codium and Terraform

.. code-block:: bash

        snap install codium --classic
        snap install terraform

## Install VS Codium extensions

.. code-block:: bash

        codium --install-extension mauve.terraform
        codium --install-extension ms-python.python
        codium --install-extension vscoss.vscode-ansible
        sudo cp terraform /usr/bin/


#       # Clone 
        git clone https://github.com/vknell/utd-automation-instructor ~/utd-automation-instructor  !!!!!  revoir avec Victor


- Deploy environmemt for students 


In order for Terraform to do this it will need to authenticate to AWS using the
AWS Access Key and Secret Key values that were presented to you or you can created in 
AWS console when the lab was started.  Rather than write these as Terraform variables, we
will use environment variables. Because in production you will upload your terraform to a repo,
if you hardcode your access key and secret access to any file and push with github to a public repo
you will create a security issue.

In using Terminal, create the environment variables by typing:

.. code-block:: bash

    $ export AWS_ACCESS_KEY_ID=your-access-key-here
    $ export AWS_SECRET_ACCESS_KEY=your-secret-key-here


(Optional) Adapt Number of POD '(default is 5)' :
-------------------------------------------------

.. code-block:: bash

    cd ~/code/utd-automation-instructor

in variables.tf file you can change number of POD you want to deploy :

.. code-block:: bash

        variable "instance_count" {
        description = "number of instances to create"
        default = "5"
        }


Initialize the AWS Terraform provider
-------------------------------------
Once you've updated the ``variables.tf`` file and populated it with the
variables and values you are now ready to initialize the Terraform providers.
For this initial deployment we will only be using the
`AWS Provider <https://www.terraform.io/docs/providers/aws/index.html>`_.
This initialization process will download all the software, modules, and
plugins needed for working in a particular environment.

.. code-block:: bash

    $ terraform init


Deploy the lab infrastucture plan
---------------------------------
We are now ready to deploy our lab infrastructure plan.  We should first
perform a dry-run of the deployment process and validate the contents of the
plan files and module dependencies.

for windows :
just before please update your module/bootstrap/main.tf line "dev/null" with "NUL"

.. code-block:: bash

    $ terraform plan

If there are no errors and the plan output looks good, let's go ahead and
perform the deployment.

.. code-block:: bash

    $ terraform apply -auto-approve

Retriev IP which used by students to connect on UTD environmemt.
---------------------------------------------------------------

After "terraform apply -auto-approve" action is finished, you can view all IP address for each POD you have deployed.





Methode 3: You want install Terraform and extensions directly on your own device (Windows/Mac/Linux)
----------------------------------------------------------------------------------------------------
Today the lab is tested with terraform 0.8 to 0.11. You can download the terraform binary and create a env PATH on your endpoint.
Terraform is a simple binary without library etc… you can follow this video (https://learn.hashicorp.com/terraform/getting-started/install.html for linux and windows.



Example for linux distribution like Debian:

download the zip file https://releases.hashicorp.com/terraform/0.11.14/

.. code-block:: bash

        apt update
        apt upgrade -y
        sudo apt-get install unzip
        unzip [Your terraform file in version 0.11.xx]
        sudo cp terraform /usr/bin/




Now you can verify the version :

.. code-block:: bash

        terraform --version


- Clone repositori as shown :

.. code-block:: bash

        cd ~/
        mkdir ./code
        git clone https://github.com/vknell/utd-automation-instructor ~/code/utd-automation-instructor  !!!!!  revoir avec Victor


- Deploy environmemt for students 


In order for Terraform to do this it will need to authenticate to AWS using the
AWS Access Key and Secret Key values that were presented to you or you can created in 
AWS console when the lab was started.  Rather than write these as Terraform variables, we
will use environment variables. Because in production you will upload your terraform to a repo,
if you hardcode your access key and secret access to any file and push with github to a public repo
you will create a security issue.

In using Terminal, create the environment variables by typing:

.. code-block:: bash

    $ export AWS_ACCESS_KEY_ID=your-access-key-here
    $ export AWS_SECRET_ACCESS_KEY=your-secret-key-here


(Optional) Adapt Number of POD '(default is 5)' :
-------------------------------------------------

.. code-block:: bash

    cd ~/code/utd-automation-instructor

in variables.tf file you can change number of POD you want to deploy :

.. code-block:: bash

        variable "instance_count" {
        description = "number of instances to create"
        default = "5"
        }



Initialize the AWS Terraform provider
-------------------------------------
Once you've updated the ``variables.tf`` file and populated it with the
variables and values you are now ready to initialize the Terraform providers.
For this initial deployment we will only be using the
`AWS Provider <https://www.terraform.io/docs/providers/aws/index.html>`_.
This initialization process will download all the software, modules, and
plugins needed for working in a particular environment.

.. code-block:: bash

    $ terraform init


Deploy the lab infrastucture plan
---------------------------------
We are now ready to deploy our lab infrastructure plan.  We should first
perform a dry-run of the deployment process and validate the contents of the
plan files and module dependencies.

for windows :
just before please update your module/bootstrap/main.tf line "dev/null" with "NUL"

.. code-block:: bash

    $ terraform plan

If there are no errors and the plan output looks good, let's go ahead and
perform the deployment.

.. code-block:: bash

    $ terraform apply -auto-approve

Retriev IP which used by students to connect on UTD environmemt.
---------------------------------------------------------------

After "terraform apply -auto-approve" action is finished, you can view all IP address for each POD you have deployed.