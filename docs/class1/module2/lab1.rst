Lab 2.1 - Deploy and operate Applications
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

    Enter an application name of "https-app" and leave the default of "Standard" selected

    .. image:: app-create-inone.png
      :scale: 75%

    Click on **Start Creating**

#. Then select "Start Creating" under the "No Virtual Servers Configured" to start creating a new virtual server.

 .. image:: ./https-app-add-vs.png

#. From within the "https-app" application service, click the **Pools** tab, then click **+ Create**, and enter the following values in the template wizard as shown in the picture below:

	Pool Name:

	.. code-block:: console

		https-app-pool

	Service Port:

	.. code-block:: console

		8080

   * Leave other options as is

     .. image:: ./https-app-pool.png

#. Navigate back to the **Virtual Servers** tab and enter the following values in the template wizard for Properties as shown in the picture below, then select **Next**

	Virtual Server Name:

	.. code-block:: console

		https-app-vs

	Virtual Port:

	.. code-block:: console

		443

	Pool:

	.. code-block:: console

		https-app-pool

   .. image:: ./https-app-virtual-addition.png

#. Select the edit button under the "Protocols and Profiles" column (adjacent to "SNAT" and "MIRRORING").

  .. image:: ./edit-protocols.png

  This is a new dialog for adding a TLS certificate to a virtual server.   Click on **Enable HTTPS (Client-Side TLS)**.  Below click on "Add" under the "No Client-Side TLS" text.

  .. image:: ./https-app-clientssl-add.png

#.  For the name of the Client-Side TLS, name is "www" and under the RSA Certificate, choose the "www.example.com" certificate.  Leave "Use Default Server" under TLS Servers and then push "Continue"

 .. image:: ./choose-cert.png

#. Click on "Save"

#. Review and Deploy

    Click on **Review and Deploy**

    .. image:: review-and-deploy.png

#. You will now see the Deploy-to screen

    .. image:: deploy-to-main.png
      :scale: 25%

    Click on the **Start Adding** button in the middle of the screen.

#. Select Location

    You will need to select **big-ip-next-03.example.com** and then click on **Add to List**

    .. warning:: You may need to adjust the zoom setting on your browser window to see the "Add to List" button

    .. image:: deploy-add-to-list.png
      :scale: 75%

#. Virtual Address

    You can now enter your Virtual Address.  Use the IP Address "10.1.10.200"

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

#. Inside the Firefox browser session go to https://www.example.com

    .. image:: access-method-firefox-url.png

#. You should now see the demo app

    .. image:: https-app-deployed.png
