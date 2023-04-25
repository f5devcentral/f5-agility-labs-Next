=============================
Lab 5.1 - Modify templates to add security policy
=============================

* Show versioning of the template
* Deploy app again using the new template


Deploy an application with a WAF policy using FAST template
###########################################################
This part of the lab covers how to create and deploy an application and protect it with a WAF policy using the FAST template and the WAF violation rating based template with the focus on ease of use.

**Note:** The violation rating based template follows the same concept as you may know already from NGINX App Protect, which is low false positives with little policy maintenance and therefore the Policy Builder is not supported with the violation rating based template.

|

Backend web apps available on the internal network running on the **Ubuntu Jump Host**:

* OWASP Juice Shop: 10.1.20.100, 10.1.20.101, 10.1.20.102, 10.1.20.103 on port 3000.
* Simple F5 demo web app 10.1.20.100, 10.1.20.101, 10.1.20.102, 10.1.20.103 on port 8080.

|

Deploy a HTTPS load balancer with a WAF policy
**********************************************

**1. Login to BIG-IP Next Central Manager in UDF**
 
 * Option 1: Navigate to your UDF deployment and select the "GUI" ACCESS method for the "BIG-IP Next Central Manager" and login with the username/password provided under DETAILS.
  
   .. image:: ../module2/pictures/cm_login.png 

 * Option 2: Go to the "Windows Jump Host" you've logged in before, start the Chrome browser and select the "Central Manager" Favorite using the same credentials as mentioned in Option 1
   .. image:: ../module2/pictures/cm_login_jumphost.png

|

**2. From "My Apps" click on "+ Add Application"**

 .. image:: ../module2/pictures/add-application.png

|

**3. Click on "Create"**

 .. image:: ../module2/pictures/create-application.png

|

**4. Choose the "WAF-Secure-Load-Balancing-Service Application Template and click on "Start Creating"**

 .. image:: ../module2/pictures/application_template.png

|

**5. Enter the following values in the template wizard for Properties as shown in the picture below and click "Next"**
   
 * Location: Choose e.g. big-ip-next-03.f5demo.com
 * Application Name: juice_lab
 * Virtual Address: 10.1.10.140
 * Virtual Port: 443 

 .. image:: ./pictures/create_application_properties.png
|

**6. Select Endpoints (Pool Members)**

 Click on "+ Create" and add the pool member IP address 10.1.20.100 and the port number 3000 and click "Next".

 .. image:: ./pictures/create_application_endpoints.png

|

**7. Select Security**

 1. Choose the certificate "self_demo.f5.com"

 .. image:: ./pictures/choose_cert.png

 2. Click "+ Create" and enter the new policy name "juice_lab_policy" in the new window as shown in the picture below.
   
 |

 .. image:: ./pictures/create_application_create_policy.png
    
 **Note:** Bot Defense, L7 DoS Protection, Threat Campaigns and IP Intelligence are enabled by default but some of them may require additional licenses when BIG-IP Next WAF reaches GA.

 |

 3. Enable "Advanced View" to show that by default the Rating-Based-Template and UTF-8 is configured. We will create another policy with a different template in a following lab.
   
 .. image:: ./pictures/policy_advanced_view.png

 4. Click "Save" and you will get back to the previous screen as shown below.

 |
   
 **Note:**  Make sure all bullets on the left are marked in green. If not, go back to the section which is not marked in green and check if everything is filled correctly or re-select it in case it is a drop down menu.

 .. image:: ./pictures/policy_created.png

|

**8. Click on "Next" to see the Summary screen below**
 
 .. image:: ./pictures/validate.png

|

**9. Click on "Validate" and you should see the "Success!" message below**

 .. image:: ./pictures/success.png
  
|

**10. Click on "View deployment validation results" to show the declaration**

 .. image:: ./pictures/declaration.png


 Click on "Exit" to go back to the previous screen.

 |

 .. image:: ./pictures/success1.png
  
|

**11. Finally click on "Deploy" and you should see the application and the WAF policy deployed**

 .. image:: ./pictures/successful_deployed.png
  
|

**12. Now let's go to the Windows Jump Host and check it out**
    
 Open Chrome, go to https://10.1.10.140 and you should see the Juice Shop app.

 .. image:: ./pictures/final_check.png

 |

 Enter https://10.1.10.140/a=<script> and you should see the blocking page.

 .. image:: ./pictures/block_check.png

|

.. add go to the dashboard and show the violation



Next: |signup|

.. |signup| raw:: html

            <a href="https://github.com/f5devcentral/f5-big-ip-next-waf-lab/blob/main/labs/module2/lab1.rst" target="_blank">Module 2: Signatures and Threat Campaigns Update</a>