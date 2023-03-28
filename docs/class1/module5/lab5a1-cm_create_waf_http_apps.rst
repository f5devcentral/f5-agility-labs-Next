============================================================================================
How to: Create WAF Applications Using an Application Template on BIG-IP Next Central Manager
============================================================================================

Summary
=======
Use these procedures to create an HTTP or HTTPS application with a Web Application Firewall (WAF) security policy. 

To create an application with WAF protection, you need to use an application template. BIG-IP Next Central Manager provides three HTTP FAST application templates for HTTP and one template for HTTPS. If required, you can create a new application template that includes a WAF policy. 

After you create the application, you deploy the declaration and the policy to a BIG-IP Next instance. Any changes
made to the WAF policy on BIG-IP Next Central Manager can be re-deployed to all associated instances.  

*Note*: For more information about using BIG-IP Next Central Manager for creating applications using a template, see `How to: Manage applications using Templates (UI) <cm_create_delete_apps.html>`_.


Prerequisites
=============

* Before you can create an application, you need to decide which template you’re going to use. 

  - If you plan to use a custom application template, you must create a template before creating the application. See :ref:`Add WAF protection to a custom application template` for creating a new WAF-protected HTTP application template.
* If you plan to use an existing or cloned WAF policy, you must configure the policy in advance. To create or import a policy, see `How To: Configure WAF Policy on BIG-IP Next Central Manager <cm_awaf_how_to_create_policy.html>`_
* One or more BIG-IP Next instances are discovered on BIG-IP Next Central Manager. Ensure you have the IP address of the BIG-IP Next instance you plan to deploy the application to.
* Required application and network parameter details, for example: server names or addresses, pool names, and pool member addresses or names.

Procedures
==========

* :ref:`Create an HTTP application with an existing or cloned WAF policy`
* :ref:`Create an HTTP application with a new WAF policy`
* :ref:`Create an HTTPS application with a WAF policy`
* :ref:`Add WAF protection to a custom application template`


.. _WAF Application Templates:

-------------------------
WAF Application Templates
-------------------------
BIG-IP Next Central Manager provides three templates for creating a WAF-protected HTTP application.  
If the templates provided do not suit the needs of your application, you can create a new application template with WAF protection.

| **http-with-waf-existing-policy** - Use this template to create an HTTP application that includes a WAF policy that is already configured on BIG-IP Next Central Manager.
| **http-with-waf-cloned-policy** - Use this template to create an HTTP application with a WAF policy that is cloned from an existing policy on BIG-IP Next Central Manager. This option allows you to make changes to the WAF policy without impacting other deployed applications with the same WAF policy.
| **http-with-waf-new-policy** - Use this template to create an HTTP application with a new WAF policy.
| **WAF-Secure-Load-Balancing-Service** - Use this template to create an HTTPS application with an existing, new, or cloned WAF policy. 

For more information about how to create and customize a WAF policy in BIG-IP Next Central Manager, see `How To: Configure WAF Policy on BIG-IP Next Central Manager <cm_awaf_how_to_create_policy.html>`_



.. _Create an HTTP application with an existing or cloned WAF policy:

--------------------------------------------------------------------
**Create an HTTP application with an existing or cloned WAF policy**
--------------------------------------------------------------------
Use this procedure to deploy a new HTTP application, with an exiting WAF policy, to BIG-IP a Next instance. This procedure creates an application that includes a WAF policy currently configured on BIG-IP Next Central Manager.


#. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Applications**.

   - The screen lists all your applications in My Apps.
#. At the top of the screen, click **Add Applications**.
#. Select one of the following **Templates**: **http-wth-waf-existing-policy** or **http-with-waf-cloned-policy**. You can type key words from the template name to filter the selection.
#. Click **Start Creating**.
#. For the **Location**, select the BIG-IP Next instance on which you want to create this application.
#. If you selected **http-with-waf-cloned-policy**, type a **Cloned WAF Policy Name** for the WAF policy you are creating.  
#. From **WAF Policy Name** select a WAF policy configured on BIG-IP Next Central Manager.
#. Type an **Application Name** to identify the new application service.
#. Click **+** and enter your pool member server's IP address.

   - Note: You need to add at least one server IP address. You can add server addresses by clicking **+**. 
