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
  
   .. image:: ./pictures/cm_login.png 

 * Option 2: Go to the "Windows Jump Host" you've logged in before, start the Chrome browser and select the "Central Manager" Favorite using the same credentials as mentioned in Option 1
 
   .. image:: ./pictures/cm_login_jumphost.png

|

**2. From "My Apps" click on "+ Add Application"**

 .. image:: ./pictures/add-application.png

|

**3. Click on "Create"**

 .. image:: ./pictures/create-application.png

|

**4. Choose the "http" Template and click on "Start Creating"** (the http template is a unified template that allows you to enable/disable capabilities)

 .. image:: ./pictures/application_template.png

|

**5. Enter the following values in the template wizard for Properties as shown in the picture below and click "Next"**
   
 * Location: Choose e.g. big-ip-next-01.f5demo.com
 * Application Name: waf-app
 * Virtual Address: 10.1.10.203
 * Virtual Port: 443 

 .. image:: ./pictures/create_application_properties.png
|

**6. Select Endpoints (Pool Members)**

 On the Pool Properties page, leave the defaults of round-robin for Load-Balancing Mode and http for Monitor Type then click "Next".
 
 On the "Pool Members" page Click "Add New Endpoint" and input the pool member IP address 10.1.20.100 and click save.  You'll now see the IP address below the filter box, but now change the Service Port to 3000 and click "Next".

 .. image:: ./pictures/create_application_endpoints.png

|

**7. Select Security**

 1. Toggle on "Enable HTTPS (TLS Server)"
 2. Choose the certificate "self_demo.f5.com"

 .. image:: ./pictures/choose_cert.png

 3. Select "Next"
 4. On the "Security Policies" page Toggle on "Use a WAF Policy"
 5. Click "+ Create" and enter the new policy name "waf-policy" in the new window as shown in the picture below.
   

 .. image:: ./pictures/create_application_create_policy.png
    
 **Note:** Bot Defense, L7 DoS Protection, Threat Campaigns and IP Intelligence are enabled by default but some of them may require additional licenses when BIG-IP Next WAF reaches GA.


 1. Click "Save" and you will get back to the previous screen as shown below.

 2. Click on "Next" to skip past iRules
   
 **Note:**  Make sure all bullets on the left are marked in green. If not, go back to the section which is not marked in green and check if everything is filled correctly or re-select it in case it is a drop down menu.

 .. image:: ./pictures/policy_created.png

|

**8. Click on "Validate" to run the deployment validation**
 
 .. image:: ./pictures/validate.png

|

**9. After running the validation you'll see a screen like below showing"Success!"**

 .. image:: ./pictures/success.png
  
|

**10. Click on "View deployment validation results" to show the declaration**

 .. image:: ./pictures/declaration.png


 Click on "Exit" to go back to the previous screen.

 |

 .. image:: ./pictures/success.png
  
|

**11. Finally click on "Deploy" and you should see the application and the WAF policy deployed**

 .. image:: ./pictures/successful_deployed.png
  
|

**12. Now let's go to the Windows Jump Host and check it out**
    
 Open Chrome, go to https://10.1.10.203 and you should see the Juice Shop app.

 .. image:: ./pictures/final_check.png

 |

 Enter https://10.1.10.203/a=<script> and you should see the blocking page.

 .. image:: ./pictures/block_check.png

|

**13. You can see your block requested by visiting the WAF dashbaord**

From Central Manager click on the top left menu to select the Security menu.

 .. image:: ./pictures/security-menu.png
  :scale: 50%

From the WAF Dashboard click on the down arrow and select your "waf-policy".

 .. image:: ./pictures/waf-dashboard-select-policy.png

You can now view your "good" and "bad" requests 

.. note:: The "Lab Progress" app will also make "bad" requests in the background

**14. (Optional)  WAF Event Logs**

.. note:: This next exercise is optional (if you are doing this as part of internal F5 training and are part of the "Security" track, please skip in favor of your dedicated "Security" lab)

Copy the "Support ID" that is displayed when you triggered a WAF block.

 .. image:: ./pictures/get-support-id.png
  
Next click "Event Logs" and enter the support ID into the filter text box

 .. image:: ./pictures/waf-events-search-support-id.png

You can then click on the URI to view more details

 .. image:: ./pictures/waf-events-details.png

