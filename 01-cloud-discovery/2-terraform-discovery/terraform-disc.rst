d=================================
Activity 1.2: Terraform Discovery
=================================

In this activity you will:


- Create AWS environment variables
- Create an SSH key-pair
- Create the Terraform variables
- Initialize the AWS Terraform provider
- Deploy the lab infrastucture plan
- Confirm firewall bootstrap completion



Open Terminal :
---------------

.. figure:: img/work-in-progress.png


Create terraform Environment :
------------------------------

You will need to download the sample repository used in this lab.  This repository (or *repo*) contains
the files needed to deploy the network and compute infrastructure we'll be working with.

.. code-block:: bash
    
    wget  -P ~/utd/utd-automation/first-step-terraform https://raw.githubusercontent.com/PaloAltoNetworks/terraform-templates/master/sample/deploy_pavm.tf
    wget  -P ~/utd/utd-automation/first-step-terraform https://raw.githubusercontent.com/PaloAltoNetworks/terraform-templates/master/sample/deploy_vpc.tf
    wget  -P ~/utd/utd-automation/first-step-terraform https://raw.githubusercontent.com/PaloAltoNetworks/terraform-templates/master/sample/variables.tf


Create AWS environment variables
--------------------------------

Select the Paris region (eu-west-3) at the top right corner of the screen

Create an SSH key-pair
----------------------
All AWS EC2 instances are required to have an SSH key-pair defined when the
instance is created.  This is done to ensure secure access to the instance will
be available once it is created.

click on EC2:

.. figure:: img/sshkeypair-1.png

Click Key Pairs

.. figure:: img/sshkeypair-2.png

Clic Create key pair button:

.. figure:: img/sshkeypair-3.png

Give it a name: ec2sshkeypair

.. figure:: img/sshkeypair-4.png

And click Create .

.. figure:: img/sshkeypair-5.png

Change the key to the following folder

.. code-block:: bash

    mv ~/Downloads/ec2sshkeypair.pem ~/utd/utd-automation/first-step-terraform/

Create S3 Bucket for bootstrapping
----------------------------------
Bootstrapping is a feature of the VM-Series firewall that allows you to load a pre-
defined configuration into the firewall during boot-up and to automate its deployment.
This ensures that the firewall is configured and ready at initial boot-up, removing the
need for manual configuration.

To create a bootstrap bucket, Sign in to the AWS console https://www.amazon.com
and click on S3

.. figure:: img/buckets3-1.png

Click Create Bucket:

.. figure:: img/buckets3-2.png

Enter a bucket name and select a region and click Create as there is no need to go
through the subsequent steps as the default values will be used.

.. figure:: img/buckets3-3.png




!!!!!!!!!!!!    Bootstrap Bucket have to be created in same region of VPC     !!!!!!!!!!!!!!!!!!



You will need to enter a globally unique bucket name. AWS will warn you if the
name is not unique. 


Once the bucket is created, select your bucket and click on **copy ARN** button and copy/paste the value in file named **ARNBucket**
In your terminal type **export ARN=** and paste the key that is in your clipboard then enter the following command:

..  code-block:: bash

    sed -i 's/arn:aws:s3:::ha1-dev-paloalto/'"$ARN"'/g' ~/utd/utd-automation/first-step-terraform/deploy_vpc.tf

This information will used later in Terraform script for bootstrap of VM FW.


Add restricted permission on S3 Bucket (Read only):
---------------------------------------------------

We need to give ** relevant rights** for IAM account created to use API (IAM account for API access).

Go to Services -> IAM and click on **Customer Managed Policies** :

.. figure:: img/buckets3-4.png

Clic **Create policy** :

.. figure:: img/buckets3-5.png

Click on **Choose a service** and choose **S3**

.. figure:: img/buckets3-6.png

Choose **Read** for Acess level:

.. figure:: img/buckets3-7.png

Click on **Ressources** **Specify bucket ressource ARN for the GetBucketLocation...** and clic on **bucket** on **Add ARN to retrict access**:

.. figure:: img/buckets3-8.png

Add ARN of the Bucket :

.. figure:: img/buckets3-9.png

Click on *Review Policy**

.. figure:: img/buckets3-10.png

Click Create policy

.. figure:: img/buckets3-11.png





Build Bootstrping in S3 Bucket :
--------------------------------


click on the newly created bucket and modify 

on the newly created bucket
and add four folders called **config**, **license**, **software** and **content** by clicking on
Create Folder:

.. figure:: img/bootstrap-1.png

Fill in the folder name and click Save. Repeat the process for the three remaining
folders.


.. figure:: img/bootstrap-2.png