#. Enter the application's **Service Port** number. This field might be automatically populated from the selected template. ..  Verify 
#. Type a **Tenant Name** to specify the partition on which the application is deployed within the BIG-IP Next instance. You must specify a tenant to create and deploy an application.
#. Enter the **Virtual Address** for the application. ..  TO DO: what is this? Also, why is it virtual addresses in the UI?
#. Enter the **Virtual Port** number. This field may be automatically populated from the selected template.
#. Click **Next** to view a summary of the application's details and validate the application's deployment.
#. Click **Validate** to test whether the application is ready to deploy to a BIG-IP Next instance.

   BIG-IP Next Central Manager performs a trial run of your application deployment to test the validity of the specified parameter values.

   - If the test deployment reveals any issues, resolve them and repeat the previous step.
#. When the test deployment is successful, click **Deploy**.

   BIG-IP Next Central Manager deploys the WAF-protected application to the target BIG-IP Next instance. In addition, the new WAF policy is  added to BIG-IP Next Central Manager and deployed to the instance.


.. _Create an HTTP application with a new WAF policy:

**Create an HTTP application with a new WAF policy**
----------------------------------------------------
Use this procedure to deploy a new HTTP application, with a new WAF policy, to a BIG-IP Next instance. When you create a new policy using this application template, the new policy is added to BIG-IP Next Central Manager.

#. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Applications**.

   - The screen lists all your applications in My Apps.
#. At the top of the screen, click **Add Application**.
#. Click **Create**.
#. Select the **Templates**: **http-with-waf-new-policy**. You can type key words from the template name to filter the selection.
#. Click **Start Creating**.
#. For the **Location**, select the BIG-IP Next instance on which you want to create this application.
#. Type a unique **New WAF Policy Name** that identifies your WAF policy on BIG-IP Next Central Manager. Ensure the policy name is unique to all WAF policy names in your BIG-IP Next configuration. You will not be able to deploy the application with a duplicated policy name. 
#. Select a **WAF Policy Template**. For more information about supported policy templates, see `Reference: WAF Policy Templates <awaf_policy_template_description.html>`_. 

#. Type an **Application Name** to identify the new application service.
#. Click **+** and enter your pool member server's IP address.

   -  Note: You need to add at least one server IP address. You can add server addresses by clicking **+ Add Server Addresses**. 
#. Enter the application's **Service Port** number. This field might be automatically populated from the selected template. 
#. Type a **Tenant Name** to specify the partition on which the application is deployed within the BIG-IP Next instance. You must specify a tenant to create and deploy an application.
#. Enter the **Virtual Address** for the application. ..  TO DO: what is this? Also, why is it 'virtual addresses' in the UI?
#. Enter the **Virtual Port** number. This field might be automatically populated from the selected template. ..  Verify
#. Click **Next** to view a summary of the application's details and validate the application's deployment.
#. Click **Validate** to test whether the application is ready to deploy to a BIG-IP Next instance.

   BIG-IP Next Central Manager performs a trial run of your application deployment to test the validity of the specified parameter values.
   - If the test deployment reveals any issues, resolve them and repeat the previous step.
#. When the test deployment is successful, click **Deploy**.

   - BIG-IP Next Central Manager deploys the WAF-protected application to the target BIG-IP Next instance. In addition, the new WAF policy is  added to BIG-IP Next Central Manager and deployed to the instance.


.. _Create an HTTPS application with a WAF policy:

**Create an HTTPS application with a WAF policy**
-------------------------------------------------
Use this procedure to deploy an HTTPS application, with a new WAF policy, to a BIG-IP Next instance. If you are cloning or creating a WAF policy using this application template, the new policy is added to BIG-IP Next Central Manager.

#. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Applications**.

   - The screen lists all your applications in My Apps.
