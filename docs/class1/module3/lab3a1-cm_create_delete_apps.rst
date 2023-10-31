.. Paul Kent: How do you create, modify, or delete applications using BIG-IP Next Central Manager templates? 

================================================================================
How to: Manage applications using BIG-IP Next Central Manager and FAST templates
================================================================================

Summary
=======
Use this procedure to create, modify, or delete applications using templates on the BIG-IP Next Central Manager. For more information about application observability after the application is deployed and receiving traffic, see `Overview: Application Observability <cm_application_observability_overview.html>`_.

For more information about application observability once it is deployed and receiving traffic, see `Overview: Application Observability <cm_application_observability_overview.html>`_.

What role do templates play in deploying an application?
========================================================

When you deploy an application, you use a template that defines the parameters and values you want to specify for that application. BIG-IP Next Central Manager includes a set of default templates that are designed for a variety of use cases. These templates are not considered to be ready for production use; instead, they are intended as a place to start. To use these templates, choose one that includes definitions for most of the application objects you want to deploy and then modify a copy of that template so that it exactly defines the application you want to deploy. For details on how to make a copy of a template, refer to `Manage FAST templates <cm_manage_fast_templates>`_.

One key thing to understand about what makes FAST templates so powerful is their ability to use customizable parameters to define an application. When you create a template that uses this feature, you replace static, hard coded parameters with  variables that you fill in when you use this template to deploy an application. 

For more information on FAST templates, refer to `F5 Application Services Templates <https://clouddocs.f5.com/products/extensions/f5-appsvcs-templates/latest/>`_.

For a list of FAST templates on Central Manager, see the `Template list <#fast-templates-on-central-manager>`_.

Prerequisites
=============

* Before you can create an application, you need to decide which template you’re going to use. There are three options:

  * Select an existing template and revise it so it defines the application you want to create. 
  * Clone an existing template and revise it so it defines the application you want to create.
  * Create a new template that defines the application you want to create.

    For details on how to work with templates, refer to `Manage FAST templates <cm_manage_fast_templates>`_.

* Parameter details (for example, server names or addresses, pool names, and pool member addresses or names) that are required by the application template you plan to use for this application.
* If you intend to attach a certificate to your application, you need to know the name of the certificate you plan to use. For details about managing certificates and keys, refer to: `How to: Manage Instance Certificates and Keys using BIG-IP Next Central Manager <cm_instance_certificate_and_key_management>`_.
* The IP address of the BIG-IP Next instance you plan to deploy the application to.


Procedures
==========

* :ref:`Create an application`
* :ref:`Delete an application`
* :ref:`Modify an application`


.. _Create an application:

---------------------
Create an application 
---------------------
You can use either the user interface (UI) or the application programming interface (API) to create an application.


Using the BIG-IP Next Central Manager UI
----------------------------------------

Use this procedure to deploy a new application to a managed BIG-IP Next instance from the UI.
>**Note**: The tabs and fields that display on the Create Application screen are determined by the template you choose for the application. Some templates that are included with BIG-IP Next Central Manager include on/off switches that you can use to determine whether specific fields display or not.

#. Log in to BIG-IP Next Central Manager as admin, click the Workspace icon next to the F5 logo, and then click **Applications**.
#. If this is the first application you are adding to BIG-IP Next Central Manager, click **Start Adding Apps**. Otherwise, at the top of the screen, click **Add Application**.
#. Under Add Application, select **Create**.
#. Under Create Application, For **Application Template** select the template you want to use for this application, and then click **Start Creating**.

   The Properties tab for the Create Application screen opens. 
#. For the **Location**, select the BIG-IP Next instance on which you want to deploy this application.
#. Type an **Application Name** for this application.
#. Type or select values (as appropriate) for the remaining parameters on this tab and click **Next**.
#. Specify values for the fields on the remaining tabs (use **Next** and **Back** to navigate) until you get to the Summary tab.
#. On the Summary tab, under Validate Deployment, click **Validate**.

   BIG-IP Next Central Manager performs a test deployment of your application to test the validity of the specified parameter values.
