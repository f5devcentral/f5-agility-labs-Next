Lab 3.3 - AS3 Editor
================================

In this exercise we will modify an application created using the AS3 editor from an HTTP to an HTTPS application.

In the previous exercise we used the "Standard" template to deploy our applications.  This template is useful when we want to deploy an application that uses common input parameter, but it can be limiting if we need to modify a setting that is not exposed (like persistence).

Using the "AS3 Editor" we can deploy an application that can take advantage of the full scope of settings that are available in AS3.

3.3.1 - View AS3 Application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First we will view an application created using the AS3 Editor.

#. Within your UDF Deployment, go to the **Firefox** access method that is under the **Ubuntu Jump Host**

    This will open an embedded Firefox browser session that is running inside the lab environment.

    .. image:: access-method-firefox.png
        :scale: 50%

#. Inside the Firefox browser session go to http://as3-demo-app.example.com (make sure you go to "http" and not "https")

#. You should see the HTTP version of the as3-demo-app application

    .. image:: as3-demo-app-firefox.png
        :scale: 50%

3.3.2 - Modify AS3 Application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Navigate to Applications


    Navigate to Applications by clicking the workspace switcher next to the F5 icon

    .. image:: top-left.png
      :scale: 50%

    Then click on **Applications**

    .. image:: central-manager-menu.png
      :scale: 50%

#. Click on "as3-demo-app"

    In the upper right click on "Edit"

    .. image:: as3-demo-app-edit.png
        :scale: 25%

#. Replace with 

    This is an example of an HTTPS application that is referencing certificates that are managed by Central Manager.

    .. code-blocK:: json

        {
        "class": "ADC",
        "id": "id-https-canonical",
        "mytenant": {
            "as3-demo-app": {
            "class": "Application",
            "my_pool": {
                "class": "Pool",
                "loadBalancingMode": "least-connections-member",
                "members": [
                {
                    "serverAddresses": [
                    "10.1.20.100",
                    "10.1.20.101"
                    ],
                    "servicePort": 8443
                }
                ],
                "monitors": [
                "http"
                ]
            },
            "my_service": {
                "class": "Service_HTTPS",
                "pool": "my_pool",
                "clientTLS": "client_tls",
                "serverTLS": "server_tls",
                "snat": "auto",
                "virtualAddresses": [
                "10.1.10.113"
                ],
                "virtualPort": 443
            },
            "client_tls": {
                "ciphers": "RSA",
                "class": "TLS_Client",
                "tls1_1Enabled": true,
                "tls1_2Enabled": true,
                "tls1_3Enabled": false
            },
            "server_tls": {
                "certificates": [
                {
                    "certificate": "webcert"
                }
                ],
                "ciphers": "RSA",
                "class": "TLS_Server",
                "tls1_1Enabled": true,
                "tls1_2Enabled": true,
                "tls1_3Enabled": false
            },
            "webcert": {
                "certificate": {
                "cm": "wildcard.example.com.crt"
                },
                "class": "Certificate",
                "privateKey": {
                "cm": "wildcard.example.com.pem"
                }
            }
            },
            "class": "Tenant"
        },
        "schemaVersion": "3.0.0"
        }
                

#. Click on "Review & Deploy"

#. Click on "Deploy"

3.3.3 - View modified AS3 Application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. Inside the Firefox browser session go to https://as3-demo-app.example.com (make sure you go to "https" and not "http")

#. You should see the HTTPS version of the as3-demo-app application

    .. image:: as3-demo-app-https-firefox.png
        :scale: 50%
