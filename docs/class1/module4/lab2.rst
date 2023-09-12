==============================================
Lab 4.2 - Migration from BIG-IP to BIG-IP Next
==============================================

In this lab you will create a UCS archive from a BIG-IP instance and then import it into Central Manager. Then you will ananlyze applications to determine their eligibility to migrate to BIG-IP Next. Next, you will import shared objects, and select individual applications to migrate, save them as a draft migration, and then migrate them to a BIG-IP Next instance. 

In the UDF Blueprint there is a BIG-IP running version 15.1 that has a number of applications defined. Go to the UDF blueprint **Components** page and look for the item labeled **BIG-IP 15.1.x**  under the **F5 Products** column. Select the drop down menu next to **Access**, and then select **TMUI**. Log in with the credentials admin/admin.

.. image:: ./images/big-ip-udf.png
  :align: center
  :scale: 75%

Create BIG-IP UCS File 
======================

In order to migrate this BIG-IP configuration to BIG-IP Next, you'll need to create a UCS archive file on the BIG-IP and export it. Then you will import the UCS into Central Manager to view, analyze and migrate applications. Central Manager performs migrations at the application-level which allows customers to gradually migrate to BIG-IP Next. This is different than the traditional UCS based migration where an entire BIG-IP device, vCMP guest or Virtual Edition are migrated all at once. Since features are being phased into BIG-IP Next, it may not be possible to move all applications to BIG-IP Next initially. The per-app migration capability allows customers to identify which applications are ready to migrate to Next, and provides workflows to move individual applications. If all applications are eligble for migration, then they can all be migrated at one time. In many customers, it is expected that the migration to BIG-IP Next will be gradual, and BIG-IP and BIG-IP Next will co-exist for some time during the migration process.

Review the current configuration of the BIG-IP, click on the **Local Traffic -> Virtual Servers** page an review the different types of virtual servers and their configuration. There is a mix of virtual server types, and each is using different types of profiles, some have iRules, some have certificates, and others have WAF policies. Because of the limited resources within UDF these virtual servers will all use the same pool on the backend.


.. image:: ./images/big-ip-udf-virtual-servers.png
  :align: center
  :scale: 75%


Since the archive file may have sensitive information such as certificates/keys it is recommended that you use the Master Key functionality in BIG-IP to allow for a secure export of this type of information. In this lab, we will retrieve the Master-Key on the BIG-IP instance before creating an archive file. You'll then need to supply this information to Central Manager so that it can decrypt sensitive information and migrate it to a BIG-IP Next instance. Some background information on Master-Keys on the BIG-IP is provided below.

`Working with Master Keys <https://techdocs.f5.com/en-us/bigip-13-1-0/big-ip-secure-vault-administration/working-with-master-keys.html>`_

`K13132: Backing up and restoring BIG-IP configuration files with a UCS archive <https://my.f5.com/manage/s/article/K13132>`_

To obtain the master-key from the source BIG-IP system, go to the main UDF screen and go to the **F5 Products** column and select the drop down menu for **Access** under the **BIG-IP 15.1.x** item. Select **Console** to attach the the BIG-IP console. Alternatively, if you have loaded your ssh keys into UDF, you can attach via SSH so its easier to copy the master key. When using the embedded console in UDF, there is no copy capability so you will have to write down the master-key and enter it manaully when importing the UCS into Central Manager. 

.. image:: ./images/big-ip-console.png
  :align: center
  :scale: 75%

Login with the credentials root/default, and then enter the command **f5mku -K** to get the BIG-IP's master-key. Now, copy or write down the master-key as you will need this when importing the UCS into Central Manager.


.. code-block:: bash

    [root@ce6e127b-032e-496c-afb5-b303545907ef:Active:Standalone] config # f5mku -K
    cgGaYTNid4Gvqdelf/85cw==
    [root@ce6e127b-032e-496c-afb5-b303545907ef:Active:Standalone] config #


In the BIG-IP webUI, go to the **Systems -> Archive** page and click the **Create** button to create a new UCS archive file. 

