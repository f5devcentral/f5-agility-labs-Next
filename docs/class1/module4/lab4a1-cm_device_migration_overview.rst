==========================================================================
Migrate BIG-IP application configurations onto BIG-IP Next Central Manager
==========================================================================
The following tool assists in a frictionless application migration experience of Local Traffic Manager (LTM) services into BIG-IP Next from existing BIG-IP devices. The process provides an end-to-end automation of application migration with minimal disruption to ongoing operations. In addition, no upgrades are required to support this process.
Application migration into BIG-IP Next provides feature compatibility checks and device health checks to ensure the application is properly migrated and deployed to your BIG-IP Next instances. In addition, as some features are not yet supported on BIG-IP Next, the migration tool ensures that only supported features are migrated. 

About application migration
===========================
The application migration tool allows you to select applications currently running on your BIG-IP devices to convert into 
Application Services (AS#. and deploy them to BIG-IP Next instances using AS3 API or F5 Application Service Templates (FAST).
Once your applications are migrated, you can deploy these applications to a BIG-IP Next instance for application service management.
This version of BIG-IP Next Central Manager supports migration from a UCS for application delivery and control services for HTTP and HTTPS applications. 

- Note: This version of BIG-IP Next supports HTTPS migration for AS3 deployment. For a FAST template deployments, you can only migrate an HTTP application (without certificates), and later select a 
- FAST HTTPS template that is configured on BIG-IP Next Central Manager.  



Overview of Application Migration at a glance
=============================================
* Supports per-application service migration and their dependencies from BIG-IP to BIG-IP Next (per-app only).
* Supports migration using BIG-IP Next Central Manager's user interface or using the OpenAPI. 
* Supports migration of applications with LTM services on BIG-IP version 12.1 (or later) to AS3 or FAST templates onto BIG-IP Next.
* Supports visibility of iRules found on migrated applications.
* Supports platform destination of Virtual Edition (VE) and VELOS managed in BIG-IP Next.
* Supports migration of HTTP (AS3 and FAST deployments) and HTTPS (AS3 deployments) applications.
* Supports migration of the following `default objects <#default-as3-object-conversion>`_ within the declaration: monitors, profiles, and iRules. 


Application deployment methods
==============================
When migrating an application from BIG-IP, application services are created out of each virtual server without applying partition 
grouping from the original BIG-IP device. This means that applications do not have dependencies on another application service (virtual server) that is not yet deployed. 
Shared objects from the source BIG-IP, such as iRules and endpoints (pool members), are copied into each application service as a separate entity. 


.. _Application Templates (FAST):

----------------------------
Application Templates (FAST)
----------------------------
Deploy each migrated HTTP application service on a selected FAST template. When you select this deployment method,
you can assign to each migrated application service a FAST template available on BIG-IP Next Central Manager. This includes both custom and pre-defined templates configured on BIG-IP Next Central Manager. See `How to: Manage FAST templates for a BIG-IP Next instance on BIG-IP Next Central Manager <cm_manage_fast_templates.html>`_ for more information about custom templates.

In addition, you can enter different deployment locations (BIG-IP Next instances) per application service. 

- Note: This deployment method supports HTTP applications without iRules. .. remove for honeydew


.. _AS3:

---
AS3
---
This deployment method allows you to select one deployment location (BIG-IP Next instance) for all application services selected during the migration session. This deployment method supports migration of:

* HTTP applications
* HTTPS applications
* iRules
* Monitors
* Profiles


.. _Secure application migration:

----------------------------
Secure application migration
----------------------------
THis version of BIP-Next supports deployment of HTTPS applications using AS#. Due to the secure nature of HTTPS applications, you will only be able to view the certificates and keys within the declaration after conversion. You can download the AS3 JSON declaration containing the embedded certificate and key pair information before deployment, or once you have deployed the applications onto BIG-IP Next Central Manager.


.. _Automatic object conversion:

---------------------------
Automatic object conversion
---------------------------
The migration tool might discover objects in your applications' virtual servers that are not supported by BIG-IP Next. 
BIG-IP Next Central Manager provides a conversion status when you select applications for migration. During the application
selection process you can review and verify the objects that will be automatically removed or converted to ensure successful
conversion and deployment to a BIG-IP Next instance.

For more information about deployment statuses, see `Reference: Application migration status <cm_device_migration_statuses.html>`_.

For more information about selecting applications for migration, see `How to: Migrate BIG-IP application configurations onto BIG-IP Next Central Manager <cm_device_migration_how_to.html>`_. 

- Note: If using the AS3 tenant migration,virtual servers are renamed to include the partition on which they were configured on a BIG-IP device. This is to prevent naming duplication when migrating from multiple BIG-IP devices. For more information see `Auto-renaming of virtual servers <cm_device_migration_how_to.html>`_.


.. _Default AS3 object conversion:

-----------------------------
Default AS3 object conversion 
-----------------------------
The migration tool recognizes configured default objects in the UCS and automatically migrates these objects into BIG-IP Next. Entries within 
objects that are not supported on BIG-IP Next are removed during conversion to AS3.

Supported default objects include, iRules, monitors and profiles. If the AS3 application included default objects before the migration, these objects
are automatically referenced in the application conversion process to ensure they are supported and deployed to a BIG-IP Next instance.

..  Grapefruit: customer should review in CM and be able to select something different so they do not need to reference cBIP.


