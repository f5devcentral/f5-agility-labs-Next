Lab 2.4 - Global Resiliency
===========================

BIG-IP Next DNS can provide Global Resiliency, the ability to steer clients to available services using DNS.

In this exercise we will extend the existing "gr.example.com" to run across multiple instances simultaneously.

Lab 2.4.1 Open Firefox
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now we will verify our application is deployed with DNS

#. Within your UDF Deployment, go to the **Firefox** access method that is under the **Ubuntu Jump Host**

    This will open an embedded Firefox browser session that is running inside the lab environment.

    .. image:: access-method-firefox.png
        :scale: 50%

#. Inside the Firefox browser session go to https://gr.example.com

#. You should now see your application

    .. image:: gr-app-firefox.png
        :scale: 50%

Lab 2.4.2 - Open Web Shell
~~~~~~~~~~~~~~~~~~~~~~~~~~

We will use the "dig" utility to verify our DNS records

#. Within your UDF Deployment, go to the **WEB SHELL** access method that is under the **Ubuntu Jump Host**
#. Type the following command

    .. code-block:: shell

      dig @10.1.10.153 gr.example.com +short

    You should see `10.1.10.154` being returned

In this example we were connecting directly to the DNS listeners on BIG-IP Next to query the DNS records

Lab 2.4.3 - Edit Application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Navigate to Applications


    Navigate to Applications by clicking the workspace switcher next to the F5 icon

    .. image:: top-left.png
      :scale: 50%

    Then click on **Applications**

    .. image:: central-manager-menu.png
      :scale: 50%

#. Click on "gr_app"

    In the upper right click on "Edit"

    .. image:: gr-app-edit.png
        :scale: 25%

    Click on "Review & Deploy"

    .. image:: gr-app-review-and-deploy.png
        :scale: 25%

#. Add Instance

    Under "Add Instance/Locations" select "big-ip-next-01" and click "+ Add to list"

    .. image:: gr-app-edit-add-instance.png
        :scale: 50%

#. Enter a Virtual Address

    Enter the Virtual Address of "10.1.10.155"

#. Under "Members" for big-ip-next-01 click on the down arrow and select "+Pool Members"
#. Click on "+ Add Row"

    Use the following values

    =============================== ==========================
    Property                        Value
    ------------------------------- --------------------------
    Name                            node2
    ------------------------------- --------------------------
    IP Address                      10.1.20.102
    =============================== ==========================

    Click on "Save" to return to the "Deploy" screen

#. Click on "Deploy Changes"
#. When prompted, press "Yes, Deploy"

    .. image:: gr-edit-yes-deploy.png
        :scale: 25%

Lab 2.4.4 - Open Web Shell
~~~~~~~~~~~~~~~~~~~~~~~~~~

We will use the "dig" utility to verify our DNS records

#. Within your UDF Deployment, go to the **WEB SHELL** access method that is under the **Ubuntu Jump Host**
#. Type the following command

    .. code-block:: shell

      dig @10.1.10.153 gr.example.com +short

#. repeat the command (you can use the up arrow to auto-complete) several times

    You should see both `10.1.10.154` and `10.1.10.155` being returned

We can see that we are getting a round robin response of DNS records