.. figure:: img/bootstrap-3.png




Upload files in the various buckets folder from ~/utd/first-step-terraform folde




Upload the bootstrap.xml and init-cfg.txt files from bootstrap folder (~/utd/first-step-terraform/bootstrap-files/) to the **config** folder by clicking **config**.

.. figure:: img/bootstrap-4.png

Select Add Files and select the two files (bootstrap.xml and init-cft.txt) handled previously and click Upload:

.. figure:: img/bootstrap-5.png

The two files should be listed under the folder:

.. figure:: img/bootstrap-6.png

Upload the **panupv2-all-contents-8225-5857** file to the **content** folder.
click on the **content** folder ins the S3 console and click Upload. Select **Add Files**
and select the file (panupv2-all-contents-8225-5857) downloaded previously and click
**Upload**:

.. figure:: img/bootstrap-7.png

Once complet ed the file is listed under the folder content :

.. figure:: img/bootstrap-8.png



Optional for Bootstrap: 
If need upgrade automaticaly your VM after boot, you can Upload a PANOS image file to the **software** folder.
click on the **software** folder ins the S3 console and click Upload. Select Add Files
and select the file (example : PanOS_vm_9.0.1 ) retrieved from PANW support site, and click
**Upload**:




Optional for Bootstrap: 
If need associate licenses (BYOL) automaticaly your FW VM after boot, you can Upload a Licenses file to the **license** folder.
click on the **license** folder ins the S3 console and click Upload. Select Add Files
and select the file (example : 0001A100110-threats.key) downloaded previously and click
Upload:




Create the Terraform variables
------------------------------
Change into the AWS deployment directory.

.. code-block:: bash

    $ cd ~/utd/utd-automation/utd/sample --------  A REVOIR----------

In this directory you will find the three main files associated with a
Terraform plan: ``deploy_panvm.tf``, ``variables.tf``, and ``deploy_vpc.tf``.  View the
contents of these files to see what they contain and how they're structured.

.. code-block:: bash

    $ more deploy_panvm.tf
    $ more deploy_vpc.tf
    $ more variables.tf


deploy_pavm.tf - Terraform template for Palo Alto Networks VM-Series
firewall.

deploy_vpc.tf - Terraform template for create a VPC on AWS. The VPC will
create the management, trust, and untrust subnets for the VM-Series firewall.
An internet gateway needed for the internet connection and AWS endpoint
(currently disabled. need to uncomment the code to enable the feature) to
allow the firewall to access the S3 bucket via private IP address.

variables.tf - Variables you can set for the deployment


Modify/Adapt Configuration
--------------------------

1. You need to modify the variables.tf file with a Terminal or text editor.


To set the AWS access key and secret key of your IAM account for API access ( see the doc `here <https://utd-automation.readthedocs.io/en/latest/00-getting-started/aws-account.html>`_. ):

.. code-block:: bash

    # AWS Credential
    variable "access_key" {
    decscription = "AWS Access Key"
    default = "XXXX"
    }
    variable "secret_key" {
    description = "AWS Secret Key"
    default = " XXXX "
    }

2.  Modify variables.tf file with a Terminal or text editor with right information regarding Region and AZ if needed:(The template is also setup to deploy in US East Region)

.. code-block:: bash


        # AWS Region and Availablility Zone
        variable "region" {
        default = "us-west-2"
        }
        variable "availability_zone" {
        default = "us-west-2a"
        }



3. Modify variables.tf file with a Terminal or text editor with right information regarding SSH keypair:

    .. code-block:: bash

        variable "pavm_key_name" {
        description = "Name of the SSH keypair to use in AWS."
        default = "ec2sshkeypair.pem"
        }
        variable "pavm_key_path" {
        description = "Path to the private portion of the SSH key specified."
        default = "~/utd/first-step-terraform/ec2sshkeypair.pem"
        }

4. (Optional) Modify variables.tf file with a Terminal or text editor with right information regarding the VPC CIDR and VPC Subnets if needed:

    .. code-block:: bash

      
        
        Modify CIDR block if needed:
        # VPC configuration
        variable "vpc_cidr_block" {
        default = "10.88.0.0/16"
        }
        variable "vpc_instance_tenancy" {
        default = "default"
        }
        Modify VPC Name if needed:
        variable "vpc_name" {
        default = "PAVM VPC"
        }
        Modify CIDR Block of subnets if needed :
        # Management subnet configuration
        variable "mgmt_subnet_cidr_block" {
        default = "10.88.0.0/24"
        }
        # Untrust subnet configuration
        variable "untrust_subnet_cidr_block" {
        default = "10.88.1.0/24"
        }
        # Trust subnet configuration
        variable "trust_subnet_cidr_block" {
        default = "10.88.66.0/24"
        }

