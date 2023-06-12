..  Author: Tami Skelton; revisions by Chad Jenison May 2023 

=================================================================
Lab 2.1 - How to: Add a BIG-IP Next instance to BIG-IP Next Central Manager
=================================================================

Description: This document describes how to add a BIG-IP Next instance to BIG-IP Next Central Manager for centralized management and what happens to local BIG-IP Next instance users when you do that.

Overview
========
When you add a BIG-IP Next instance to BIG-IP Next Central Manager, all users currently configured on that local BIG-IP Next instance are automatically disabled so management of the instance is done exclusively from BIG-IP Next Central Manager.

Prerequisites
=============
Before you add an instance to BIG-IP Next Central Manager, you must have the instances:

- IP address (In this lab, we'll use 10.1.1.7)

    .. code-block:: console

		10.1.1.7

- Admin username and password (In this lab, we'll use admin:Welcome123!)

	Username:

	.. code-block:: console
		
		admin

	Password:

	.. code-block:: console
		
		Welcome123!


Procedure
=========
#. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Infrastructure**.
    .. image:: ./lab2_img01_login_to_next_central_manager.png
		:scale: 10%
    .. image:: ./lab2_img02_navigation_to_infrastructure1.png
		:scale: 25%
    .. image:: ./lab2_img03_navigation_to_infrastructure2.png
		:scale: 25%
#. At the top of the screen, click **+ Add**.
    .. image:: ./lab2_img04_my_instances_list_add.png
		:scale: 25%
#. Type the IP address (10.1.1.7) for the BIG-IP Next instance and click **Connect**. You can ignore the top option for Creating a new instance. You must use port `5443`.
    IP Address:
    	.. code-block:: console

			10.1.1.7

	.. image:: ./lab2_img05_add_instance_dialog_1.png
		:scale: 25%

#. For the Management Credentials, in the **Username** and **Password** fields, enter the username and password that were used to login to the BIG-IP Next Central Manager and click **Submit**. (admin:Welcome123!)
    Username:

        .. code-block:: console

            admin

    Password:

        .. code-block:: console

            Welcome123!

    .. image:: ./lab2_img06_login_to_instance.png
        :scale: 25%

#. Once you have authenticated to the instance, you'll see a dialog prompting you to supply new Management Credentials. We suggest accepting the pre-populated username (admin-cm) and re-using the same password that has been used so far in the lab (Welcome123!). Enter the password twice (in the **Password** and **Confirm Password** fields). You'll use this username and password to manage the BIG-IP Next instance and click **Add Instance**.
    Password:

		.. code-block:: console
			Welcome123!

    .. image:: ./lab2_img07_add_instance_dialog_2.png
		:scale: 25%
#. You'll be asked to confirm Central Management of the instance. BIG-IP Next Central Manager removes all locally-configured users from the BIG-IP Next instance you are adding. If, for any reason, disablement of users on the local BIG-IP Next instance fails, adding the BIG-IP Next instance to BIG-IP Next Central Manager is halted and all users are re-enabled on the local BIG-IP Next instance. You should click **Add** at this confirmation dialog.
    .. image:: ./lab2_img08_central_management_confirmation.png
#. After completing this procedure, you'll now see a new instances in the **My Instances** list.
    .. image:: ./lab2_img09_instances_list_3_instances.png
		:scale: 25%

Result
======
You can now manage this BIG-IP Next instance from BIG-IP Next Central Manager.
