=============================
Lab 5.1 - Modify templates to add security policy (Basic WAF Lab)
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
 
 Navigate to your UDF deployment and select the "GUI" ACCESS method for the "BIG-IP Next Central Manager" and login with the username/password provided under DETAILS.
  
   .. image:: ./pictures/cm_login.png 


**2. From "My Apps" click on "+ Add Application"**

 .. image:: ./pictures/add-application.png

|

**3. For the "Application Service Name" fill in waf-app.  Select "From Template" and then click on "Select Template." Choose the "http" Template and click on "Start Creating"** (the http template is a unified template that allows you to enable/disable capabilities)

   * Application Service Name is "waf-app."  Click on "From Template" and then "Select Template"
  
 .. image:: ./pictures/create-application.png

|

**4. Select the "http" Application Template and when the waf-app APplication Service Properties comes up, click "Start Creating"

 .. image:: ./pictures/application_template.png

|

**5. Under the "waf-app," click on "Pools" and enter the following values in the template wizard as shown in the picture below**
  
     .. image:: ./pictures/waf-app-pool.png

   * Pool Name: waf-app-pool
   * Service Port: 3000
   * Leave other options the same


**6. Under the waf-app," click on "Virtual Servers" and enter the following values in the template wizard for Properties as shown in the picture below and then click "Next."**

   .. image:: ./pictures/waf-app-virtual-addition.png

   * Virtual Server Name: waf-app-vs
   * Pool: Select "waf-app-pool" from the drop-down
   * Port:  443

**7. Press the edit button under "Protocols and Profiles" (adjacent to "SNAT" and "MIRRORING")**

   * Press "Enable HTTPS (Client-Side TLS)" and choose the "self_demo.f5.com" certificate
 
 .. image:: ./pictures/choose_cert.png

**8. Select the edit button under "Security Policies" 

   1. Select "Use a WAF Policy"
   2. Click on "+ Create"
   3. Name:  waf-policy, leave all other items as default and click "Save" and then "Save" again.

**9. Pressing "Review and Deploy" will take you to the "Deploy" page.  Select "Start Adding" and select big-ip-next-01.f5demo.com as the instance to deploy to and then click "+ Add to List."**

   * The Deploy tab is the first place you'll actually define a virtual server.  The screens before were defining things like virtual server and pool names which will then be consistent as you deploy across infrastructure.  Imagine a global app that is deployed and a site is added.  The definition will already be in Central Manager and all you will need to define is a small subset of data (IP and pool members) and you will have a functional application that matches exactly the rest of your infrastructure.
 
 .. image:: ./pictures/instances-add-to-list.png

**10. Add the IP of 10.1.10.203 to the "Virtual Address" box, and then click the down arrow and select "+ Pool Members." **

 .. image:: ./pictures/IP_for_VIP.png

**11. Click on "+ Add Row" on the right and fill in "m_10.1.20.100" for the Name and "10.1.20.100" for the IP Address.  Click "Save"

 .. image:: ./pictures/pool_member_add.png
   
**12. Click on "Validate All" to run the deployment validation.  When the validation is complete, you will see a icon and status next to the deployment, such as the green icon and "Validated" in the picture below**
 
 .. image:: ./pictures/validate.png

**13. Click on "View deployment validation results" to show the declaration**

 .. image:: ./pictures/declaration.png


 Click on "Exit" to go back to the previous screen.

  
**14. Finally click on "Deploy" after which you will be prompted to confirm the deployment or cancel.  Click "Yes, Deploy" and you should see the application and the WAF policy deployed**

 .. image:: ./pictures/successful_deployed.png
  



**15. Let's validate the application through the UDF Firefox**
    
 On the lab components, select "Access" under the "Ubuntu Jump Host" and select "Firefox."  Within this proxied Firefox, go to https://10.1.10.203 and you should see the Juice Shop app.

 .. image:: ./pictures/final_check.png

 |

 Enter https://10.1.10.203/a=<script> and you should see the blocking page.

 URL:

 .. code-block:: console
  
    https://10.1.10.203/a=<script>

 .. image:: ./pictures/block_check.png

|

**16. You can see your block requested by visiting the WAF dashbaord**

From Central Manager click on the top left menu to select the Security menu.

 .. image:: ./pictures/security-menu.png
  :scale: 50%

From the WAF Dashboard under the Policies box, click on the three dots next to "waf-policy" and select "Filter by Policy Name".

 .. image:: ./pictures/waf-dashboard-select-policy.png

You can now view your "good" and "bad" requests 

.. note:: The "Lab Progress" app will also make "bad" requests in the background

**17. (Optional)  WAF Event Logs**

.. note:: This next exercise is optional (if you are doing this as part of internal F5 training and are part of the "Security" track, please skip in favor of your dedicated "Security" lab)

The Firefox copy and paste function doesn't often work, so remember the first few digits of the blocking "Support ID" when you triggered a WAF block.

 .. image:: ./pictures/get-support-id.png
  
Next click "Event Logs" and enter the "Support ID" into the filter text box.

 .. image:: ./pictures/waf-events-search-support-id.png

You can then click on the URI to view more details

 .. image:: ./pictures/waf-events-details.png

