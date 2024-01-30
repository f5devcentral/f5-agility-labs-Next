Lab 3.5 - Identity Aware Proxy
===============================

BIG-IP Next Access can provide an Identity Aware Proxy, a proxy that requires authentication before a resource can be accessed.

In this exercise we will explore the existing "signed.example.com" that is using Okta and Azure AD for restricting access to an application.

Lab 3.5.1 Open Firefox
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Now we will verify our application is deployed with DNS

#. Within your UDF Deployment, go to the **Firefox** access method that is under the **Ubuntu Jump Host**

    This will open an embedded Firefox browser session that is running inside the lab environment.

    .. image:: access-method-firefox.png
        :scale: 50%

#. Inside the Firefox browser session go to https://signed.example.com 

#. Login user username: user1@f5access.onmicrosoft.com and password user1
    
    .. image:: okta-login.png
        :scale: 50%

#. You should now be logged in
    
    .. image:: signed-example-logged-in.png
        :scale: 50%

#. In Central Manager Navigate to **Security**

    Navigate to **Security** by clicking the workspace switcher next to the F5 icon

    .. image:: top-left.png
      :scale: 50%

    Then click on **Security**

    .. image:: central-manager-menu-security.png
      :scale: 50%
         
#. Click on "Access Dashboard"
    
    You will see your session

    .. image:: access-dashboard.png
        :scale: 50%

    .. note:: Due to single sign on with Okta if you remove your session you will still be logged in.  An example that uses a different authentication type would cause your session to end after you remove the session.