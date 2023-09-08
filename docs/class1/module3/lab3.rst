Lab 3.3 - Customizing a Template
================================

In this exercise we will modify our previous HTTPS-Load-Balancing-Service template to perform TLS re-encryption to the backend server.

* Change virtual server port from 443 to 8443 (to avoid port conflicts in the lab environment)
* Change the pool member monitor from http to https
* Change the pool member port from 8080 to 8443
* Enable TLS encryption between the BIG-IP Next instance and the backend pool members

3.3.1 - Clone Template
~~~~~~~~~~~~~~~~~~~~~~

When a template has been used to deploy an application it will enter a "Published" state.  This makes the template a read-only object.  To make edits to the template you can either remove any applications that are using the template to revert it into "Draft" mode, or clone the template.  In this lab exercise we will clone the template.

.. note:: The "http" app is shipped by Central Manager.  We have loaded a custom HTTPS-Load-Balancing-Service application into this lab environment.  It is a modified version of the provided "http" template.

#. Under the Applications menu clik on "App Templates"
#. Select the checkbox next to the "HTTPS-Load-Balancing-Service" template
#. Click on the "Clone" button
    
    .. image:: fast-clone-template.png
        :scale: 50%

#. Click on "Save"

Continue to the next lab section on how to modify the cloned template.

3.3.2 - Modify Cloned Template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now that you have cloned the template you will make the following three changes

* Change virtual server port from 443 to 8443 (to avoid port conflicts in the lab environment)
* Change the pool member monitor from http to https
* Enable TLS re-encryption to the backend pool member

#. click on "clone_HTTPS-Load-Balancing-Service" and then click on the "Template" tab

#. Search for "virtualPort" (CTRL-F in your browser or scroll ~50% of the page) 
    
    .. image:: fast-template-template-body.png
        :scale: 80%
    
#. Change the value of the default from "443" to "8443" (without quotes)
#. Next search for "pools:" and change the default to use an https monitor and connect on port 8443, the completed change should look like
    
    .. code-block:: yaml

        pools: # Do not remove and do not change the property name. This is used to take pools information
            type: array
            default:
            -  {"loadBalancingMode": "round-robin","monitorType": ["https"],"poolName": "my_pool","servicePort": 8443} 
            
#. Next search for "enable_TLS_Server:" and change the default to true, the completed change should look like
    
    .. code-block:: yaml

        enable_TLS_Server:
          title: Enable Server-side TLS
          description: Enable TLS to encrypt server-side connections.
          type: boolean
          default: true      
     
#. Click on "Save"

3.3.3 - Verify Your Cloned Template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Next we will verify that your changes are present in your cloned template.

#. Under Applications go back to "My Application Services" and click on "+Add Application"
#. Enter the Application Service name of "https-re-encrypt"
#. Select "From Template"
#. Select "clone_HTTPS-Load-Balancing-Service" template (template you previously cloned)
#. Click on "Start Creating"
#. Verify that the Virtual Port is now 8443 and that you see TLS under Protocols & Profiles
    
    .. image:: cloned-virtuals.png
        :scale: 75%
    
#. Next click on the "Pools" tab and verify that the monitor is now https and the service port is 8443
    
    .. image:: cloned-pools.png
        :scale: 75%
    
#. Click on Cancel & Exit
#. Select your "https-re-encrypt" Application and select "Delete" under "Actions"

.. note:: If you run into any issues modifying yoru template you can delete the "https-re-encrypt" application to make the template editable again