5. Adapt variables.tf file with a Terminal or text editor with right information regarding AMI reference if needed:

An Amazon Machine Image (AMI) provides the information required to launch an instance. You must specify an AMI when you launch an instance. You can launch multiple instances from a single AMI when you need multiple instances with the same configuration. You can use different AMIs to launch instances when you need instances with different configurations.

An AMI includes the following:

    - One or more EBS snapshots, or, for instance-store-backed AMIs, a template for the root volume of the instance (for example, an operating system, an application server, and applications).
    - Launch permissions that control which AWS accounts can use the AMI to launch instances.
    - A block device mapping that specifies the volumes to attach to the instance when it's launched.

To find a Palo Alto Networks AMI using the Images page

    Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/

From the navigation bar, select the Region in which to launch your instances. You can select any Region that's available to you, regardless of your location.

In the navigation pane on left, choose AMIs.

Use the Filter options to scope the list of displayed AMIs to see only the AMIs that interest you. 
For example, to list all Palo Alto Networks AMIs provided by AWS, select Public images. Type **palo alto networks** in filter fiels to view list of AMI available in choosen Region. 


Then verify or adapt AMI ID if needed :

.. code-block:: bash

    # PAVM configuration
    variable "pavm_payg_bun2_ami_id" {
    default = {
    eu-west-1 = "ami-5d92132e",
    ap-southeast-1 = "ami-946da7f7",
    ap-southeast-2 = "ami-d7c6e5b4",
    ap-northeast-2 = "ami-fb08c195",
    eu-central-1 = "ami-8be001e4",
    ap-northeast-1 = "ami-b84b5ad6",}
    }
    us-east-1 = "ami-29a8a243",
    us-west-1 = "ami-12d0ad72",
    sa-east-1 = "ami-19810e75",
    us-west-2 = "ami-e4be4b84"
    variable "pavm_byol_ami_id" {
    default = {
    ap-south-1 = "ami-5c187233",
    eu-west-1 = "ami-73971600",
    ap-southeast-1 = "ami-0c60aa6f",
    ap-southeast-2 = "ami-f9c4e79a",
    ap-northeast-2 = "ami-fa08c194",
    eu-central-1 = "ami-74e5041b",
    ap-northeast-1 = "ami-e44b5a8a",
    us-east-1 = "ami-1daaa077",
    us-west-1 = "ami-acd7aacc",
    sa-east-1 = "ami-1d860971",
    us-west-2 = "ami-e7be4b87"
    }
    }


6. Adapt variables.tf file with a Terminal or text editor with right information regarding Bucket S3 for Bootstraping where XXXX is the name of your bucket S3.

.. code-block:: bash

    variable "pavm_user_data" {
    #default = "vmseries-bootstrap-aws-s3bucket=panw-mlue-bucket"
    default = "vmseries-bootstrap-aws-s3bucket=XXXX"
    }
    variable "pavm_iam_instance_profile" {
    default = "pa_bootstrap_s3_readonly"
    }


7. You need to modify the deploy_panw.tf file with a Terminal or text editor.

For both AWS, the licensing options are bring your own license (BYOL) and pay as you go/consumption-based (PAYG) subscriptions.

    - BYOL: Any one of the VM-Series models, along with the associated Subscriptions and Support, are purchased via normal Palo Alto Networks channels and then deployed through your AWS or Azure management console.
    - PAYG: Purchase the VM-Series and select Subscriptions and Premium Support as an hourly subscription bundle from the AWS Marketplace.
        - Bundle 1 contents: VM-300 firewall license, Threat Prevention Subscription (inclusive of IPS, AV, Malware prevention) and Premium Support.  
        - Bundle 2 contents: VM-300 firewall license, Threat Prevention (inclusive of IPS, AV, Malware prevention), WildFire™ threat intelligence service, 
          URL Filtering, GlobalProtect Subscriptions and Premium Support.


In deploy_panw.tf you can adapt the AMI information regarding your licensing
type (BYOL or Bundle2):

