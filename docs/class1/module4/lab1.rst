=============================
Lab 4.1 - Import App from UCS
=============================

First, access the Windows Jump Host via RDP.

.. image:: ./lab4-desktop.png

Launch Google Chrome to access BIG-IP Next CM 

.. image:: ./lab4-NextCM.png


After logging into BIG-IP Next CM, click on  "+ Add Application" on the upper right

.. image:: ./lab4-add_app_NextCM.png
    

Select "New Migration" and enter the following information for the Migration Details:

    Session Name - config-import
    Description - Agility2023 BIG-IP Next Lab

.. image:: ./lab4-new-app-migration.png

Source BIG-IP System - Select the UCS from the UCS folder stored on the lab4-desktop

Leave the Master Key and Encrypred UCS Archives options disabled.
Click Save & Continue

.. image:: ./lab4-ucs-import.png

Now you will have to select "Add Application" to import the 2 applications that were converted from the UCS
Select both and click on "Add" and then "Save & Continue"

.. image:: ./lab4-select2-apps.png

For Deployment Method, select the API option and for "Deploy Location" select big-ip-next-03.f5demo.com (10.1.1.10)

.. image:: ./lab4-select-Next03.png

Then select "Deploy" and after about 30 seconds, both applications should showcase a green successful icon.
Click Finish & Exit and now at the My Apps screen you will be able to see the 2 new apps that you have imported.

.. image:: ./lab4-import-finish.png
