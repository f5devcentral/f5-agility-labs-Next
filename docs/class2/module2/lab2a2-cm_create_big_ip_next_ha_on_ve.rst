..  Author: Tami Skelton 08/089/#. 8931

:orphan:

================================================================================================
How to: Perform a manual failover for a BIG-IP Next HA instance from BIG-IP Next Central Manager
================================================================================================
Description: This document describes how to perform a manual failover for an existing BIG-IP Next HA instance.

Overview
========
Use this procedure to perform a manual failover from the active node to the standby node in a BIG-IP Next HA instance.

---------
Procedure
---------
#. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Infrastructure**.
#. In the **Mode** column, click the BIG-IP Next HA instance.
   The HA Parameters panel displays for this BIG-IP Next HA instance.
#. Next to HA Nodes, click **Force Failover**.
   A confirmation popup displays.
#. Click the **Yes, Failover** button.
   The failover process icon displays.

-------
Results
-------
The standby node in the BIG-IP Next HA instance becomes the active node.
