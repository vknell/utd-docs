############
Requirements
############

The following are requirements of this training workshop:


***************
Laptop required
***************
- A laptop with Internet connectivity (SSH, alternative HTTP - TCP/8080 - and HTTPS access are required).
- A standards-compliant web browser (Firefox or Google Chrome are recommended).
- An understanding of Linux or Windows operating system basics.
- Proficiency with a text editor such as vim, sublime text, visual studio code or notepad++
- A basic understanding of cloud computing concepts.


*********************
Provided POD or BYOD
********************

Predeployed Pod
===============

The instructor will give you an URL to connect on predeployed VM set with all required tools.

If you wish to deploy your own set of tools on your machine or lab machine here are the lists of what you will need to install tools on your own device. See the following sections.


Linux & Mac Endpoint
====================

- A standards-compliant web browser (Firefox or Google Chrome are recommended).
- An SSH client (any Terminal, OpenSSH, SecureCRT, etc).
- An SSL generator (ssh-keygen, OpenSSL).
- A code friendly Text editor (like VSCode, SublimeText or VIM/emacs))
- Terraform 0.11
- Ansible
- Python 3


Windows Endpoint
================

- A standards-compliant web browser (Firefox or Google Chrome are recommended).
- An SSH client (e.g., OpenSSH, PuTTY, SecureCRT, etc).
- An SSL generator for Windows (PuTTYgen, OpenSSL).
- A code friendly Text editor (like VSCode, SublimeText or Notepad++))
- Terraform 0.11
- Ansible
- Python 3


************************
Create your AWS Accounts
************************

Create a root AWS account
=========================
If you do not have an AWS account already, go to
https://aws.amazon.com/console/ and create an account. In order to continue you
will need to add a method of payment to your AWS account. Use the following
https://console.aws.amazon.com/billing/home#/paymentmethods
If creating a new account, you may receive a phone call from AWS for verification
purposes.


Create IAM AWS Accounts
=======================

You will need to create two users and a group on AWS for the labs.


Log into the AWS console
------------------------

Navigate to the `AWS URL <https://console.aws.amazon.com/>`_

Log in with your AWS credentials. 
You will create a specific role for for AWS console and terraform in a few minutes.

It is a best practice not to use the root account to create workloads and modify any configuration.
You will create a user and a group to access the console with the proper rights.


From AWS console, go to **Services** and choose **IAM** as shown:

.. figure:: img/aws-iam.png


Clic on **Users** as shown:

.. figure:: img/aws-iam-user.png


Create IAM account for console access
-------------------------------------

Clic **Add User**, give a name and define **Access type** for **AWS Management Console access** as shown after that clic on **Next Permissions**:

.. figure:: img/aws-iam-user-add-console.png

.. figure:: img/aws-iam-user-creation-console.png


Next clic **Create Group**:

.. figure:: img/aws-iam-user-create-group-console.png


Provide a group name **utd** and filter **policy Type** menu by **FullAccess**, and add the following rights.
Amazon Rights for group **utd**:
    - AWSMarketplaceFullAccess
    - AmazonEC2FullAccess
    - AWSLambdaFullAccess
    - ElasticLoadBalancingFullAccess
    - AmazonS3FullAccess
    - CloudWatchFullAccess
    - AmazonVPCFullAccess
Then click **Create Group**:

.. figure:: img/aws-iam-user-group-creation-console.png

Then select the group:

.. figure:: img/aws-iam-user-to-group-console.png


Click **Next:Tags**, you can leave it empty and click **Next:Review** and finally click **Create User**:

.. figure:: img/aws-iam-user-review-console.png


The user is now created, download the CSV file to retrieve the password. After this step you can click on **Close**:

.. figure:: img/aws-iam-user-success-csv-console.png


Create IAM account for API access
---------------------------------

Clic **Add User**:

.. figure:: img/aws-iam-user-add-api.png


Give it a name and define **Access type** for **Programmatic access** as shown after that clic on **Next Permissions**:

.. figure:: img/aws-iam-user-creation-api.png


Next select the **utd** group previously created. And click on **Next:Tags**:

.. figure:: img/aws-iam-user-to-group-api.png


Clic **Next:Review**, and finally click on **Create User**:

.. figure:: img/aws-iam-user-review-api.png


The API user has been created, download the **Access key ID** and **Secret access key** by clicking **Download .csv**.These credentials are needed to connect to Amazon EC2 service through Terraform.

We also recommand to copy/paste theses information in a **notepad** to use them to create AWS environment variables.
After this step, you can click on close:

.. figure:: img/aws-iam-user-success-csv-api.png