#. If the test deployment reveals any issues, resolve them and repeat the previous step.
#. When the test deployment is completely valid, click **Deploy**. 
   BIG-IP Next Central Manager deploys the application.


Using the BIG-IP Next Central Manager API
-----------------------------------------
Use this procedure to deploy a new application to a managed BIG-IP Next instance using the BIG-IP Next Central Manager API.

#. Authenticate with the BIG-IP Next Central Manager API. For details refer to `How to: Authenticate with the BIG-IP Next Central Manager API <../use_cm/cm_api_auth.html>`_.

#. Create the application by sending a Post to the ``/mgmt/shared/fast/applications`` endpoint.     

   ``POST https://<big-ip_next_cm_mgmt_ip>/mgmt/shared/fast/applications``

   For the API body, use the following, substituting values appropriate for the application you want to create.

   .. code-block:: json

    {
      "name": "Policy-Examples/HTTP-Load-Balancing-Service",
      "parameters": {
        "application_name": "basic-app1",
        "server_addresses": [
          "10.2.3.7"
        ],
         "service_port": 80,
        "tenant_name": "Tenant1",
        "virtual_address": "1.2.3.4",
        "virtual_port": 80
      },
      "target": {
        "address": "10.3.4.5"
      }
    }

Create an HTTPS application 
---------------------------

You can use either the user interface (UI) or the application programming interface (API) to create a secure application. In either case, all you do differently to make sure your application uses HTTPS is make sure to use the template named **HTTPS-Load-Balancing-Service**.


----------------------------------------
Using the BIG-IP Next Central Manager UI
----------------------------------------

Use this procedure to deploy a new https application to a managed BIG-IP Next instance from the UI.
>**Note**: The tabs and fields that display on the Create Application screen are determined by the template you choose for the application. Some templates that are included with BIG-IP Next Central Manager include on/off switches that you can use to determine whether specific fields display or not.


.. _Prerequisites:

Prerequisites
-------------
Before you can create an HTTPS application you must have an existing certificate. 

#. Log in to BIG-IP Next Central Manager as admin, click the Workspace icon, and then click **Applications**.
#. If this is the first application for this BIG-IP Next Central Manager, click **Start Adding Apps**. Otherwise, at the top of the screen, click **Add Application**.
#. Under Add Application, select **Create**.
#. Under Create Application, For **Application Template** select the template named **HTTPS-Load-Balancing-Service**, and then click **Start Creating**.

   The Properties tab for the Create Application  screen opens. 
#. For the **Location**, select the BIG-IP Next instance on which you want to create this application.
#. Type an **Application Name** for this application.
#. For **Virtual Address**, click the plus sign, then type the IP address for the application server.
#. For **Virtual Port**, type the port number of the virtual server for this application, and then click **Next**.

   The Endpoints (Pool Members) tab displays.
#. Click the **+** sign under **Advanced Server Addresses**, and then enter the IP address and name for the server this application will use. To add additional servers, click the **+** sign again.
#. For **Service Port**, type the port number used to access this application.
#. For **Load-Balancing Mode**, select the mode you want to use for this application.
#. For **Ratio**, type the numerator you want for the ratio you want to use for this application. For example for a ratio of 10/1, type in **10**.
#. For **Monitor Type**, select the monitor that you want to use for this application, then click **Next**.

   The Security tab opens.
#. For **Please choose a certificate**, select the certificate this application will use, then click **Next**.

   The Summary tab of the Create Application screen opens.
#. Under Validate Deployment, click **Validate**; 

   BIG-IP Next Central Manager performs a test deployment of your application deployment to test the validity of the specified parameter values.
#. If the test deployment reveals any issues, resolve them and repeat the previous step.
#. When the test deployment is completely valid, click **Deploy**. 

    BIG-IP Next Central Manager deploys the application.


-----------------------------------------
Using the BIG-IP Next Central Manager API
-----------------------------------------
Use this procedure to deploy a new https application to a managed BIG-IP Next instance using the BIG-IP Next Central Manager API.


