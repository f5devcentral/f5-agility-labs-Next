==================================================================================
How to: Migrate BIG-IP application configurations onto BIG-IP Next Central Manager
==================================================================================
This document is for users who are currently managing their applications using a BIG-IP device (version 12.1 or later) and intend to migrate their
application management services into BIG-IP Next. From BIG-IP Next Central Manager, you can use each managed BIG-IP device's UCS file to migrate the 
device's application services to a BIG-IP Next instance.

Bulk operations are not required, as you can select specific applications for migration. You can perform this process multiple times to ensure high priority applications are migrated first. An increased number of applications selected per migration will impact the time required to complete the migration process.

For a full overview of application migration, see `Migrate BIG-IP application configurations onto BIG-IP Next Central Manager <cm_device_migration_overview.html>`_.

Prerequisites
=============

* BIG-IP Next tenants are instantiated on your VELOS and/or Virtual Environment (VE).
* You must have administrative access privileges on BIG-IP Next Central Manager.  
* BIG-IP Next instances are added to BIG-IP Next Central Manager.
* A downloaded UCS file from each BIG-IP device that hosts the applications you would like to migrate. 
* Determine the required `application migration path <cm_device_migration_overview.html>`_.


--------------------------------
Application migration procedures 
--------------------------------
The application migration process provides automatic conversion of applications (HTTP or HTTPS) from BIG-IP onto BIG-IP Next.
The migration tool provides automatic object removal or replacement if these objects are not supported on BIG-IP Next. 
You are able to evaluate these changes before deployment and decide whether to convert the application with these automatic changes.

You can use the UI or `API <#application-migration-and-deployment-using-the-api>`_ to migrate and deploy applications to BIG-IP Next.


.. _Migration from BIG-IP into BIG-IP Next (UI):
.. _Migrate a BIG-IP UCS to BIG-IP Next (UI):

Migration from BIG-IP into BIG-IP Next (UI)
-------------------------------------------
Migrate a BIG-IP UCS file into BIG-IP Next

* :ref:`Migrate a BIG-IP UCS to BIG-IP Next (UI)`


.. _Application deployment to BIG-IP Next (UI):

Application deployment to BIG-IP Next (UI)
--------------------------------------------
Once you migrate the UCS, select whether to deploy applications with a FAST template or as an AS3 application:

* `Application templates (FAST) deployment`
* `AS3 application deployment`

- Note: FAST deployment supports HTTP application services without iRules. If your application services are HTTPS or contain iRules, use the AS3 deployment method.


.. _Application migration and deployment using the API:

Application migration and deployment using the API
--------------------------------------------------
You can migrate and deploy applications using the API for BIG-IP Next Central Manager.