.. image:: ./images/export-ucs-webui.png
  :align: center
  :scale: 75%

When creating the UCS archive supply a Name, enable Ecryption, then supply a passphrase (that you will remmeber), and confirm the passphrase. Then click **Finished**. Click **OK** after the archive has completed. 

.. image:: ./images/archive-passphrase.png
  :align: center
  :scale: 75%

Your achive file should be displayed in the summary, click on it.

.. image:: ./images/export-summary.png
  :align: center
  :scale: 75%

You should see something similar to the output below. Click the Download option to download the UCS file to your local machine. 

.. image:: ./images/download-archive.png
  :align: center
  :scale: 75%

Import UCS into Central Manager
===============================

Log into Central Manager and click on the **Go to Applications Workspace** button. You will be taken to the Applications main page, where you can then click the **Add Application** button.

.. image:: ./images/central-manager-add-apps.png
  :align: center
  :scale: 50%

Here you can either create a brand new application, create a new migration, or resume an existing migration that you have started previously. Under the **Migrate Application(s)** section select **New Migration**.

.. image:: ./images/new-migration.png
  :align: center
  :scale: 50%

Give the migration a **Session Name** and **Description** as seen below, then click **Next**.

.. image:: ./images/first-migration.png
  :align: center
  :scale: 50%

Here you'll need to upload the UCS archive file you exported from your BIG-IP system. Click on the area noted below, and a screen will pop up allowing you to select the UCS file from your local computer.

.. image:: ./images/ucs-file.png
  :align: center
  :scale: 50%

Master Key and Passphrase
=========================

Since the archive file may have sensitive information such as certificates/keys it is recommended you use the Master Key functionality in BIG-IP to allow for a secure export of this type of information. In this lab, we will use the Master-Key from the BIG-IP instance that you viewed before creating an archive file. You'll then need to supply this information to Central Manager so that it can decrypt sensitive information and migrate it to a BIG-IP Next instance.

Enter the **Master-Key** that you obtained from your BIG-IP, and then enable **Encrypted UCS Archive** enter the **Passphrase** you entered when creating the UCS archive in the **Password** field. 

.. image:: ./images/ucs-master-key.png
  :align: center
  :scale: 50%


Grouping of Application Services
================================


Central Manager provides two options for grouping application services. You may group them by **IP Addresses (Recommended)** or by **Virtual Server**. Grouping by IP addresses is recommended because it will group and migrate all services that use the same virtual IP address together. It would be very difficult to migrate services that use the same IP address but separate ports at different times, because typically the IP address will move from the source device to the target device during the migration. Choose **Group by Addresses (Recommended)**.


.. image:: ./images/ucs-grouping.png
  :align: center
  :scale: 50%

Click **Next** and the UCS file will be uploaded and analyzed.

Analyze Configuration
=====================

After filling in the source BIG-IP information and loading the UCS file, an **Application Migration** page will be displayed. Click **Add Application**.

.. image:: ./images/application-page.png
  :align: center
  :scale: 50%

The applications from your BIG-IP will now be displayed as Application Services.

.. image:: ./images/big-ip-app-services.png
  :align: center
  :scale: 50%


Depending on the type of grouping selected, and how the applications are configured, you may see a single virtual service per application, or you may see multiple virtual services if grouping by IP Addresses was selected and an application has more than one port. Each application service will display the virtual server address, port, a color coded status indicating its eligibility for migration, and a security status column. You can hover over the Status icon for each application to get more detail on its migration eligibility.


.. image:: ./images/icon-hover.png
  :align: center
  :scale: 75%

Here you can select individual applications to analyze them to see if they are eligible to be migrated to BIG-IP Next. Not all BIG-IP features are currenlty supported on Next. There will be a phasing of support for some configuration objects so it is expected that some applications cannot migrate at the current time. 

To see if an application is eligible for migration, click the application name as well as the virtual service underneath it and then click the **Analyze** button in the top right-hand corner off the screen. Note: You can only analyze one application service at a time. 

.. image:: ./images/analyze.png
  :align: center
  :scale: 50%


