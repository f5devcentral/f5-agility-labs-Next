Lab 3.1 - Deploy and operate Applications
=========================================

From "My Apps" click on "+Add Application"

.. image:: add-application.png
  :scale: 25%

Click on "Create"

.. image:: create-application.png
  :scale: 25%

Select "HTTPS-Load-Balancing-Service"

.. image:: select-https-app.png
  :scale: 25%

Use the following inputs to create the app on the first screen.

.. note:: If you were unable to add big-ip-next-01 in the previous lab you can use big-ip-next-03 instead

=========================== ==========================
Name                        Value
--------------------------- --------------------------
Location                    big-ip-next-01.f5demo.com
--------------------------- --------------------------
Application Name            https-app
--------------------------- --------------------------
Virtual Address             10.1.10.200
--------------------------- --------------------------
Virtual Port                443
=========================== ==========================

.. image:: create-application-tab1.png
  :scale: 25%

Click on "Next" to add endpoints (pool members) for the application.

Click on "Add Endpoints"

.. image:: click-add-endpints.png
  :scale: 25%

Add the following endpoint address (no need to specify name)

* 10.1.20.100

Click on "Ok"

Change "Service Port" from 443 to 8080

Click on "Next"

For the Certificate select "self_demo.f5.com"

Click on "Next"

Click on "Validate Deployment"

Click on "View Validation Results"

You will see a preview of the AS3 declaration that will be created by the template.

Click on "Exit" to leave the preview of the AS3 declaration

Click on "Deploy"

Open a new browser tab in the RDP client and go to https://10.1.10.200 (you will need to click past the cert errors)

You should now see the demo app

.. image:: https-app-deployed.png
  :scale: 25%
    
