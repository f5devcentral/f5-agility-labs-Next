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

- IP address
- Admin username and password


Procedure
=========
#. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Infrastructure**.
#. At the top of the screen, click **+ Start Adding Instances**.
#. Type the IP address for the BIG-IP Next instance and click **Enter**.
	 You must use port `5443`.
#. For the Management Credentials, in the **Username** and **Password** fields, specify a username and create a password for managing this instance from BIG-IP Next Central Manager. The password must meet the criteria displayed on the screen. ..  11/1/2021: Note I am not specifying the criteria for security purposes, since these are public-facing docs. Tami Skelton  
#. Confirm the password you created by typing it in the **Confirm Password** field. You'll use this username and password to manage the BIG-IP Next instance.
#. Click the **Add Instance** button. BIG-IP Next Central Manager removes all locally-configured users from the BIG-IP Next instance you are adding. If, for any reason, disablement of users on the local BIG-IP Next instance fails, adding the BIG-IP Next instance to BIG-IP Next Central Manager is halted and all users are re-enabled on the local BIG-IP Next instance.

Result
======
You can now manage this BIG-IP Next instance from BIG-IP Next Central Manager.