This will open the **Configuration Analyzer** page and you will see the BIG-IP configuration display from different files such as bigip.conf, or some of the default profile and monitor files. Each file will have a status associated with it indicating if there is a migration issue or not. Note: There is an enhancement logged to update the status icons of each file individually, right now some files are being grouped together, when there is not an issue in that particular file. 

.. image:: ./images/analyzer-green-files.png
  :align: center
  :scale: 75%
 
You can browse the configuration of each file for and any unsupported items, or items that may need adjusting, they will be highlighted with a squiggly yellow, red, or blue line. You can also see this on the summary preview on the left hand side of the display, it will allow you to quickly zoom in to where the problem may be in the file. Below is an example of a file with a migration issue and the squiggly yellow line notes where the issue is in both the summary and in the scroll bar.

.. image:: ./images/squiggly-line1.png
  :align: center
  :scale: 75%

You can click on the yellow line in the scroll bar and it will take you to the part of the file that has the migration issue. The squiggly yellow line will note the configuration object that is not supported. 

.. image:: ./images/squiggly-line2.png
  :align: center
  :scale: 75%

You can hover over the squiggly line to get more details about the unsupported object. You can also click the **View Problem** message for addtional details.

.. image:: ./images/squiggly-line3.png
  :align: center
  :scale: 75%

Using the Configuration Analyzer you can make a determination if an application service is ready for migration, or if you may have to wait until additional functionality is integrated into BIG-IP Next. BIG-IP Next is on a much more rapid release schedule than TMOS, so new features are being integrated on regular invtervals.

Migrate Applications to BIG-IP Next
===================================

Applications with status indicating a yellow triangle or blue information icon may not be ready for migration, or may need some changes to fully migrate to BIG-IP Next. A red icon is an unsupported object and cannot be migrated to BIG-IP Next. For this lab, we will first attempt to migrate all the green application services to BIG-IP Next. Before migrating the applications, it is a good idea to rename each application service to use a name that better represents the application instead of the genneric style names (application_1, application_2 etc...). Go ahead and rename each application, try and use the name nested underneath the application service name, so its clear what the applications are configured for, as the names are descriptive of the use case.

.. image:: ./images/rename-applications.png
  :align: center
  :scale: 100%

Below is an exmaple of the pop-up that will appear when you try and rename an application service.

.. image:: ./images/rename-applications-2.png
  :align: center
  :scale: 100%

After renaming the application services, the new names should be reflected in the output as seen below. Do this for every application.

.. image:: ./images/rename-applications-3.png
  :align: center
  :scale: 75%

After all the application services have been renamed, select all the green status services and then select the **Add** button. This will indicate that you are either ready to migrate these services, or that you are going to save them as a draft application service. After adding these applications you'll have more options on the next screen before making a decision.

.. image:: ./images/add-applications.png
  :align: center
  :scale: 75%

The next screen will present an Application Migration summary. Here, you can review the applications that you wish to move forward with, or you can remove an application from the migration. This doesn't delete the application, it is still in the UCS and you can go back later and add it again. If you forgot an application, you can click the **Add** button to go back to the remaining applications and add other apps if you wish. Once you are satisfied with the summary of applications, click **Next**.

.. image:: ./images/app-migration-summary.png
  :align: center
  :scale: 75%

The next phase is the **Pre Deployment**, here you can **Import** shared configration objects associated with the application into Central Manager. Examples of shared objects would be iRules, WAF policies, Certificates etc... These objects are treated differently than the rest of the configuration because they are managed centrally and not specific to any one device, or in the case of certificates Central Manager is managing those centrally. As an example, in traditional BIG-IP management, iRules are managed on a device-by-device basis, there is no central iRule management. Central Manager addresses this issue and allows iRules to be imported and treated as shared objects, meaning they can be shared and deployed to more than one device. Central Manager manages the entire iRule lifecycle including deployment and versioning. This is huge improvement over traditional BIG-IP iRule management. Other shared objects such as WAF policies enjoy similar benefits of centralized mangement, versioning, and full lifecycle management. 


.. image:: ./images/pre-deployment.png
  :align: center
  :scale: 75%

