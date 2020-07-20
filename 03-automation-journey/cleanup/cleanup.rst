########
Clean Up
########

In this activity you will:

- Destroy the lab deployment

**************************
Destroy the lab deployment
**************************

When deploying infrastructure in the public cloud it is important to tear it down when it is no longer needed. Otherwise you will end up paying for services that are no longer needed. We'll need to go back to the deployment directory and use Terraform to destroy the infrastructure we deployed at the start of the lab.

Change into the ``deployment`` directory.

For AWS:

.. code-block:: console

    cd ~/utd-automation/journey/deployment/aws


Tell Terraform to destroy the contents of its plan files.

.. code-block:: console

    terraform destroy

.. warning:: Make sure that all resources that you created today are destroyed. Failing to do so may incur high unexpected charges.

