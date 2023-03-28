==============================================
How to: Make your FAST templates easier to use  
==============================================
There are two sections to a template that BIG-IP Next Central Manager uses when deploying an application associated with the template: 

* Definitions: specifies how each field displays when you use the template.
* Template: defines the content for the fields in the template.    

What is the definitions section in a template? 
==============================================
At the top of a BIG-IP Next Central Manager application template, you can add (or modify existing) optional definitions to make that template easier to use. This definitions section controls the appearance of the fields in the create application wizard. You can control the order of appearance of the fields, set default values, and validate field values. Add these definitions to make it easier to use your template.

How do I create new definitions for an application template?
============================================================
You add definition entries to a template near the top of the template. The definitions section begins with the entry: ``definitions:`` and ends with the start of the template body which begins with the entry: ``template: |``. 
**Note**: For details about how to edit a template refer to: `How to: Manage applications using BIG-IP Next Central Manager and FAST templates <cm_create_delete_apps.html>`_.

Here are some example definitions for several variable fields. The entries in these definitions specify how fields display and what constitutes a valid entry for each field when you use this template. The example is followed by a table that explains how the entries that comprise the definition are used, and an illustration of how this field displays in the Create Application wizard.

Example field definition
========================

**virtualPort**:

  |  title: Virtual Port
  | description: Port of the virtual server
  | type: integer
  | default: 80
  | minimum: 0
  | maximum: 65535

**application_name**:

  |  title: Application Name

**accessPolicyEnum**:

  |  title : Access Policy Name
  |  description: Select the Access policy.
  |  type: string
  |  preProcessCM: accessPolicyList

**accessAdditionalConfigurations**:

  |  title : Access Additional Configurations (For internal use. DO NOT MODIFY)
  |  description: Retrieves Access Internal Configurations (For internal use. DO NOT MODIFY).
  |  type: string
  |  default: " "

**certificatesEnum**:

  |  title: Please choose a certificate
  |  type: string
  |  preProcessCM: certificateNames

**central_manager**:

  |  preDeployCM.accessAdditionalConfigurations:

     |  accessAdditionalConfigurations:

        |  command: accessAdditionalConfigurations:{'policyName'= '{{accessPolicyEnum}}'}

  |  preDeployCM.accessPolicy:

     |  accessPolicyEnum:

       |  command: accessPolicy:{'policyNamthis definitione'= '{{accessPolicyEnum}}'}

  |  preDeployCM.certificate:

     |  certificatesEnum:

       |  command: deployCertAndKey:{'certificate_name'='{{certificatesEnum}}'}



*Note*: When you specify field definitions for a template, your entries must be indented correctly. If you indent an entry incorrectly, the **Template Body** box changes from black to red to indicate that BIG-IP Next Central Manager will ignore it. When you correct the indent, the box color changes back to black.  

