==========================================================================
How to: Deploy iRules to a FAST application on BIG-IP Next Central Manager
==========================================================================

The following describes how to deploy iRules to a new application using BIG-IP Next Central Manager.

You must have already configured iRules to deploy them to an application.
For more information about creating iRules, see `How to: Create or modify iRules on BIG-IP Next Central Manager <cm_irules_create_how_to.md>`_.
For more information about creating applications on BIG-IP Next Central Manager, see `How to: Manage applications using BIG-IP Next Central Manager and FAST templates <cm_create_delete_apps.md>`_.

BIG-IP Next Central Manager provides a FAST application template for secure application (iRule-HTTPS-Load-Balancing-Service). The template allows you to select one or more iRules for the application.

Use the following procedure to add iRules to a secure application:

* :ref:`Create an HTTPS application with iRules`

Prerequisites
=============

* Configure iRules to BIG-IP Next Central Manager.
* The IP address of the BIG-IP Next instance you plan to deploy the application to.
* The virtual IP address, port, list of endpoint server (pool member) addresses.
* A `TLS certificate <cm_instance_certificate_and_key_management.md>`_ is configured to BIG-IP Next Central Manager.

..
   * Determine which iRules template you’re going to use. There are two default iRules templates:
   * **iRule-Embedded-Basic-Load-Balancing-Service** - Create applications with load balancing capabilities that have a basic iRule.
   * **iRule-HTTPS-Load-Balancing-Service** - Create secure applications with load balancing capabilities and attached iRules.
     Attach existing, new or cloned iRules to the application. A predefined certificate is required.
     The template prompts you to select a TLS certificate and iRule and to enter a virtual IP address, a port, and a list of HTTP/S endpoint server addresses.


.. _Create an HTTPS application with iRules:

---------------------------------------
Create an HTTPS application with iRules
---------------------------------------
The following procedure adds a FAST application using BIG-IP Next Central Manager's UI wizard.

#. Click the Workspace icon next to the F5 icon
#. At the top right of the screen, click **Add Application**.
#. Click **Create**.
#. Select one of the pre-defined iRules FAST application template **iRule-HTTPS-Load-Balancing-Service**
#. Click **Start Creating**.
#. For **Location** select a BIG-IP Next instance on which to deploy the application.
#. Enter the **Virtual Address** for the application.
#. If the **Virtual Port** is different from the default provided, change the port number.
#. Add the **Application Name** for the new application. The name will display on the Applications list.
#. Click **Next**.
#. Under **Advanced Server Addresses** click **+**.

   #. Enter the Endpoint (pool member) **Address**.
   #. Enter the server **Name**.
   #. Repeat step 4 if your application requires multiple endpoints.

#. If the **Service Port** number is different from the default, change the port number.
#. Click **Next**,
#. Click **+ Add**.

   * The panel lists all iRules configured to BIG-IP Next Central Manager.
#. Click the check box next to one or more iRules.

   * Note: You can create or clone an iRule by clicking **Create**, or select one iRule and click **Clone** to the top left of the panel. For more information about creating/cloning
   * iRules, see `Create a new iRule <cm_irules_create_how_to.html#create-a-new-irule>`_.
#. Click **Add**.

   * The iRules are added to the application's iRules list. The list displays the iRule priority order.
#. Review the iRules added to the application:

   #. Click **+Add** to add more iRules to the application.
      * The list displays iRules that have not been added, or have been removed from the application.
   #. To remove an iRule click the check box next to the iRule and click **Remove**.
   #. To change the order of the iRules in the list, select the check box next to one or more iRules and use the arrow buttons to move the iRule
      up or down the priority list.

#. Click **Next**.
#. Select a **Certificate** and click **Next**.
   * The **Summary** of the application is displayed.

#. Review the summary information. If any information is incorrect, click **Back** to edit the application details.
#. Click **Validate** to validate that the application is ready to deploy.
#. If the validation is a success, click **Deploy**.
   * If the validation finds errors. View the deployment validation results and correct the deployment issues.

The new FAST application is now in the Applications list.
