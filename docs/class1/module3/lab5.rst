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

#. You should now be logged in