.. code-block:: bash

    # Palo Alto VM-Series Firewall
    resource "aws_instance" "pavm" {
    #ami = "${lookup(var.pavm_byol_ami_id, var.region)}"
    ami = "${lookup(var.pavm_payg_bun2_ami_id, var.region)}"
    availability_zone = "${var.availability_zone}"
    tenancy = "default"
    ebs_optimized = false
    disable_api_termination = false
    instance_initiated_shutdown_behavior = "stop"
    instance_type = "${var.pavm_instance_type}"
    key_name = "${var.pavm_key_name}"
    monitoring = false
    vpc_security_group_ids = [ "${aws_security_group.default-security-gp.id}" ]
    subnet_id = "${aws_subnet.mgmt-subnet.id}"
    associate_public_ip_address = "${var.pavm_public_ip}"
    private_ip = "${var.pavm_mgmt_private_ip}"
    source_dest_check = false
    tags = {
    Name = "PAVM"
    }



8. (Optional) You need to modify the deploy_vpc.tf file with a Terminal or text editor if you want to use a VPC Endpoint.

A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct 
Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

Endpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components. They allow communication between instances in your VPC and services without imposing availability risks or 
bandwidth constraints on your network traffic. 

In deploy_vpc.tf you have to uncomment code to use Bootstrap S3 Bucket and give the S3 name bucket:

.. code-block:: bash

    # Create an endpoint for S3 bucket
    /* Uncomment to enable */
    resource "aws_vpc_endpoint" "private-s3" {
    vpc_id = "${aws_vpc.pavm-vpc.id}"
    service_name = "com.amazonaws.us-west-2.s3"
    /* Uncomment to enable policy
    policy = <<POLICY
    {
    "Statement": [{
    "Effect": "Deny",
    "Principal": "*",
    "Action": "s3:*",
    "Resource": "arn:aws:s3:::mys3bucketutd"
    }
    ]
    }
    POLICY
    */

Nota : 
- Value for ARN (**arn:aws:s3:::mys3bucketutd**) was been copied in in file named **ARNBucket** in ~/utd/first-step-terraform folder at the begining of activity (see ici)






Initialize the AWS Terraform provider
-------------------------------------
Once you've created the ``terraform.tfvars`` file and populated it with the
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

.. code-block:: bash

    $ terraform plan

If there are no errors and the plan output looks good, let's go ahead and
perform the deployment.

.. code-block:: bash

    $ terraform apply -auto-approve







At a high level these are each of the steps this plan will perform:

    #. Create the VPC
    #. Create the Internet gateway
    #. Create VPC NAT Gateway
    #. Create the subnets
    #. Create the security groups for each subnet
    #. Create routing tables and routes
    #. Create the VM-Series firewall instance
    #. Create the VM-Series firewall interfaces
    #. Create the Elastic IPs for the ``management`` and ``untrust`` interfaces





The deployment process should finish in a few minutes and you will be presented
with the public IP addresses of the VM-Series firewall management and untrust
interfaces.  However, the VM-Series firewall can take up to *ten minutes* to
complete the initial bootstrap process.

It is recommended that you skip ahead and read the :doc:`../03-run/terraform/background-terraform` section while you wait.


Verify on AWS Console some elements created by terraform
--------------------------------------------------------

.. figure:: img/work-in-progress.png



Confirm firewall bootstrap completion
-------------------------------------
SSH into the firewall with the following credentials.

- **Username:** ``admin``
- **Password:** ``admin``

.. code-block:: bash

    $ ssh admin@<FIREWALL_MGMT_IP>

Replace ``<FIREWALL_MGMT_IP>`` with the IP address of the firewall management
interface that was provided in the Terraform plan results.  This information
can be easily recalled using the ``terraform output`` command within the
deployment directory.

.. warning:: If you are unsuccessful the firewall instance is likely still
   bootstrapping or performing an autocommit.  Hit ``Ctrl-C`` and try again
   after waiting a few minutes.  The bootstrap process can take up to *ten
   minutes* to complete before you are able to successfully log in.

Once you have logged into the firewall you can check to ensure the management
plane has completed its initialization.

.. code-block:: bash

    admin@lab-fw> show chassis-ready

If the response is ``yes``, you are ready to proceed with the configuration
activities.

.. note:: While it is a security best practice to use SSH keys to authenticate
          to VM instances in the cloud, we have defined a static password for
          the firewall's admin account in this lab (specifically, in the 
          bootstrap package).  This is because the PAN-OS XML API cannot utilize SSH keys and requires a
          username/password or API key for authentication.



Destroy the lab infrastucture plan:
-----------------------------------
To clean up the deployment, just run the following command

.. code-block:: bash

$ terraform destroy

it will automatically delete every object that was created by the template.



What were bad things on this Activity:
--------------------------------------

- AWS Access key and AWS Secret key are visible and stored in vraiable.tf file.
- It needs to prepare a S3 bucket from AWS Console before use this scripts Terraform. 
- Methode for Bootstraping expose password administrator of the FW in S3 bucket with potentiel high risk.
- Structure of scripts is not relevant for large deployment architecture. 



Conclusion:
-----------
We can do better then let's go to next activity : **Build Multicloud**