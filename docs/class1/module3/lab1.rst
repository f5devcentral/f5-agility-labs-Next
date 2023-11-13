Lab 3.1 - Deploy and operate Applications
=========================================

This section of the lab will utilize the "Applications" section of BIG-IP Next Central Manager.

When creating an application service, it happens in two steps.

Step 1:

Start by selecting a template, which will determine what type of features you will need (i.e. HTTPS, WAF, etc...). Some templates can be built to allow you to toggle features on/off (the template that ships with Central Manager includes the ability to toggle on/off WAF, iRules, etc...).

Once you have selected your template, you will need to define the name of your virtual server (destination of where you want your clients to connect) and information about the pools (backend servers).  This includes information such as the port numbers that should be used.  

Step 2: 

After you have defined the properties for your application, you will need to select the location of where you would like to deploy your applications.  You will then be prompted for the IP addresses for that specific location.

#. Navigate to Applications


    Navigate to Applications by clicking the workspace switcher next to the F5 icon

    .. image:: top-left.png
      :scale: 50%

    Then click on **Applications**

    .. image:: central-manager-menu.png
      :scale: 50%

#. Create Application
    
    From **My Application Services** click on **+ Add Application**

    .. image:: add-application.png
      :scale: 25%

    Enter an application name of "https-app" and select **From Template**

    .. image:: app-create-inone.png
      :scale: 75%

    Click on **Select Template**

#. Select **HTTPS-Load-Balancing-Service**

    .. note:: This is a customized template that was created specifically for this lab.  It is not included in a default installation of Central Manager

    .. image:: select-https-app.png
      :scale: 50%

    Then click on **Start Creating**

#. Application Service Properties

    .. image:: application-service-properties.png
      :scale: 25%

    You will see the default Application Service Properties for this template (these have been pre-populated by the template; in a later lab you will need to fill these in)

    Click on the edit icon next to "HTTPS" to view further details of the HTTPS configuration

#. Protocols and Profiles

    Here you can see the TLS options, the template has also pre-selected the "self_demo.f5.com" Certificate

    .. image:: protocols-and-profiles.png
      :scale: 50%

    In a later lab we will modify this to create a new template that uses TLS to connect to the backend server

    Click on **Cancel and Exit** to return to the previous "Application Service Properties" screen

#. Review and Deploy

    Click on **Review and Deploy**

    .. image:: review-and-deploy.png

#. You will now see the Deploy-to screen

    .. image:: deploy-to-main.png
      :scale: 25%

    Click on the **Start Adding** button in the middle of the screen.

#. Select Location

    You will need to select **big-ip-next-01.f5demo.com** and then click on **Add to List**

    .. warning:: You may need to adjust the zoom setting on your browser window to see the "Add to List" button

    .. image:: deploy-add-to-list.png
      :scale: 75%

#. Virtual Address

    You can now enter your Virtual Address.  Use the IP Address of "10.1.10.200"

    .. image:: deploy-to-virtual-address.png
    
    Then click on the down arrow next to "members" to open the Pool Members screen

#. Pool Members

    Click on the **+ Pool Members** to add pool members

    .. image:: deploy-to-pool-members-plus.png
      :scale: 75%

    On the Pool Members screen click on the **Add Row** that is in the lower right

    .. image:: deploy-to-pool-members-add-row.png
    
    Use the following values to add two rows

    =========================== ==========================
    Name                        IP Address
    --------------------------- --------------------------
    node1                       10.1.20.100
    --------------------------- --------------------------
    node2                       10.1.20.101
    =========================== ==========================

    .. image:: deploy-to-pool-members-nodes.png

    Then click on **Save**

#. Validate

    You can now validate your changes before deploying them.

    Click on **Validate All**

    .. image:: deploy-to-validate-all.png

    After it completes click on **View Results**

    .. image:: deploy-to-validate-all-view-results.png
      :scale: 75%

    You can inspect the AS3 declaration that will be deployed to your BIG-IP Next instance.

    .. image:: deploy-to-validation-results.png

    Click on **Exit** to leave the preview of the AS3 declaration

#. Deploy Changes

    You are now ready to deploy your application to the desired location.

    Click on **Deploy Changes**

    .. image:: deploy-to-deploy-changes.png

#. Confirm that you would like to deploy

    You will be prompted to confirm, click on **Yes, Deploy**

    .. image:: deploy-to-confirmation.png

#. Within your UDF Deployment, go to the **Firefox** access method that is under the **Ubuntu Jump Host**

    This will open an embedded Firefox browser session that is running inside the lab environment.

    .. image:: access-method-firefox.png

#. Inside the Firefox browser session go to https://10.1.10.200 

    .. image:: access-method-firefox-url.png

#. You will need to click past the cert errors by clicking on **Advanced** -> **Accept the risk and continue**

    .. image:: access-method-firefox-accept-the-risk.png

#. You should now see the demo app

    .. image:: https-app-deployed.png
    
