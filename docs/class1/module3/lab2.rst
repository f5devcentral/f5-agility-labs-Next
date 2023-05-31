Lab 3.2 - Deploy new iRule version and "rollback"
=================================================

In this lab exercise you will learn about how to deploy a new iRule version

3.2.1 - Create iRule v2
~~~~~~~~~~~~~~~~~~~~~~~

First we will create a new version of our iRule

#. Navigate to Applications


    Navigate to Applications by clicking the workspace switcher next to the F5 icon

    .. image:: top-left.png
      :scale: 50%

    Then click on "Applications"

    .. image:: central-manager-menu.png
      :scale: 50%
         
#. Click on "iRules" followed by "udf_demo_xff"
    
    .. image:: irules-menu.png
        :scale: 50%
            
#. You will need to next click on the "v1" at the top of the screen and select "Stage from this version"
    
    .. image:: irules-stage-from-this-version.png
        :scale: 50%
            
#. Using the built-in text editor modify the iRule to add "2" after "Test Page" then click on "Commit Changes"
    
    .. image:: irules-v2.png
        :scale: 50%
            
#. Click on "more commit options"
    
    .. image:: irules-more-commit-options.png
        :scale: 50%
            
#. Click on "Commit without any attached applications." then click on "Yes, Commit"
    
    .. image:: irules-commit-with-any-attached-applications.png
        :scale: 50%

Congratulations you have created a new version of your iRule.  Note that this iRule has not been deployed yet, continue to learn about how to deploy your new version.

3.2.2 - Deploying a new iRule version from iRules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are two different ways that you can attach an iRule to an application.  In this next exercise we will attach the iRule to the application from the iRule itself.

#. Start on the "Properties" page of your iRule.  You should already be on this screen from the previous lab exercise.  If you can re-open the iRule as needed.
    
    .. image:: irules-attach-v2-to-application.png
        :scale: 50%

    make sure that the version is "v2" and that you can see "Attach Applications" (you may need to scroll the window to see the button) and click on "Attach Applications"

#. You should only see one available application "irule_demo_app".  Click on the checkbox next to the name and then click on "Attach"
    
    .. image:: irules-attach-v2-to-application-attach.png
        :scale: 50%
    
#. On the next screen you should see a warning and a button to "Deploy Changes" click on the "Deploy Changes" button
    
    .. image:: irules-v2-deploy-changes.png
        :scale: 50%
    
#. You will be asked to confirm the change, click on "Yes, Continue"

Congratulations you have deploy v2 of your iRule.  Notice that you had the option to select which application(s) you wanted to attach the iRule to.

In the next exercise we will learn how to "roll over" a bad version.

3.2.3 - Rolling Over Bad Changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this exercise we will "roll over" a bad version.  This is not a "roll back", but instead we will update the latest version with the last known "good" version.

#. From the previous exercise you should still be on the "Properties" page for your v2 iRule.  You can re-open the iRule as needed.
#. Click on the "v2" at the top and select "v1"
    
    .. image:: irules-select-v1.png
        :scale: 50%

#. You should now see the original v1 iRule without your changes.
#. Click on the "v1" at the top and select "Stage from this version"
    
    .. image:: irules-v1-stage-from-version-to-v3.png
        :scale: 50%
    
#. Click on "Commit Changes"
            
#. Click on "more commit options"
    
    .. image:: irules-more-commit-options.png
        :scale: 50%
            
#. Click on "Commit without any attached applications." then click on "Yes, Commit"
    
    .. image:: irules-commit-with-any-attached-applications.png
        :scale: 50%

#. You should now see "v3" (ignore any warnings about viewing an older version of the iRule)
    
    .. image:: irules-v3.png
    
3.2.4 - Updating Application iRule version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Previously we updated the application from the "iRules" menu.  We will next update the iRule version from the "My Apps" menu

#. Click on "My Apps" under "Applications" and click on the "irule_demo_app" application
    
    .. image:: irules-demo-app.png
        :scale: 50%
    
#. Click on "iRules" under App Settings and then select "v3" under Version
   
   .. image:: irules-demo-app-select-version.png
        :scale: 50%

#. Click on "Next" to view the Summary and then click on "Deploy"

You have now "rolled over" the bad "v2" to a good "v3".  Yes, it's possible to have selected "v1" again, but by "rolling over" we ensure that the latest version is the correct version.