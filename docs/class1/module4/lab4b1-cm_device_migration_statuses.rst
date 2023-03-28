=======================================
Reference: Application migration status
=======================================
Tenant, application, and service (virtual server) statuses indicate whether an application's virtual servers are ready for deployment, or 
require modifications to unsupported objects. You can convert an application regardless of its status. If an 
application is selected for conversion the migration tool will remove or replace objects to ensure successful conversion and
subsequent deployment to BIG-IP Next instances. Object replacement is only applied to unsupported security objects. 

You can review modified objects during the **Application Migration** stage (pre-conversion). When selecting applications to convert,
you can open the editor (**Edit & Verify**), and click **Save & Verify**. This highlights the anticipated changes per virtual server. 

Statuses
========

-----------------------------
Ready for deployment: |valid|
-----------------------------
This status indicates that all objects are supported. No changes will be made to the objects in the application's virtual servers during the conversion process.


------------------------------
Unsupported objects: |invalid|
------------------------------
This status indicates required removal of virtual server objects that are not supported by BIG-IP Next. 

If an application with this status is selected for migration, unsupported objects are removed prior to conversion. This ensures successful application deployment to BIG-IP Next instances. 


-------------------------------------------------------
Unsupported security and default objects: |invalid-sec|
-------------------------------------------------------
This status indicates required changes to a virtual server’s secure objects of embedded certificates and keys, and other secure default objects. 

**Certificates and Keys**

- This version of BIG-IP Next supports cipher suites that use these algorithms: Rivest Shamir Adleman (RSA),  Elliptic Curve Digital Signature Algorithm (ECDSA), and the FIPS-HSM certificate/key pairs. 
- Any other certificates and keys (e.g. DSA) are automatically replaced by a default HTTPS certificate and key pair during migration to BIG-IP Next. 

**Default objects**

- If a default object (iRules, monitors, and profiles) is found, the converted application automatically references the source BIG-IP to ensure that object is fully in the AS3 configuration. Objects with entities that are not supported in BIG-IP Next are removed. 

You can manually change the virtual server objects in the embedded editor. Unless the manual editing process is 
specified, F5 does not recommend manual editing, as it can result in unsuccessful application conversion
or deployment to a BIG-IP Next instance. 



.. |valid| image:: ../images/migration-valid-deploy-icon.png

.. |invalid| image:: ../images/migration-invalid-object-icon.png

.. |invalid-sec| image:: ../images/migration-invalid-sec-object-icon.png