Prerequisites
-------------
Before you can create an HTTPS application you must have an existing certificate.  

#. Authenticate with the BIG-IP Next Central Manager API. For details refer to `How to: Authenticate with the BIG-IP Next Central Manager API <../use_cm/cm_api_auth.html>`_.
#. Create the application by sending a Post to the ``/mgmt/shared/fast/applications`` endpoint.     

   ``POST https://<big-ip_next_cm_mgmt_ip>/mgmt/shared/fast/applications``

   For the API body, use the following, substituting values appropriate for the application you want to create. 

   For the template name field, use a template that includes a certificate, for example **HTTPS-Load-Balancing-Service**. Here's a sample body that uses a secure template.

   .. code-block:: json

    {
      "name": "Policy-Examples/HTTPS-Load-Balancing-Service",
      "parameters": {
        "application_name": "SecureApp1",
        "certificateNames": "\"SelfSigned1.crt\"",
        "certificatesEnum": "SelfSigned1",
        "serverAddresses": [
          "10.2.3.4"
        ],
        "servicePort": 80,
        "tenant_name": "SecureTenant",
        "virtualAddress": "10.3.4.5",
        "virtualPort": 80
      },
      "target": {
        "address": "10.4.5.6"
      }
    }


.. _Delete an application:

---------------------
Delete an application
---------------------
Use this procedure to remove an application that resides on a managed BIG-IP Next instance.



----------------------------------------------------
Using the BIG-IP Next Central Manager user interface
----------------------------------------------------
#. Log in to BIG-IP Next Central Manager as admin, click the Workspace icon, and then click **Applications**.
#. Select the checkbox next to the name of the application that you want to delete.
#. At the top of the screen, click (!`Delete <../images/delete.png)>`_ **Delete**.
#. In the Confirm Delete popup, click **Delete**.

   BIG-IP Next Central Manager removes the selected application.  


-----------------------------------------
Using the BIG-IP Next Central Manager API
-----------------------------------------
To delete an application using the API, you send a Delete to the ``/mgmt/shared/fast/applications/<tenant-name>/<application-name>`` endpoint.

#. Authenticate with the BIG-IP Next Central Manager API. For details refer to `How to: Authenticate with the BIG-IP Next Central Manager API <../use_cm/cm_api_auth.html>`_.
#. Delete the application by sending a Delete to the ``/mgmt/shared/fast/applications/<tenant-name>/<application-name>`` endpoint. You must include the tenant name and application name in your post.     

   ``DELETE https://<big-ip_next_cm_mgmt_ip>/mgmt/shared/fast/applications/<tenant-name>/<application-name>``

   No body is necessary for a Delete call.

.. _Modify an application:

---------------------
Modify an application
---------------------
After you deploy an application, there are some things that you cannot modify (the name of the application, the tenant, or the template used to deploy the application), but you can edit the other parameter values.

----------------------------------------
Using the BIG-IP Next Central Manager UI
----------------------------------------
Use the following procedure to modify an application using the BIG-IP Next Central Manager user interface.

#. Log in to BIG-IP Next Central Manager as admin, click the Workspace icon, and then click **Applications**.
#. Select the name of the application that you want to edit.

   BIG-IP Next Central Manager opens the application editing screen.
#. Locate the parameter(s) you want to change and select (or type) the new value.

   For example, to change the certificate this application uses, scroll to **Please choose a certificate** and select the new certificate.
#. Under Validate Deployment, click **Validate**.
#. When the test completes satisfactorily, click **Deploy** to complete your edits to this application.

   BIG-IP Next Central Manager redeploys the application, using the revised parameters that you specified. 


