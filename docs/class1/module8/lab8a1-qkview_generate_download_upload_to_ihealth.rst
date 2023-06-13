=================================================================================================================
How to: Use BIG-IP Next Central Manager to generate and upload to iHealth a QKView file for BIG-IP Next instances
=================================================================================================================

Overview
========

You can generate a QKView file for a BIG-IP Next instance (either standalone or HA configuration), which is automatically uploaded to F5 iHealth.  Additionally, you can download the QKView file directly from BIG-IP Next Central Manager for a limited time after it is generated.

*Important*: The following procedures are intended for BIG-IP Next Central Manager. If you are running BIG-IP Next Central Manager, you will not be able to 
generate a QKView directly on your BIG-IP Next instance. If you manage your BIG-IP Next instances directly, use `How to: Generate & download a QKView file for BIG-IP Next and upload to F5 iHealth or Support <qkview_generate_download_upload_to_ihealth.html>`_ to generate and download a QKView file for manual upload to iHealth.


Prerequisites
=============

- BIG-IP Next instances are added to BIG-IP Next Central Manager.
- If generating a QKView task for BIG-IP Next HA instances, identify whether you are generating a QKView file for the active or standby node, or both. 
- `F5 iHealth <https://www.f5.com/services/support/support-offerings/big-ip-ihealth-diagnostic-tool>`_ account credentials.
- (Optional) A F5 Support case number.


Procedures
==========
You can generate and download a QKView file that is automatically uploaded to iHealth for a BIG-IP Next standalone or HA instance using the BIG-IP Next Central Manager's UI or API:

* :ref:`Generate a QKView file for a BIG-IP Next instance`
* :ref:`Download a QKView file`
* :ref:`Delete a QKView file log entry`


.. _Generate a QKView file for a BIG-IP Next instance:

**Generate a QKView file for a BIG-IP Next instance**
-----------------------------------------------------
When generating a QKView file from BIG-IP Next Central Manager, the generated file is automatically
uploaded to your iHealth account. 

If you have not yet created a iHealth account, you need to create one before you proceed. Go to `F5 iHealth <https://www.f5.com/services/support/support-offerings/big-ip-ihealth-diagnostic-tool>`_ to create an account.

Once the QKView file generation process is complete, review the status to ensure the QKView file was successfully uploaded to iHealth:

* **Completed** - The QKView file was generated and automatically sent to the iHealth account.
* **Failed** - The QKView file either failed to generate, or was generated, but was not sent to the iHealth account. If this occurs you can :ref:`Download a QKView file` to manually upload to iHealth. You will not be able to download a QKView file that failed to generate. You can retry `generating a QKView file :ref:`Generate a QKView file for a BIG-IP Next HA instance`.  

Use one of the following procedures based on your BIG-IP Next instance configuration, and whether you are using the UI or API:

* :ref:`Generate a QKView file for a BIG-IP Next HA instance`
* :ref:`Generate a QKView file for a standalone BIG-IP Next instance - UI`
* :ref:`Generate a QKView file for a BIG-IP Next instance - API`


.. _`Generate a QKView file for a BIG-IP Next HA instance`:

.. _Generate a QKView file for a BIG-IP Next HA instance - UI:

**Generate a QKView file for a BIG-IP Next HA instance - UI**
-------------------------------------------------------------

#. Click the Workspace icon next to the F5 logo, and click **Instances**.
#. From the left menu click **My Instances**.
#. Select the row for a BIG-IP Next HA instance, click **Actions...** and then **Create QKView**.

   Alternatively, you can click the instance name, and then click **QKView Files** from the instance properties panel menu. 
#. Click **Create**.
#. Login to F5 iHealth using your iHealth credentials.
#. Click **Submit**.
#. Enter a **QKView File Name**.
#. (Optional) Enter your **F5 Support Case Number**.
#. At the bottom of the panel, select whether to generate a QKView for either the active, standby, or both nodes.
#. Click **Generate**.

The QKView file list displays the status of the node(s) selected. Generating a QKView file can take a few minutes. Review the statuses to ensure the file was uploaded to iHealth.


.. _Generate a QKView file for a standalone BIG-IP Next instance - UI:

**Generate a QKView file for a standalone BIG-IP Next instance - UI**
---------------------------------------------------------------------

#. Click the Workspace icon next to the F5 logo, and click **Instances**.
#. From the left menu click **My Instances**.
#. Select the row for a BIG-IP Next standalone instance, click **Actions...** and then **Create QKView**.

   Alternatively, you can click the instance name, and then click **QKView Files** from the instance properties panel menu. 
#. Click **Create**.
#. Login to F5 iHealth using your iHealth credentials.
#. Click **Submit**.
#. Enter a **QKView File Name**.
#. (Optional) Enter your **F5 Support Case Number**.
#. Click **Generate**.

Generating a QKView file can take a few minutes. Review the statuses to ensure the file was uploaded to iHealth.



.. _Generate a QKView file for a BIG-IP Next instance - API:

**Generate a QKView file for a BIG-IP Next instance - API**
-----------------------------------------------------------
Send a post request to generate a QKView file for a BIG-IP Next instance. The generated QKView file is
automatically uploaded to the iHealth account listed in the request. Ensure that you have the following information available:

* `instance_id` - To view a full list  of managed instances, send the following get request: 

  .. code-block::console

     ``GET https://{{bigip_next_cm_mgmt_ip}}/api/device/inventory``

* `filename` - Add a unique file name to identify the QKView file.
* `ihealth_user` and `ihealth_password` - Have your iHealth account credentials ready. To check if your credentials are valid use the OpenAPI for QKView: `F5 BIG-IP Next API Specification - QKView Feature <https://clouddocs.f5.com/products/big-iq/mgmt-api/v0.0.1/ApiReferences/bigip_public_api_ref/r_openapi-next.html#tag/QkviewFeature>`_.
* `f5_support_case` - (Optional) you can add an F5 support case number to iHealth and share with the listed case owner.