To understand what the shared object is, click on the number under the **Shared Objects** column. A flyout window will appear with more information about that shared object.

.. image:: ./images/import-details.png
  :align: center
  :scale: 100%

You'll also have the ability to select on a per-application basis whether the migration is saved as a **Draft** application or whether it is deployed to a specific BIG-IP Next instance. For now, we will leave all Locations for **Save as Draft**. Click the **Import** buttons for the applications that have shared objects. After the imports have finished click the **Deploy** button. The name of the button is currently misleading for this use case because you aren't really deploying the applications, you are saving them as draft applications. We will likely update the button name to reflect this in a future release. 

After hitting **Deploy** you will see status of the applications being deployed, and finally a status of **Successful**. Click the **Finish** button to complete saving the draft application migrations.

.. image:: ./images/deployment-summary.png
  :align: center
  :scale: 75%

The draft applications are added into the application dashboard, but are tagged in a **Draft** status as seen below.

.. image:: ./images/draft-application-dashboard.png
  :align: center
  :scale: 75%

If you click on one of the draft applications you will be able to view the AS3 declaration for the migrated application. Central Manager converts existing BIG-IP configurations into AS3 before moving the application over to BIG-IP Next. Click **Cancel & Exit**.

.. image:: ./images/as3-declaration.png
  :align: center
  :scale: 100%

Next, you can review the shared objects that you previosuly imported into Central Manager from the draft applications. Click on the **iRules** menu item on the left-hand side of the page. Note that some of the iRules have **migrated_** prepended to the iRule name. This lets you know that the iRule was imported via the migration process. You can click on the iRule hyperlink to get more details, and to view the actual iRule. Here, you could also edit the AS3 delcaration before deploying, but for now click the **Cancel & Exit** button.

.. image:: ./images/irules-prepend.png
  :align: center
  :scale: 75%

You can also click on the **Certificates & Keys** menu item on the left-hand side of the page. Any certs & keys imported during the migration process will also have **migrated_** prepended to the name. 

.. image:: ./images/certs-and-keys.png
  :align: center
  :scale: 75%


Performing the Migration
========================

When migrating between BIG-IP and BIG-IP Next there are a number of ways to decomission applications from BIG-IP and move them over to BIG-IP Next. Central Manager will perform all the necasarry steps to convert the configuration, but some coordination will be needed to to ensure no duplicate IP addresses are on the network, or to ensure the virtual server IP addresses are changed when they are migrated to BIG-IP Next. Some customers wil prefer to preserve the current IP addresses when applications move over to BIG-IP Next, while others may want to supply new IP addresses, and update their DNS infrastructure to point to the new IP addresses. There are pros and cons to each approach. 

Currently, Central Manager assumes the IP adddresses will stay the same (but you can manually edit the config to change them), but it does not offer any means for coordintating the transition / deocmissioning of IP addresses. This is something that will likely be enhanced to offer more support in this area in the future. For this lab, we will assume the IP addresses for the virtual servers are going to change. This is also an area that is currently very manual that could be enhanced in the future. Your feedback on migration enhancements is welcome. 

You can click on any of the draft applications and edit the AS3 declaration to alter the virtual server IP address, and then deploy the application to the new instance. This would allow the application to exist on both BIG-IP and BIG-IP Next at the same time, and DNS could coordiinate the swing of traffic to BIG-IP Next. This could be done all at once, or gradually through GSLB policies. 

Alter the draft applications virtual server addresses by replacing the last octet so that instead of it being 10.1.10.5x it is now 20 digits higher As an example, if the virtual server address is 10.1.10.51, change it to 10.1.10.71 (20 digits higher). Then click **Save & Deploy**. This will migrate the application to BIG-IP Next.

.. image:: ./images/edit-vs.png
  :align: center
  :scale: 100%

You'll then be prompted for a deploy location. Select 10.1.1.10 and select **Yes, Deploy**. NOTE: An enhancement has been filed to provide hostnames of the BIG-IP Next instances instead of IP addresses.

.. image:: ./images/deploy-ip.png
  :align: center
  :scale: 100%