#. At the top of the screen, click **Add Application**.
#. Click **Create**.
#. Select the **Templates**: **WAF-Secure-Load-Balancing-Service**. You can type key words from the template name to filter the selection.
#. Click **Start Creating**.
#. For the **Location**, select the BIG-IP Next instance on which you want to create this application.
#. Type an **Application Name** to identify the new application service.
#. Enter the **Virtual Address** for the application. ..  TO DO: what is this? Also, why is it 'virtual addresses' in the UI?
#. Enter the **Virtual Port** number. This field might be automatically populated from the selected template. ..  Verify
#. Click **Next** to add endpoints (pool memebers).
#. Click **+** and enter your pool member server's IP address.

   - Note: You need to add at least one server IP address. You can add server addresses by clicking **+ Add Server Addresses**. 
#. Enter the application's **Service Port** number. This field might be automatically populated from the selected template. 
#. Click **Next** to add a certificate and WAF policy.
#. From **Please choose a certificate** select a certificate. For more information about adding certificates in BIG-IP Next Central Manager, see `How to: Manage certificates and keys for a BIG-IP Next instance using BIG-IP Next Central Manager <cm_instance_certificate_and_key_management.html>`_.
#. If you are using an existing WAF policy select a **WAF Policy Name**.
#. If you are creating a new WAF policy:

   #. Click **Create**. The panel displays general settings for the WAF policy. For more information about managing a WAF policy, see `How To: Manage and edit a WAF policy on BIG-IP Next Central Manager <cm_awaf_manage_edit_policy.html>`_.
   #. Type a policy **Name** and an optional **Description**.
   #. Add **Tags** if you would like to filter your policy according to keywords.
   #. Select a Template for your WAF policy. For more information about supported policy templates, see `Reference: WAF Policy Templates <awaf_policy_template_description.html>`_. The template will populate the required fields within the new policy.
   #. Click **Save**
#. If you are cloning a WAF policy:

   #. Select a **WAF Policy Name**.
   #. Click **Clone**. 
   #. Type a new **Name** for the cloned policy and an optional **Description**.
   #. You can de-select **Clone Tags** to remove the tags from the cloned policy.
   #. Click **Clone**.   
#. Click **Next** to view a summary of the application's details and validate the application's deployment.
#. Click **Validate** to test whether the application is ready to deploy to a BIG-IP Next instance.

   BIG-IP Next Central Manager performs a trial run of your application deployment to test the validity of the specified parameter values.
   - If the test deployment reveals any issues, resolve them and repeat the previous step.

#. When the test deployment is successful, click **Deploy**.

   - BIG-IP Next Central Manager deploys the WAF-protected application to the target BIG-IP Next instance. In addition, the new WAF policy is  added to BIG-IP Next Central Manager and deployed to the instance.


.. _Add WAF protection to a custom application template:

**Add WAF protection to a custom application template**
-------------------------------------------------------
Use this procedure to add a WAF policy to a customized application template. This procedure provides allows you to add a new, cloned, or existing WAF application to a custom FAST template.


#. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **App Templates**.
#. Select a template to customize by clicking the name of the template. You can use any template, but the following already include definitions and parameters for a WAF policy:

   #. Template with existing WAF policy:**http-wth-waf-existing-policy**. For required definitions and parameters, see :ref:`Custom Template with Existing WAF Policy`.
   #. Template with option to clone a WAF policy: **http-with-waf-cloned-policy**. For required definitions and parameters, see `Custom Template with Cloned WAF Policy`.
   #. Template with option to create a new WAF policy **http-with-waf-new-policy**. For required definitions and parameters, see `Custom Template with New WAF Policy`.

      - BIG-IP Next Central Manager opens the template for editing.
#. Copy all of the content in the **Template Content** box.
#. Click **Cancel** to close the template editing screen.
#. At the top of the screen, click **Create**.

   The Create New Template page opens.