.. code-block::console

   ``POST https://{{bigip_next_cm_mgmt_ip}}/api/qkview/v1/qkviews``

Request body example with iHealth credentials and F5 Support case number:
..  what is visible in gui?

.. code-block:: json

 {
   "instance_id": "f4a0bdb2-8b31-4a8e-aee9-7f2ac2b629d2",
   "filename": "qkview_file",
   "source": "INSTANCE",
   "storage": "EXTERNAL",
   "ihealth_user": "user_admin",
   "ihealth_password": "user_password",
   "save_credential": true,
   "user_agent": "CM",
   "f5_support_case": "C351101",
   "share_with_case_owner": false,
   "visible_in_gui": false
 }


Successful response example:

.. code-block:: json

 {
   "_links": {
     "self": {
       "href": "/v1/qkviews/43b7bd5b-5b61-4a64-8fe4-68ef8ed910f2"
     }
   },
   "qkview_id": "43b7bd5b-5b61-4a64-8fe4-68ef8ed910f2",
   "task_id": "3325e40d-d9f2-4ab7-adb3-4ce1b6dbd48e"
 }


.. _Download a QKView file:

**Download a QKView file**
--------------------------
Once you generate a QKView file, you can download the QKView file to your local network. If 
the QKView file generation task fails, retry generating the QKView.

**Note**: QKView files are retained for a limited time on BIG-IP Next Central Manager. Depending on the outcome of the file generation, the following is the amount of time a QKView file is saved after it is generated:

 * The QKView task status is **Completed** - 1 day
 * The QKView task status is **Failed**- 7 days
 * QKView file task record information from the instance log - #. days

Once a QKView file is uploaded to iHealth, you can download directly from your iHealth account.

Use one of the following procedures based on whether you are using the UI or API:

* :ref:`Download a QKView file - UI`
* :ref:`Download a QKView file and upload to iHealth - API`

.. _Download a QKView file - UI:

**Download a QKView file - UI**
-------------------------------

#. Click the Workspace icon next to the F5 logo, and click **Instances**.
#. From the left menu click **My Instances**.
#. Select the name of the instance to open the instance properties panel.
#. From the panel menu, click **QKView Files**.
#. Select the check box for the QKView file row.
#. Click **Download**.

The QKView file is downloaded to your local system's downloads. You can manually upload the file to iHealth or F5 support.  


.. _Download a QKView file and upload to iHealth - API:

**Download a QKView file and upload to iHealth - API**
------------------------------------------------------
Once you have successfully generated a QKView file, you can download it to your local system. Ensure you have the following information: 

* `qkview_id` - The ID number created when the file was generated. For a list of all QKView file information send the following get request:
   .. code-block::console
   
      ``GET https://{{bigip_next_cm_mgmt_ip}}/api/qkview/v1/qkviews``

.. code-block::console

   ``GET https://{{bigip_next_cm_mgmt_ip}}/api/qkview/v1/files/{id}/download``

To upload the downloaded QKView file to iHealth use the following post request:

   .. code-block::console
   
      ``POST https://{{bigip_next_cm_mgmt_ip}}/api/qkview/v1/files/{id}/upload``

Request body example:

.. code-block:: json

 {
   "username": "user",
   "user_agent": "Chrome",
   "description": "Description about the request",
   "f5_support_case": "case",
   "share_with_case_owner": false,
   "visible_in_gui": true
 }


.. _Delete a QKView file log entry:

**Delete a QKView file log entry**
----------------------------------
You can delete a QKView file from your instance QKView file log and delete the QKView file from BIG-IP Next Central Manager, if applicable.
When removing a QKView log entry for a BIG-IP Next HA instance, you must delete all log entries associated with the QKView generation task.

Deleting a log entry from BIG-IP Next Central Manager does not impact your upload to iHealth.

**Note**: QKView file log entries are automatically removed from BIG-IP Next Central Manager after #. days. Depending on the status, QKView files can be downloaded from BIG-IP Next Central Manager within 1 or 7 days after it is generated.

Use one of the following procedures based on whether you are using the UI or API:

* :ref:`Delete a QKView file log entry - UI`
* :ref:`Delete a QKView file log entry - API`


.. _Delete a QKView file log entry - UI:

**Delete a QKView file log entry - UI**
---------------------------------------
#. Click the Workspace icon next to the F5 logo, and click **Instances**.
#. From the left menu click **My Instances**.
#. Select the name of the instance to open the instance properties panel.
#. From the panel menu, click **QKView Files**.
#. Select the check box for the QKView file row. If you have generated multiple files for an HA instance, select all rows associated with the HA instance.

#. Click **Delete**.

The QKView file log entry is removed from the list.



.. _Delete a QKView file log entry - API:

**Delete a QKView file log entry - API**
----------------------------------------
Ensure you have the following information: 

* `qkview_id` - The ID number created when the file was generated. For a list of all QKView file information send the following get request:
   .. code-block::console
   
      ``GET https://{{bigip_next_cm_mgmt_ip}}/api/qkview/v1/qkviews``

Use the following delete request to remove the QKView file from the instance log:

   .. code-block::console
   
      ``DELETE https://{{bigip_next_cm_mgmt_ip}}/api/qkview/v1/files/{id}``
