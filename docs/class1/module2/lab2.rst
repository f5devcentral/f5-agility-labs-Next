Lab 2.2 - Deploy and Roll Over New iRule Version
================================================

In this lab exercise you will learn about how to deploy and roll over a new iRule version.

2.2.1 - Create iRule v2
~~~~~~~~~~~~~~~~~~~~~~~

First we will create a new version of our iRule

#. Navigate to **Applications**

    Navigate to **Applications** by clicking the workspace switcher next to the F5 icon

    .. image:: top-left.png
      :scale: 50%

    Then click on **Applications**

    .. image:: central-manager-menu.png
      :scale: 50%

#. Click on **iRules** followed by **insert_xff_iRule**

    .. image:: irules-menu.png
        :scale: 50%

#. You will need to next click on the **v1** at the top of the screen and select **Stage from this version**

    .. image:: irules-stage-from-this-version.png
        :scale: 50%

#. Using the built-in text editor, modify the iRule to add the following lines after the 4th line, and click **Commit Changes**

    .. code-block:: text

            } else {
                HTTP::header insert "X-Forwarded-For" [IP::client_addr]

    .. image:: irules-v2.png
        :scale: 50%

#. Click on **More Commit options**

    .. image:: irules-more-commit-options.png
        :scale: 50%

#. Click on **Commit without any attached applications.** then click on **Yes, Commit**

    .. image:: irules-commit-with-any-attached-applications.png
        :scale: 50%

Congratulations! You have created a new version of your iRule.  Note that this iRule has not been deployed yet. Continue the lab to learn about how to deploy your new version.

2.2.2 - Updating Application iRule version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We will next update the iRule version from the **My Application Services** menu

#. Click on **My Application Services** under **Applications** and click on the **irule-demo-app** application

    .. image:: irules-demo-app-new.png
        :scale: 50%

#. Click on edit icon in the top right of the screen to modify the Application Service Properties

   .. image:: irules_demo_app-edit-application-services-properties.png

#. Click on the edit icon next to **iRules**

    .. image:: irules-application-service-properties-edit-irule.png

#. Change the version from **v1** to **v2** and click **Save**

    .. image:: irules-v1-to-v2.png
    

#. Click on **Review & Deploy**

#. Click on **Deploy Changes**

#. You will be prompted to confirm your changes. Click on **Yes, Deploy**

You have now changed from **v1** to **v2**.  If you encounter problems in later portions of the lab please revert the iRule back to version **v1**

2.2.3 - Viewing the differences between versions of an iRule
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When troubleshooting an iRule, it can be useful to compare it to a previous version. This can also be done prior to deploying as a peer review step.

#. Select the **v2** at the top of the page and change it back to **v1** in order to see the first version of this iRule.

   .. image:: irules-select-v1.png
       :scale: 50%

#. Select **iRule** on the left side to open the preview window.

   .. image:: irules-preview-select.png
       :scale: 50%

#. Update the **Mode** dropdown to **Compare (Diff)**.

   .. image:: irules-compare-diff.png

In this view you are able to identify exact changes within the iRule from **v1** and **v2**.

