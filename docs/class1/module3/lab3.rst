Lab 3.3 - Customizing a Template
================================

In the previous 3.1 lab exercise we needed to change the "Service Port" to 8080 from 443.

In this lab exercise we will clone the template from the previous lab exercise and make the following changes

* Change default "Service Port" from 443 to 8080
* Change the UI title from "Service Port" to "Pool Member Port"


3.3.1 - Clone Template
~~~~~~~~~~~~~~~~~~~~~~

#. Under the Applications menu clik on "App Templates"
#. Select the checkbox next to the "HTTPS-Load-Balancing-Service" template (make sure it is "HTTPS")
#. Click on the "Clone" button
    
    .. image:: fast-clone-template.png
        :scale: 50%

Continue to the next lab section on how to modify the cloned template.

3.3.2 - Modify Cloned Template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that you have cloned the template you will make the following two changes

* Change default "Service Port" from 443 to 8080
* Change the UI title from "Service Port" to "Pool Member Port"

#. Find the "Template Body" in the cloned template and look for "Service Port" (should be close to top third of the text field)
    
    .. image:: fast-template-template-body.png
        :scale: 50%
    
#. Change the value of the default from "443" to "8080" (without quotes)
#. Change the value of the title from "Service Port" to "Pool Member Port"
#. Verify that the output looks like the following
    
    .. code-block:: yaml
        
          servicePort:
            title: Pool Member Port
            description: Port for the pool members
            type: number
            default: 8080
            minimum: 0
            maximum: 65535

#. Click on "Save"

3.3.3 - Verify Your Cloned Template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Next we will verify that your changes are present in your cloned template.

#. Under Applications go back to "My Apps" and click on "+Add Application"
#. Click on "Create"
#. Select "clone_HTTPS-Load-Balancing-Service" template (template you previously cloned)
#. Click on "Start Creating"
#. Click on the "Endpoints (Pool Members)" under App Settings
    
    .. image:: fast-verify-changes.png
        :scale: 50%
    
    You should see both of your changes applied to the UI
#. Click on "Cancel & Exit" and proceed to the next lab exercise