..
  Tami Skelton
  Updated: 10/10/2022.

=====================================================================================================
How to: Upgrade a BIG-IP Next HA standalone instance on VE from BIG-IP Next Central Manager
=====================================================================================================

**THIS SECTION INCOMPLETE** - Chad Jenison

Overview
========
Use this procedure to upgrade the software for a BIG-IP Next HA Standalone instance from BIG-IP Next Central Manager.

Procedure
=========

#. Log in to BIG-IP Next Central Manager as admin, click the Workspace icon next to the F5 logo, and click **Infrastructure**.

#. Select the checkbox next to the BIG-IP Next HA instance you want to upgrade.

#. At the top right of the screen, click **Actions** and select **Upgrade**.
#. For the **Standby Node**, click the **Upgrade** button next to the software version.

   **Important**: You must upgrade the standby node before upgrading the active node.
#. For the image file, click **Upload file** and the **Browse** link to navigate to the location you downloaded the image **tgz** file.
#. Click the **Open** button.

   Click the **Upload** button.
#. For the image file, click **Upload file** and the **Browse** link to navigate to the location you downloaded the image **tgz** file.
#. Click the **Open** button.

   These files are large, so upload can take some time.
#. After the file(s) upload, click the **Upgrade** button and confirm the upgrade by clicking the **Upgrade** button on the confirmation message.

Result
======
After the upgrade process, the BIG-IP Next HA instance displays with the new version on the **My Instances** page.