Once you migrate applications, you can select whether to deploy your application services as API (AS#. or Template (FAST) based applications.

The following are general procedures for migrating and then deploying either FAST or AS3 declarations to BIG-IP Next. For more information about API for application migration, see `OpenAPI <https://clouddocs.f5networks.net/products/big-iq/mgmt-api/v0.0.1/ApiReferences/bigip_public_api_ref/r_openapi-next.html#tag/JourneysFeature>`_.

* `Migrate application services to BIG-IP Next - API`


.. _Migrate a BIG-IP UCS to BIG-IP Next (API):

Migrate a BIG-IP UCS to BIG-IP Next
-----------------------------------
This procedure creates applications services for each virtual server found on the uploaded UCS. Shared objects from the source BIG-IP, such as iRules and endpoints (pool members), are copied into each application service as a separate entity.

Objects or entities that are not supported on BIG-IP Next are automatically modified and removed during the application conversion. You will receive a `status notification <cm_device_migration_statuses.html>`_ regarding migration readiness of every application service (statuses yellow, blue, green).

#. Log in to BIG-IP Next Central Manager as admin, click the Workspace icon next to the F5 icon, and click **Applications**.
#. At the top of the screen, click **Add Application**.
#. From the Migrate Application(s) area, click **New Migration**.

   - Note: If you have already imported your applications, but have not completed the migration process, click **Resume Migration** to select an existing migration session.
#. For **General Properties**, type a **Session Name** (without whitespace) and an optional **Description**.
#. Click **Save & Continue**. 
#. For **Source BIG-IP System** add the UCS file and customize encryption and merge preferences for the BIG-IP virtual servers:

   #. In the UCS File area, upload your UCS file by dragging and dropping in the upload area, or click **browse** to select the file from your local system.
   #. (Optional) Enable **Master Key** to apply a master key using BIG-IP Next Central Manager. The master key is used to decrypt and encrypt fields and files in the source configuration.
      If you do not apply a master key, you might not be able to migrate an application that uses SSL certificates and keys (e.g. HTTPS) to BIG-IP Next instances.

      #. Enter the **Master Key** password.
   #. (Optional) Enable **Encrypted UCS Archive** to provide password encryption for the UCS.

      #. Enter the **Encrypted UCS Archive** password.
   #. Click **Save & Continue**.

#. For **Application Migration** select the applications you want to migrate. The following procedure allows you to select the virtual servers for migration. 

   #. Click **Add Application** from the center right of the panel. This action opens the BIG-IP Applications panel, which lists the services (virtual servers) found on the UCS file.

      Each row specifies the virtual server (service), IP address, and last date modified. The `status <cm_device_migration_statuses.html>`_ indicates whether the application's virtual servers contain unsupported objects |yellow| and/or unsupported security objects |blue|. 
      The migration tool replaces or removes objects that are not supported by AS3 or BIG-IP next. 
      You can view migration changes (see next step). 

      *Note*: An application can be converted regardless of its status.

   #. (Optional) If the status is not green, you can verify which objects in the services (virtual servers) will be automatically replaced or removed during the application conversion:

      #. Click the name of the service.
      #. Select a configuration file from the panel menu and click **Save & Verify**. This creates a preview that highlights objects in the declaration that will be automatically removed or replaced during the conversion to BIG-IP Next.
      #. Click **Yes, continue** to verify the file. Depending on the size of the file, this may take up to 15 minutes.
      #. Click **Preview AS3 </>** to view the service after it is converted to BIG-IP Next.
      #. Click **Close** to close the service panels and return to the BIG-IP Application Services panel.

   #. Select the check box of the services you would like to migrate.
   #. Click **Add**. The services are added to the Application Migration list.
   #. Click **Save & Continue**.

You have completed the migration process, and are now ready for `pre-deployment <#deploy-migrated-applications>`_. You can migrate each selected application service on
a `FAST template](#application-templates-fast-migration) or all selected application services on an [AS3 declaration <#as3-application-migration>`_.


.. _Deploy migrated applications:

Deploy migrated applications
----------------------------
When you completed UCS upload from the source BIG-IP and select application services you can prepare for deployment to a BIG-IP Next instance. Ensure you
have completed all steps in :ref:`Migrate a BIG-IP UCS to BIG-IP Next (UI)`.


.. _Application templates (FAST) deployment:

Application templates (FAST) deployment
---------------------------------------
Application services selected in **Application Migration** are assigned a FAST template that is currently configured to BIG-IP Next Central Manager.

#. In **Pre-deployment** select the **Application Templates** deployment method.
#. Select an **Application Template** and **Deploy Location** (BIG-IP Next instance) for each application service.

   - Note: The FAST templates provided are default and custom templates that are currently configured to BIG-IP Next Central Manager. 
   - If you require a new template, you can click **Save & Finish** and resume the migration session at another time. 
   - See `How to: Manage FAST templates for a BIG-IP Next instance on BIG-IP Next Central Manager <cm_manage_fast_templates.html>`_ for creating FAST templates.

#. If a warning appears next to the application service name, this indicates that there are required fields in the selected FAST template that do not exist in the migrated application service:

    #. Click the name of the application service.
    #. Add the required properties.
    #. Click **Next**
    #. Click **Validate** to ensure the changes can be deployed.
    #. Click **Save & Exit**.

#. Click **Deploy**.  This deploys the applications to the BIG-IP Next instance. You can see the application deployment status in the Deployments list.
#. To view deployment details:

   #. Click the deployment's **Status**.
   #. Click **View Logs** to the top of the deployment summary panel. The log displays the general activities and failures during the deployment process.


.. _AS3 application deployment:

AS3 application deployment
--------------------------
Application services selected in **Application Migration** are prepared as a single AS3 declaration and deployed to a single BIG-IP Next instance (deploy location). Once you complete the application conversion process, you can view iRules associated with the application service, and download the converted AS3 declaration.

#. In **Pre-deployment** select the **AS3** deployment method.
#. Select the BIG-IP Next instance for **Deploy Location**.
#. To download the converted declaration, click **Download AS3**.
#. To preview iRules in the converted service:

   #. Click the number in the iRule column for each application service. This opens a panel with a list of custom iRules in the application service.
   #. Click the name of the custom iRule in the service to preview before deployment. 

      - *Note*: It is possible to edit the iRule, ensure that your changes are valid to prevent issues in deployment.
      - For more information about iRules, see `iRules Home <https://clouddocs.f5.com/api/irules/>`_.
      - *Important*: Ensure that you save the changes before you return to the summary screen. You might lose your work if you navigate away from the editor.
   #. Click **Save & Close**.
#. Click **Deploy**.

   - This deploys the applications to the BIG-IP Next instance. You can see the application deployment status in the Deployments list. 
   - You can click the status to view the deployment summary and log.  
   - *Note*: If you are not ready to deploy, click **Save & Finish** to save the migration session and deploy later.


Migrate application and deploy services to BIG-IP Next - API
------------------------------------------------------------
The following lists the steps required to complete a general migration:

* :ref:`Create a new session for application service migration`
* :ref:`Provide source configuration (UCS)`
* :ref:`Preview virtual server list`
* :ref:`Stage virtual servers (application services)`

Use the following procedures to deploy migrated application services:

* :ref:`Generate and deploy an AS3 declaration for application services`.


.. _Create a new session for application service migration:

Create a new session for application service migration
------------------------------------------------------

Initiate the migration process by establishing new session and generating a session ID:

``POST https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/sessions``

Sample request:

.. code-block:: json

 {
   "name": "session",
   "description": "optional description"
 }


Sample successful response:

.. code-block:: json

 {
   "last_update_time": "2021-01-28T15:24:05.029102Z",
   "id": 0,
   "url": "sessions/{session_id}",
   "name": "session",
   "description": "optional description"
 }


.. _Provide source configuration (UCS):

Provide source configuration (UCS)
----------------------------------
Add the configuration (UCS file) from the source BIG-IP system to the new session. Use the session ID generated from :ref:`Create a new session for application service migration`. Add the `ucs_file` to the body of the request. Include the `ucs_passphrase` and/or `master_key` if the UCS file is password protected. 


``POST https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/sessions{session_id}/source``


.. _Preview virtual server list:

Preview virtual server list
---------------------------
Request a list of virtual servers found on the UCS. 

``GET https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/sessions/{session_id}/virtuals``

Preview a specific virtual server:

``GET https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/sessions/{session_id}/virtuals/{virtual_name}``


.. _Stage virtual servers (application services):

Stage virtual servers (application services)
--------------------------------------------
Stages all virtual servers for the migration session:

``PUT https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/sessions/{session_id}/bulk_stage``

To stage specific virtual server:

``POST https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/sessions/{session_id}/virtuals/{virtual_name}/stage``


..
  ## Deployment methods:
   
  Choose the deployment method:
  :ref:`Generate and deploy application services with FAST templates`.
  :ref:`Generate and deploy an AS3 declaration for application services`.
   
  .. _Generate and deploy application services with FAST templates:
    
  Generate and deploy application services with FAST templates
  ------------------------------------------------------------
  Get a list of supported FAST templates:
   
  ``GET https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/fast/supported_templates``-->


.. _Generate and deploy an AS3 declaration for application services:

---------------------------------------------------------------
Generate and deploy an AS3 declaration for application services
---------------------------------------------------------------
Create an AS3 declaration that can be deployed to a BIG-IP Next instance:

``POST https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/sessions/{session_id}/output``

Deploy the AS3 declaration to a BIG-IP Next instance. Ensure the `instance_address` (IP address) is included in the body of the post...  verify

``POST https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/sessions/{session_id}/deployments``

The deployment request returns a `deployment_id`. You can check the progress of your deployment with the following request:

``POST https://<BIG-IP-Next-Central-Manager-IP-Address>/api/journeys/v1/sessions/{session_id/}deployments/{deployment_id}``


.. |yellow| image:: ../images/migration-invalid-object-icon.png

.. |blue| image:: ../images/migration-invalid-sec-object-icon.png

