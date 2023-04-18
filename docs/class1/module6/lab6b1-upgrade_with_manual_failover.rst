..
  Tami Skelton
  Updated: 10/10/2022.

=====================================================================================================
How to: Upgrade a BIG-IP Next HA instance on VE from BIG-IP Next Central Manager with manual failover
=====================================================================================================

**THIS SECTION INCOMPLETE** - Chad Jenison

Overview
========
Use this procedure to upgrade the software for a BIG-IP Next HA instance on VE from BIG-IP Next Central Manager using the manual failover method. For this procedure, you upgrade the standby node first, and then the active node.

..
  *Note*: For information about upgrading a BIG-IP Next HA instance on VE using the auto-failover procedure see `Upgrade a BIG-IP Next HA instance on VE from BIG-IP Next Central Manager using the automatic failover method <../use_cm/cm_upgrade_ve_bigip_ha_ve_autofailover.html>`_.

Procedure
=========

#. Log in to the `F5 Beta Community Member site <https://f5beta.centercode.com/welcome/>`_.

   **Note**: The BIG-IP Next Central Manager user interface directs you to the F5 Downloads site. Until general availability, you must navigate to the Beta site.
#. Click the link for the version you want to download.
#. At the top of the screen, click **BIG-IP Next EA Program** \> **Documents & Builds**.
#. Click the link for the BIG-IP Next version you want to download and download the **tgz** file and the (if required) **sig** file.

   The file is downloaded to your local system and can take up to an hour because of the size of these files.
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

