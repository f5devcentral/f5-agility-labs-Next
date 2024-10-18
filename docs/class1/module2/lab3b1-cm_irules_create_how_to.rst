==============================================================
How to: Create or modify iRules on BIG-IP Next Central Manager
==============================================================

The following describes how to create a new, edit, or delete iRules using BIG-IP Next Central Manager.

iRules are created using the programming language Tcl. To create or modify an iRule you need to have familiarity with
Tcl and iRule commands.

For more information about iRule structure, uses, and traffic management customizations see
F5 iRule documentation `iRules Home <https://clouddocs.f5.com/api/irules/>`_.


* :ref:`Create a new iRule`
* :ref:`Clone an iRule`
* :ref:`Edit an iRule`

  Make general changes to an existing iRule without staging changes.
* :ref:`Stage an iRule`

  Create version changes to an iRule and deploy to a limited testing environment. You can later attach verisons to a production environment.
* :ref:`Attach iRule version to applications`

  Attach applications directly to an iRule, and select the iRule version to deploy to one or all attached applications.
* :ref:`Compare iRules`

  Compare versions of the same iRule to one-another in the editor.
* :ref:`Delete an iRule`

.. ## Prerequisites

.. _Create a new iRule:

------------------
Create a new iRule
------------------
Create an iRule using the UI or API. Once you create an iRule, you can add it
to one of your applications, or create a new application using a iRule application template.

See `Example iRules <cm_irules_example_irules_reference.md>`_ provided in BIG-IP Next Central Manager. These are commonly applied iRules
that you can use as examples copy or :ref:`clone`.


* :ref:`Create a new iRule - UI`
* :ref:`Create a new iRule - API`


.. _Create a new iRule - UI:

-----------------------
Create a new iRule - UI
-----------------------

#. Click the Workspace icon next to the F5 icon, and click **Applications**.
#. Click **iRules** from the left menu.
#. At the top of the screen, click **Create**.
#. Type an **Name** for the iRule, and add an optional **Description**.
#. Click on line 1 of the editor in the panel to start creating the iRule. The following is an example procedure for a simple iRule:

   *Note*: The UI includes inline syntax validation. The editor provides a list of allowed Tcl/iRule syntax as you add to the schema.
   With any list-selected input, you can hover over the schema element to view details and use case examples for that selection.
   See a `sample iRule procedure <#sample-irule-in-editor>`_ in the GIF below.

   #. Begin typing the iRule function <!--declaration??--> and select from the provided list.
   #. Press the SPACE bar to view a list of all valid `events <https://clouddocs.f5.com/api/irules/Events.html>`_. You can type key words to filter the event list.
   #. Enter the conditions and `commands <https://clouddocs.f5.com/api/irules/Commands.html>`_.

#. To copy the iRule click the copy button in the top right of the editor.

   .. image:: ../images/cm_copy_icon_button.png

#. Click **Save**.

The new iRule is added to the iRules list. To deploy iRules to a secure FAST application, see `How to: Deploy iRules to an application on BIG-IP Next Central Manager <cm_irules_deploy_how_to.rst>`_.


.. _Sample iRule in editor:

----------------------
Sample iRule in editor
----------------------

.. image:: ../images/cm_irule_create_gif.gif

.. _Create a new iRule - API:

------------------------
Create a new iRule - API
------------------------
Use the following procedure to create a new iRule.

Send a POST request using the iRule OpenAPI.

``POST https://{{bigip_next_cm_mgmt_ip}}/api/irule/v1/irule``

The body of the POST must include:
   * ``name``- iRule name
   * ``content`` - The iRule schema in Tcl
   * ``description`` - (optional)Additional information about the iRule.

.. Need more info for 'content' bullet

.. Example request body for creating an iRule Needs sample

.. _clone:
.. _Clone an iRule:

--------------
Clone an iRule
--------------
Clone an iRule to work with an existing iRules contents, without creating a new iRule from scratch using the BIG-IP Next Central Manager UI.
(See :ref:`Create a new iRule - API` to create an iRule using the API.)


.. _Clone an iRule - UI:

-------------------
Clone an iRule - UI
-------------------

#. Click the Workspace icon next to the F5 icon, and click **Applications**.
#. Click **iRules** from the left menu.
#. Click the iRule name.

   A panel for the iRules properties and content editor is displayed.
#. To the top right of the panel, click **Clone**.

   The Clone iRule panel opens. **Name** is automatically populated with original iRule's name and the prefix `clone`.
#. (Optional) Update the **Name** and add a **Description**.
#. From the panel menu click **iRule Editor**.

   The editor contains the iRule script from the original iRule.
#. When you have completed changes to the iRule, click **Save**.