-----------------------------------------
Using the BIG-IP Next Central Manager API
-----------------------------------------
Use the following procedure to modify an application using the BIG-IP Next Central Manager API.
#. Authenticate with the BIG-IP Next Central Manager API. For details refer to `How to: Authenticate with the BIG-IP Next Central Manager API <../use_cm/cm_api_auth.html>`_.
#. Modify the application by sending a PATCH to the ``/mgmt/shared/fast/applications/<tenant-name>/<application-name>`` endpoint.     

   ``PATCH https://<big-ip_next_cm_mgmt_ip>/mgmt/shared/fast/applications/<tenant-name>/<application-name>``

   For the API body, use the following, substituting values appropriate for the application you want to modify. In this example, the virtual address for the application was updated:

   .. code-block:: json

    {
      "parameters": {
        "application_name": "sample-app9",
        "server_addresses": [
          "10.2.3.60"
        ],
        "service_port": 80,
        "tenant_name": "tenant1",
        "virtual_address": "1.2.3.40",
        "virtual_port": 82
      },
      "target": {
        "address": "10.4.5.6"
      }
    }   


.. _FAST templates on Central Manager:

FAST templates on Central Manager
---------------------------------
The following table shows a list of the current FAST templates available on BIG-IP Next Central Manager.

- For more information about creating HTTP applications with WAF, see `How to: Create WAF Applications Using a FAST Template on BIG-IP Next Central Manager <../waf_management/cm_create_waf_http_apps>`_.

+---------------------------------------------+---------------------------------------------------------------------------------+
| Template                                    | Description                                                                     |
+=============================================+=================================================================================+
| HTTP-Load-Balancing-Service                 | Create a basic HTTP application with load balancing. A virtual IP address       |
|                                             | and port are required along with a list of HTTP server addresses.               |
+---------------------------------------------+---------------------------------------------------------------------------------+
| http-with-waf-cloned-policy                 | Create an HTTP application with a cloned WAF policy.                            |
+---------------------------------------------+---------------------------------------------------------------------------------+
| http-with-waf-existing-policy               | Create an HTTP application with an existing WAF policy.                         |
+---------------------------------------------+---------------------------------------------------------------------------------+
| http-with-waf-new-policy                    | Create an HTTP application with a new WAF policy created using the default      |
|                                             | template.                                                                       |
+---------------------------------------------+---------------------------------------------------------------------------------+
| HTTPS-Load-Balancing-Service                | Create a basic HTTPS application with load balancing. A TLS certificate,        |
|                                             | virtual IP address, and port are required along with a list of HTTP/S           |
|                                             | endpoint server addresses.                                                      |
+---------------------------------------------+---------------------------------------------------------------------------------+
| https-with-access-policy                    | Create an HTTPS Application that uses the default template and includes an      |
|                                             | Access policy.                                                                  |
+---------------------------------------------+---------------------------------------------------------------------------------+
| iRule-Basic-Embedded-Load-Balancing-Service | Create an application with load balancing capabilities and a basic iRule. A     |
|                                             | predefined and pre-validated iRule is needed. An example iRule for redirecting  |
|                                             | to HTTPS is provided. Do not use for editing iRules. A virtual IP address and   |
|                                             | port  are required along with a list of endpoint server addresses.              |
+---------------------------------------------+---------------------------------------------------------------------------------+
| IRule-HTTPS-Load-Balancing-Service          | Create applications with load balancing capabilities and attached iRules.       |
|                                             | Attach existing, new, or cloned iRules to the application. A predefined         |
|                                             | certificate is required. A TLS certificate and an iRule are required and you    |
|                                             | must provide a virtual IP address, a port, and a list of HTTP/S endpoint server |
|                                             | addresses.                                                                      |
+---------------------------------------------+---------------------------------------------------------------------------------+
| TCP-Load-Balancing-Service                  | Create basic TCP applications with load balancing capabilities. TCP-specific    |
|                                             | ingress and egress profiles are used and can be modified. A virtual IP address  |
|                                             | and port is required, and you must provide a list of endpoint server addresses. |
+---------------------------------------------+---------------------------------------------------------------------------------+
| WAF-Secure-Load-Balancing-Service           | Create a simple WAF application with load balancing. A TLS certificate,         |
|                                             | WAF policy, virtual IP address, and port are required along with a list of      |
|                                             | HTTPS server addresses.                                                         |
+---------------------------------------------+---------------------------------------------------------------------------------+