#. Type a **Name** and an optional **Description** in their respective fields.
#. In the **Template Content** text area, paste in the FAST template content that you copied to the clipboard.
#. Revise the FAST template code so that it describes the application you want to deploy based on the template you copied. Depending on the type of WAF policy (existing, cloned, new), ensure that the following definitions and parameters are included within your template.


.. _Custom Template with Existing WAF Policy:

**Custom Template with Existing WAF Policy**
--------------------------------------------
The following fields are required within application template definitions. See the following example:

WAFPolicyName:

 | type: string
 | title: WAF Policy Name
 | preProcessCM: wafPolicyList

Once you have added **WAFPolicyName** to the definitions, ensure the **preDeployCM** parameters are added. This ensures that the WAF policy on BIG-IP Next Central Manager is deployed to the BIG-IP Next instance. See the following example that applies an HTTP application:


.. code-block:: json

 {
   "serviceMain": {
     "class": "Service_HTTP",
     "policyWAF": {
       "bigip": "preDeployCM:wafPolicy:{'policy_name'='{{WAFPolicyName}}'}"
     }
   }
 }


.. _Custom Template with Cloned WAF Policy:

**Custom Template with Cloned WAF Policy**
------------------------------------------
The following fields are recommended within application template definitions. See the following example:
*Note*: **ClonedWAFPolicyName** is not required, but recommended as it organizes the cloned policy within BIG-IP Next Central Manager's list of policies. 


WAFPolicyName:
 |   type: string
 |   title: WAF Policy Name
 |   preProcessCM: wafPolicyList
 |
ClonedWAFPolicyName: 
 |   type: string
 |   title: Cloned WAF Policy Name

After you have added **WAFPolicyName** to the definitions, ensure **preDeployCM** is specified. This ensures that the WAF policy on BIG-IP Next Central Manager is deployed to the BIG-IP Next instance. See the following example that applies an HTTP application:


.. code-block:: json

 {
   "serviceMain": {
     "class": "Service_HTTP",
     "policyWAF": {
       "bigip": "preDeployCM:cloneWafPolicy:{'policy_name'='{{WAFPolicyName}}','cloned_policy_name'='{{ClonedWAFPolicyName}}'}"
     }
   }
 }


.. _Custom Template with New WAF Policy:

**Custom Template with New WAF Policy**
---------------------------------------
The following fields are recommended within application template definitions.  See the following example:

- Note for this version of BIG-IP Next Central Manager, the only WAF policy template available is Moderate-Protection. For **WAFPolicyTemplate**, the **default** field must be Moderate-Protection.



WAFPolicyName:
 |   type: string
 |   title: New WAF Policy Name
 |
WAFPolicyTemplate: 
 |   type: string
 |   title: WAF Policy Template
 |   default: Moderate-Protection

After you have added **WAFPolicyName** to the definitions, ensure **preDeployCM** is specified. This ensures that the WAF policy on BIG-IP Next Central Manager is deployed to the BIG-IP Next instance. See the following example that applies an HTTP application:


.. code-block:: json

 {
   "serviceMain": {
     "class": "Service_HTTP",
     "policyWAF": {
       "bigip": "preDeployCM:newWafPolicy:{'policy_name'='{{WAFPolicyName}}', 'template_name'='{{WAFPolicyTemplate}}'}"
     }
   }
 }


..
 TO DO: For GA ## Update a WAF policy on a deployed HTTP application
 Use this procedure once you have made changes to a WAF policy that is currently deployed on one or more applications. Your changes will be deployed to all applications that include this policy, so ensure that the changes meet 
 the required security for all relevant applications. 

..
 TO DO: Do we need this here? ## Delete an application
 
 Use this procedure to remove an application that resides on a managed BIG-IP Next instance.
 
 #. Log in to BIG-IP Next Central Manager as admin, click the workspace switcher next to the F5 icon, and click **Applications**.
 #. Select the checkbox next to the name of the application that you want to delete.
 #. At the top of the screen, click (!`Create <images/delete.png)>`_  **Delete**.
 #. In the Confirm Delete popup, click **Delete**.
  
    BIG-IP Next Central Manager removes the selected application.