The cloned iRule is added to the iRules list. To deploy iRules to a secure FAST application, see `How to: Deploy iRules to an application on BIG-IP Next Central Manager <cm_irules_deploy_how_to.rst>`_.


Use the following procedure to create a new iRule.

Send a POST request using the iRule OpenAPI.
``POST https://{{bigip_next_cm_mgmt_ip}}/api/irule/v1/irule``

The body of the POST must include:
   * ``name``- iRule name
   * ``content`` - The iRule schema in Tcl
   * ``description`` - (optional)Additional information about the iRule.


.. TODO: Example request body for creating an iRule


.. _Edit an iRule:

-------------
Edit an iRule
-------------

You can edit the contents of an iRule configured on you BIG-IP Next Central Manager. Use the following procedures to
make changes.

* :ref:`Edit an iRule - UI`
* :ref:`Edit an iRule - API`


.. _Edit an iRule - UI:

------------------
Edit an iRule - UI
------------------
#. Click the Workspace icon next to the F5 icon, and click **Applications**.
#. Click **iRules** from the left menu.
#. Click the iRule name.

   A panel for the iRules properties and content editor is displayed.
#. Select the version to the right of the iRule name . See image below.

   .. image:: ../images/cm_irule_version.gif
#. From the **Properties** area you can edit the **Description** and view the attached applications.
#. From the panel menu click **iRule Editor**.

   The iRule content schema is displayed in the panel
#. Edit the iRule schema. For more information about editing iRules, see `iRules Home <https://clouddocs.f5.com/api/irules/>`_.

   Note: If you are editing a staged iRule, the editor automatically saves your changes.
#. Click **Save**.


.. TODO: Need to check if the need to re-deploy the app



.. _Edit an iRule - API:

-------------------
Edit an iRule - API
-------------------
Use the following procedure to edit an existing iRule.

You will need the name (``nameOrID``) of the iRule. Use the following GET request to see a full list of iRules on BIG-IP
Next Central Manager and their details:

``GET https://{{bigip_next_cm_mgmt_ip}}/api/irule/v1/irule``

Send a PUT request with changes to the iRule ``content`` using the iRule OpenAPI.

``PUT https://{{bigip_next_cm_mgmt_ip}}/api/irule/v1/irules/{nameOrID}``

.. TODO: Example request body for editing an iRule Needs sample


.. _Stage an iRule:

--------------
Stage an iRule
--------------
One user can create one staged iRule

.. _Attach iRule version to applications:

Attach iRule version to applications
------------------------------------

.. _Compare iRules:

Compare iRule versions
----------------------
You can compare iRule versions to view differences and decide whether to attach staged iRules to a production environment.

iRules in staging can only be edited by the login owner.

#. Click the Workspace icon next to the F5 icon, and click **Applications**.
#. Click **iRules** from the left menu.
#. Click the iRule name.

   A panel for the iRules properties and content editor is displayed.
#. Select the version to the right of the iRule name.

   Note: You can click **Show Version History** to view a full version list. Then select 2 versions and click **Compare**. This option limits is a view-only editor.
#. From the panel menu click **iRule Editor**.
#. From **Mode** select **Compare(Diff)**.
#. From **Compare to iRule** select an iRule, and select a **Version**.

   Note: If you do not select the most recent version a banner will appear on the panel. From the banner you can select the latest version, or the staged version, if available.

The editor highlights differences between the two selected iRules. Staged iRules can only be edited by the user who created the staged version. Ensure you are using the proper credentials if you wish to edit the iRule.


.. _Delete an iRule:

---------------
Delete an iRule
---------------
You can remove an iRule. Ensure that you have removed the iRule from any attached applications.

* `Delete an iRule - UI :ref:`Delete an iRule - UI`
* `Delete an iRule - API :ref:`Delete an iRule - API`


.. _Delete an iRule - UI:

--------------------
Delete an iRule - UI
--------------------
#. Click the Workspace icon next to the F5 icon, and click **iRules**.
#. Click check box next to the iRule name.
#. Click **Delete**.

The iRule is deleted from the iRules list.


.. _Delete an iRule - API:

---------------------
Delete an iRule - API
---------------------
Use the following procedure to delete an iRule.

You will need the name (``nameOrID``) of the iRule. Use the following GET request to see a full list of iRules on BIG-IP
Next Central Manager and their details:

``GET https://{{bigip_next_cm_mgmt_ip}}/api/irule/v1/irule``

Send a DELETE request with changes to the iRule ``content`` using the iRule OpenAPI.

``DELETE https://{{bigip_next_cm_mgmt_ip}}/api/irule/v1/irules/{nameOrID}``