Field explanations
==================
+---------------+---------------------------------------------------------------------------------------------------+
|Entry          | Explanation                                                                                       |
+===============+===================================================================================================+
| field-name    | Displays the name of the mustache variable in the template. The remaining entries for this        |
|               | definition are all indented. The field name for the definition displays first, followed by the    |
|               | optional, indented entries.The field name for this entry in the example definition is             |
|               | **virtualPort**.                                                                                  |
+---------------+---------------------------------------------------------------------------------------------------+
| title         | Specifies the field title that appears in the Create Application wizard.The title that displays   |
|               | on the Create Application window for the example definition is **Virtual Port**.                  |
+---------------+---------------------------------------------------------------------------------------------------+
| description   | Specifies the tooltip entry for this definition. When you hover your cursor over the information  |
|               | icon next to this field in the UI, this text displays. The tooltip entry for the example          |
|               | definition is **Port for the pool members**.                                                      |
+---------------+---------------------------------------------------------------------------------------------------+
| type          | Specifies the type requirements for the field described by this definition, for example string or |
|               | number. The type requirement for this field in the example definition is **number**.              |
+---------------+---------------------------------------------------------------------------------------------------+
| pattern       | Specifies the regular expression that BIG-IP Next Central Manager uses to validate entries for    |
|               | the field described by this definition. This field is not present in the example definition.      |
+---------------+---------------------------------------------------------------------------------------------------+
| default       | Specifies the default value for the field.The default port in the example definition is **80**.   |
+---------------+---------------------------------------------------------------------------------------------------+
| minimum       | Specifies the minimum acceptable value for the field. The minimum acceptable value in the example |
|               | definition is **0**.                                                                              |
+---------------+---------------------------------------------------------------------------------------------------+
| maximum       | Specifies the maximum acceptable value for the field. The maximum acceptable value in the example |
|               | definition is **65535**.                                                                          |
+---------------+---------------------------------------------------------------------------------------------------+
| disableEdit   | When this entry is set to true, you can only specify a value for this field while you are         |
|               | creating the application. After you create the application, you won't be able to edit it. This    |
|               | field is not present in the example definition.                                                   |
+---------------+---------------------------------------------------------------------------------------------------+
| preProcessCM  | Specifies that BIG-IP Next Central Manager gets the list of existing Access policies or TLS       |
|               | certificates, and displays a selection list in the Create Application wizard.                     |
|               | **preProcessCM: accessPolicyList** specifies that BIG-IP Next Central Manager gets the list of    |
|               | existing Access policies, and displays a selection list in the Create Application wizard.         |
|               | **preProcessCM: certificateNames** specifies that BIG-IP Next Central Manager gets the list of    |
|               | existing certificates, and displays a selection list in the Create Application wizard.            |
|               | If you are creating a new template that includes Access policies or TLS certificate references,   |
|               | you must include both preDeployCM and preProcessCM in one of your definitions.                    |
+---------------+---------------------------------------------------------------------------------------------------+
| uiMetaCM      | There are several components to this entry:                                                       |
|               | **page**: Specifies which tab this field displays on. If omitted, the field displays on the       |
|               | Properties tab.                                                                                   |
|               | A page entry of **0**, specifies that this field displays on the first tab following the          |
|               | always-present Properties tab.                                                                    |
|               | **order**: Specifies the order in which fields display on the tab. If omitted, the field displays |
|               | in the order in which is is listed in the template. An order entry of 1 specifies that this field |
|               | appears as the second field on the tab. **pageName** defines the name of the tab on which this    |
|               |  field appears. You only need to define the tab name the first time you use it. If you forget, the|
|               | page number displays instead.**field: EndpointsField** This entry is used as part of the server   |
|               | field. When you include this field, BIG-IP Next Central Manager changes the way you specify pool  |
|               | members for an application. When you include this field in the servers field, for the first pool  |
|               | member, an **Add New Endpoints** button displays. As you add pool members, they are listed as     |
|               | clickable links that you can navigate to view details for each pool member. Then, to add          |
|               | additional pool members, you click the **Add** button. If you omit this field, you can still add  |
|               | pool members, but they will not be listed as clickable links; instead, just the IP address and    |
|               | name for each pool is displayed. This field is not present in the example definition.             |
|               | **SelectCreateCloneField** Specifies that BIG-IP Next Central Manager provides the user with a    |
|               | choice to either select, clone, or create a WAF policy.This field is not present in the example   |
|               | definition.                                                                                       |
|               | **widget:textarea** Specifies that BIG-IP Next Central Manager creates a multi-line text box for  |
|               | the field.This field is not present in the example definition.                                    |
+---------------+---------------------------------------------------------------------------------------------------+
| preDeployCM   | Specifies that BIG-IP Next Central Manager deploys a list of required objects to the target       |
|               | instance before it deploys the application. The preDeployCM entry must use the name of the        |
|               | variable identified in the preProcessCM entry. If you are creating a new template that includes   |
|               | Access policies, WAF policies, or TLS certificate references, you must include preDeployCM and    |
|               | preProcessCM in one of your definitions. There are multiple entries supplied in the example       |
|               | definition.The following entry specifies that the BIG-IP Next Central Manager deploys the Access  |
|               | policy to the instance as a pre-requisite to application deployment.                              |
|               | **preDeployCM.accessPolicy**: accessPolicyEnum:                                                   |
|               | command: accessPolicy:{'policyName'= '{{accessPolicyEnum}}'}                                      |
|               | The following entry specifies that the BIG-IP Next Central Manager deploys the WAF policy to the  |
|               | instance as a pre-requisite to application deployment.                                            |
|               | **preDeployCM.wafPolicy**: wafPolicyEnum:                                                         |
|               | command: wafPolicy:{'policyName'= '{{wafPolicyEnum}}'}                                            |
|               | The following entry specifies that the BIG-IP Next Central Manager deploys the certificate to the |
|               | instance as a pre-requisite to application deployment: **preDeployCM.certificate**:               |
|               | certificatesEnum: command: deployCertAndKey:{'certificate_name'='{{certificatesEnum}}'}**         |
+---------------+---------------------------------------------------------------------------------------------------+



This screenshot shows how the Virtual Port field appears in the Create Application wizard when these definitions are used. The tooltip displays because the cursor is hovering over the Service Port information icon !`information icon <../images/info-icon.png>`_.

!`Create Application Screenshot <../images/app-wizard-virtual-port.png>`_
