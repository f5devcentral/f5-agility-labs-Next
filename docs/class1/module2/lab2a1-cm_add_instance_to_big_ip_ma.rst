..  Author: Tami Skelton 

=================================================================
How to: Add a BIG-IP Next instance to BIG-IP Next Central Manager
=================================================================

Description: This document describes how to add a BIG-IP Next instance to BIG-IP Next Central Manager for centralized management and what happens to local BIG-IP Next instance users when you do that.

Overview
========
When you add a BIG-IP Next instance to BIG-IP Next Central Manager, all users currently configured on that local BIG-IP Next instance are automatically disabled so management of the instance is done exclusively from BIG-IP Next Central Manager.

Prerequisites
=============
Before you add an instance to BIG-IP Next Central Manager, you must have the instance's:

- IP address (In this lab, we'll use 10.1.1.7)
- Admin username and password (In this lab, we'll use admin:Welcome123!)


Procedure
=========
Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Infrastructure**.

.. image:: lab2_img01_login_to_next_central_manager.png
	:scale: 10%
.. image:: lab2_img02_navigation_to_infrastructure1.png
	:scale: 25%
.. image:: lab2_img03_navigation_to_infrastructure2.png
	:scale: 25%

At the top of the screen, click **+ Add**.

.. image:: lab2_img04_my_instances_list_add.png
	:scale: 25%

Type the IP address (10.1.1.7) for the BIG-IP Next instance and click **Enter**.

	 You must use port `5443`.
.. image:: lab2_img05_add_instance_dialog_1.png
	:scale: 25%

For the Management Credentials, in the **Username** and **Password** fields, specify a username and create a password for managing this instance from BIG-IP Next Central Manager. .. 

.. image:: lab2_img06_login_to_instance.png
	:scale: 25%

Confirm the password you created by typing it in the **Confirm Password** field. You'll use this username and password to manage the BIG-IP Next instance.

.. image:: lab2_img07_add_instance_dialog_2.png
	:scale: 25%

Click the **Add Instance** button after entering confirmation of password. You'll be asked to confirm Central Management of the instance. BIG-IP Next Central Manager removes all locally-configured users from the BIG-IP Next instance you are adding. If, for any reason, disablement of users on the local BIG-IP Next instance fails, adding the BIG-IP Next instance to BIG-IP Next Central Manager is halted and all users are re-enabled on the local BIG-IP Next instance.

.. image:: lab2_img08_central_management_confirmation.png

After completing this procedure, you'll now see a new instances in the **My Instances** list.

.. image:: lab2_img09_instances_list_3_instances.png
	:scale: 25%


Result
======
You can now manage this BIG-IP Next instance from BIG-IP Next Central Manager.