Repeat this process for each application you saved as a Draft. not that some applications may have more than one virtual that needs to be edited:

.. image:: ./images/ssl-offload-2virtuals.png
  :align: center
  :scale: 100%

You can now test each virtaul server by going to the original virtual server IP addresss on BIG-IP, and then connecting to the new virtual server IP address on BIG-IP Next. In the main UDF page, go to the Ubuntu server, and in the **Access** drop down slection selct **Firefox**.

Try and connect to the following virtual servers on BIG-IP first:

- FASTL4-VS - http://10.1.10.51
- STANDARD-VS-W-TCP-PROG-VS - http://10.1.10.52:8080
- SSL-OFFLOAD-VS - http://10.1.10.53 
- SSL-OFFLOAD-VS - https://10.1.10.53 <----Will fail due to self-signed cert
- iRule-pool-slection-VS - http://10.1.10.54
- LTM-POLCY-VS - http://10.1.10.55

Now try an connect to the virtuals on BIG-IP Next:

- FASTL4-VS - http://10.1.10.71
- STANDARD-VS-W-TCP-PROG-VS - http://10.1.10.72:8080
- SSL-OFFLOAD-VS - http://10.1.10.73 
- SSL-OFFLOAD-VS - https://10.1.10.73 <----Will fail due to self-signed cert
- iRule-pool-slection-VS - http://10.1.10.74
- LTM-POLCY-VS - http://10.1.10.75

Migrating Applications with Certifictaes and Keys
=================================================

Go back into your previous migration via the **Resume Migration** option, after clicking **Add Application**. When the Application Migration page opens click **Add** to see all the applications again. Applications with Ceritifcates and Keys are currently migrated in a slightly different manner. They will be noted by a blue circle information icon. If you hover over the blue circle you will see text explaining: **Available for deployment. Unsupported objects are highlighted and will be automatically removed when deployed. Unsuporrted certificates and keys are highlighted and will be automatically replaced with a default HTTPS profile.** 

.. image:: ./images/blue-circle.png
  :align: center
  :scale: 100%

If you then **Analyze** that application you'll see the finer details of which certificates are being highlighted. Search for the blue squiggly line, to see the items that have issues. There is a known issue with the passphrase not being obsfucated properly (it should be hidden beneath the red boxes), this will be addressed shortly.

.. image:: ./images/ssl-cert-and-key.png
  :align: center
  :scale: 100%

Click **Add** to add the SSL_OFFLOAD application, and then click **Next**.


.. image:: ./images/ssl-offload-summary.png
  :align: center
  :scale: 100%

You'll see the pre-deploytment summary, and the application will have one shared object. Click on the shared object to see the details.  Note, the Certificate **Import Status** is skipped. The current version of the migration manager will not directly import the certificates into Central Manager, so they are skipped. In the future these certificates would be imported and managed by Central Manager, but for now there is a manual import process with the current release. 

.. image:: ./images/cert-predeployment.png
  :align: center
  :scale: 100%

 Click **Exit**, and then ensure the Deploy Location is **Save as Draft**, and then click **Deploy**. After a brief moment, you will see a susccessful deployment, at this time click **Finish**.

.. image:: ./images/import-summary-cert.png
  :align: center
  :scale: 100%

Now click on the SSL_OFFLOAD application. You can now view and edit the AS3 declaration. The Private Key and Certiciate will have to be manaully added into the AS3 delcaration before deploying as highlighted below. As mentioned above, this would be a temporary solution until the certificate import is fully implemented. This is expected to be available in the general release of BIG-IP Next. 

.. image:: ./images/ssl-cert-and-key-manual-edit.png
  :align: center
  :scale: 100%

We won't migrate this application at the current time, as the workflow will be changing shortly to allow for the import of certificates and keys into the shared objects. Click **Cancel & Exit**.

Migrating WAF Applications
==========================

Lastly, you will migrate applications that have WAF policies associated with the virtual servers. Go back into your previous migration via the **Resume Migration** option, after clicking **Add Application**. When the Application Migration page opens click **Add** to see all the applications again.