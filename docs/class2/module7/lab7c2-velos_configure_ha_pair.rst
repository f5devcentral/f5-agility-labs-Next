==================================================================
How to: Configure an active/standby HA pair - BIG-IP Next on VELOS
==================================================================

Overview
========

Active/standby (A/S) high availability (HA) is the deployment of redundant pairs of BIG-IPs consisting of an active and standby. If the active fails or is administratively downed, the standby takes over ensuring availability of BIG-IP services with minimal disruption and no downtime.

To allow the tenant's management network floating IP address to work, each tenant must be on a separate VELOS chassis and the management network configured on both chassis must be on the same L2 network.

Prerequisites
=============

- Postman or a similar REST API client.
- Install the VELOS chassis and power on in your data center.
- Complete the `initial configuration of a VELOS system <../install/velos_initial_config.md>`_.
- `How to: Install BIG-IP Next tenant on VELOS <velos_install_ma_tenant_on_velos.rst>`_
  **Download the bundle file**.
- `How to: Install BIG-IP Next tenant on VELOS <velos_install_ma_tenant_on_velos.rst>`_
  **Update the system controller software**.
- `How to: Install BIG-IP Next tenant on VELOS <velos_install_ma_tenant_on_velos.rst>`_
  **Create a chassis partition**.
- `How to: Install BIG-IP Next tenant on VELOS <velos_install_ma_tenant_on_velos.rst>`_
  **Login to the chassis partition WebUI**.
- `How to: Install BIG-IP Next tenant on VELOS <velos_install_ma_tenant_on_velos.rst>`_
  **Upload a tenant image onto the chassis partition**.


- Pre-tenant deployment \- for configuring HA on a tenant. Ensure you have:

  - Management IP, subnet, and virtual (floating) IP addresses.
  - List of VLANs available for the tenant.
  - Control plane (CP) HA VLAN tag, IP address, subnet, and virtual (floating) IP address.
  - Data plane (DP) HA VLAN tag, IP address, and subnet.
  - DP interfaces (SVFs).


Summary
=======

* :ref:`Create a tenant`
* :ref:`Reset the admin password`
* :ref:`Login`
* :ref:`Associate VLANs with the interface`
* :ref:`Update the default network to associate data plane VLANs with the interface`
* :ref:`Create L2 networks`
* :ref:`Create L3 networks`
* :ref:`Get the default system created service`
* :ref:`Create an HA cluster`
* :ref:`Check the Job`
* :ref:`Get instance(s) health`
* :ref:`Create self IPs`
* :ref:`Create an application`
* :ref:`Run failover`
* :ref:`Retrieve system health statistics`

Procedure
=========
**IMPORTANT:** Perform steps on **both** nodes *up to and including* **Associate VLANs with the 1.1 interface**. Starting with **Get the default system created service**, perform steps ***only*** on the **Active node**.

.. _Create a tenant:

---------------
Create a tenant
---------------
1. Command using the API:

``POST https://{{velos_partition_1_mgmt_ip}}:8888/restconf/data/f5-tenants:tenants``

.. code-block:: json

  {
    "tenant": [
      {
        "name": "{{bigip_next_1_name}}",
        "config": {
          "type": "BIGIP-Next",
          "image": "{{velos_bigip_next_version}}",
          "deployment-file": "{{velos_bigip_next_version}}.yaml",
          "nodes": [
            "{{velos_partition_1_node}}"
          ],
          "mgmt-ip": "{{bigip_next_1_mgmt_ip}}",
          "gateway": "{{bigip_next_mgmt_network_gw}}",
          "prefix-length": "{{bigip_next_mgmt_network_mask}}",
          "vlans": [
            "{{bigip_next_internal_vlan_tag}}",
            "{{bigip_next_external_vlan_tag}}",
            "{{bigip_next_ha_dp_vlan_tag}}",
            "{{bigip_next_ha_cp_vlan_tag}}"
          ],
          "vcpu-cores-per-node": "{{velos_tenant_cpu}}",
          "memory": "{{velos_tenant_memory}}",
          "storage": {
            "size": "{{velos_tenant_disk_size}}"
          },
          "cryptos": "enabled",
          "running-state": "deployed"
        }
      }
    ]
  }


Wait for ~4 minutes after creating a tenant to allow all the pods to be running in state.

**Note**: For HA, the tenant name needs to be the same for both tenants in a single pair.

2. To create a list of tenants:

``GET https://{{velos_partition_1_mgmt_ip}}:8888/restconf/data/f5-tenants:tenants``

.. _Reset the admin password:

------------------------
Reset the admin password
------------------------

On the primary node:

``PUT {{bigip_next_mgmt_floating_ip}}:5443/api/v1/me``


.. code-block:: json

   {
     "currentPassword" : "admin",
     "newPassword" : "{{bigip_next_admin_password}}"
   }


.. _Login:

-----
Login
-----

Command using the API:

``PUT https://{{bigip_next_1_mgmt_ip}}:5443/api/v1/login``

**Example**

.. code-block:: json

  {
    "token": "eyJhbGciOiJIUzM4NCIsImtpZCI6IjY0YzMxNDZmLTVmN2QtNDg4Mi05MDRjLWRhY2JhNzYwNGFmZSIsInR5cCI6IkpXVCJ9.eyJFeHRlbnNpb25zIjp7IngtZjUtdXNlci1wYXNzLWNoYW5nZSI6WyJubyJdLCJ4LWY1LXVzZXItcm9sZSI6WyJhZG1pbmlzdHJhdG9yIl0sIngtZjUtdXNlci1zdGF0dXMiOlsiZW5hYmxlZCJdLCJ4LWY1LXVzZXItc3RyYXRlZ3kiOlsibG9jYWwiXX0sIkdyb3VwcyI6bnVsbCwiSUQiOiI5ODIwM2UwZi1lYmQ5LTQ0MTUtOWM2OS00MGU5NWViYWQzN2EiLCJOYW1lIjoiYWRtaW4tY20iLCJhdWQiOlsiIl0sImV4cCI6MTY0OTgwOTEwNywiaWF0IjoxNjQ5ODA1NTA3LCJuYmYiOjE2NDk4MDU1MDcsInN1YiI6Ijk4MjAzZTBmLWViZDktNDQxNS05YzY5LTQwZTk1ZWJhZDM3YSJ9.o-986cUSb6idk9ydXrhlgyTJSs1cELMSIKNjeHLS70jLRQh562si_p_f9O4we2aa",
    "tokenType": "Bearer",
    "expiresIn": 3600,
    "refreshToken": "NDdmMjJkNjgtOWIwNy00NTQwLTk3YTgtMTQxNzA3YWM3ZjMzOqc4PglvRyKQEqW3Z7iwWbFEI0bH6H8Y8tz6j6CcQUatFYo9xAqzWfwovH+7sdnTqg",
    "refreshExpiresIn": 1209600,
    "refreshEndDate": "2022-07-11T23:19:27Z"
  }

An authorization header is automatically generated when you send the request. To display, below the command, click **Authorization**.

After the call is sent, a token bearer is created (Type: Bearer Token). It is used to authenticate the calls that follow for the instance.


.. _Associate VLANs with the interface:

----------------------------------
Associate VLANs with the interface
----------------------------------


``GET {{bigip_next_mgmt_floating_ip}}:5443/api/v1/L1-networks?include=includeAllChildren``

.. code-block:: json

   {
     "_embedded": {
       "L1Networks": [
         {
           "_links": {
             "self": "/L1-networks/d1c51419-39ff-4e87-ad54-5d1480dd74e9",
             "vlans": "/L1-networks/d1c51419-39ff-4e87-ad54-5d1480dd74e9/vlans"
           },
           "id": "d1c51419-39ff-4e87-ad54-5d1480dd74e9",
           "name": "Default L1-Network"
         }
       ]
     },
     "_links": {
       "self": "/L1-networks?"
     },
     "count": 1,
     "total": 1
   }


.. _Update the default network to associate data plane VLANs with the interface:

---------------------------------------------------------------------------
Update the default network to associate data plane VLANs with the interface
---------------------------------------------------------------------------


PUT https://{{bigip_next_1_mgmt_ip}}:5443/api/v1/L1-networks/{{L1_network_id}}


.. code-block::

   {
     "id": "{{L1_network_id}}",
     "name": "Default L1-Network",
     "vlans": [
       {
         "mtu": 1500,
         "name": "{{bigip_next_ha_cp_vlan_name}}",
         "tag": {{bigip_next_ha_cp_vlan_tag}}
       },
       {
         "name": "{{bigip_next_ha_dp_vlan_name}}",
         "tag": {{bigip_next_ha_dp_vlan_tag}},
         "taggedInterfaces": [
           "1.1"
         ]
       },
       {
         "name": "{{bigip_next_external_vlan_name}}",
         "tag": {{bigip_next_external_vlan_tag}},
         "taggedInterfaces": [
           "1.1"
         ]
       },
       {
         "name": "{{bigip_next_internal_vlan_name}}",
         "tag": {{bigip_next_internal_vlan_tag}},
         "taggedInterfaces": [
           "1.1"
         ]
       }
     ]
   }

**Note**: Include all VLANs in the ``PUT`` request; associate VLANs use for data plane (DP) with the 1.1 interface.


.. _Create L2 networks:

------------------
Create L2 networks
------------------

``PUT https://{{bigip_next_1_mgmt_ip}}:5443/api/v1/L2-networks``

.. code-block::

   {
     "name": "HA_L2Network",
     "vlans": [
       "{{bigip_next_internal_vlan_name}}",
       "{{bigip_next_external_vlan_name}}",
       "{{bigip_next_ha_dp_vlan_name}}"
     ]
   }


.. _Create L3 networks:

------------------
Create L3 networks
------------------

``PUT https://{{bigip_next_1_mgmt_ip}}:5443/api/v1/L3-networks``


.. code-block:: json

   {
     "name": "HA_L3Network",
     "l2Networks": [
       "HA_L2Network"
     ],
     "config": {
       "l3NetworkType": "L3Nat"
     }
   }


.. _Get the default system created service:

--------------------------------------
Get the default system created service
--------------------------------------

``GET {{bigip_next_mgmt_floating_ip}}:5443/api/v1/services``


.. _Create an HA cluster:

--------------------
Create an HA cluster
--------------------

``PUT {{bigip_next_mgmt_floating_ip}}:5443/api/v1/services/{{service_id}}/cluster``


.. code-block::

   {
     "clusterManagementIP": "{{bigip_next_mgmt_floating_ip}}",
     "clusterControlPlaneIP": "{{bigip_next_ha_cp_floating_ip}}",
     "dataPlaneVlan": "{{bigip_next_ha_dp_vlan_name}}",
     "controlPlanVlan": "{{bigip_next_ha_cp_vlan_name}}",
     "nodes": [
       {
         "managementAddress": "{{bigip_next_1_mgmt_ip}}",
         "controlPlaneAddress": "{{bigip_next_1_ha_cp_ip}}/{{bigip_next_ha_cp_network_mask}}",
         "name": "node1",
         "dataPlanePrimaryAddress": "{{bigip_next_1_ha_dp_ip}}/{{bigip_next_ha_dp_network_mask}}",
         "username": "{{bigip_next_admin_user}}",
         "password": "{{bigip_next_admin_password}}"
       },
       {
         "managementAddress": "{{bigip_next_2_mgmt_ip}}",
         "controlPlaneAddress": "{{bigip_next_2_ha_cp_ip}}/{{bigip_next_ha_cp_network_mask}}",
         "name": "node2",
         "dataPlanePrimaryAddress": "{{bigip_next_2_ha_cp_ip}}/{{bigip_next_ha_cp_network_mask}}",
         "username": "{{bigip_next_admin_user}}",
         "password": "{{bigip_next_admin_password}}"
       }
     ]
   }

**Note:**
- ``clusterControlPlaneIP``: A floating IP, which is always attached to the ACTIVE node. Use to set on the control plane HA VLAN, which is used by the BIG-IP Next HA control plane components to talk to the active node.
- ``dataPlaneVlan``: A VLAN used by the data plane (TMM) to talk to the peer data plane (TMM).
- ``controlPlaneAddress``: A self IP to set on the control plane HA VLAN, which is used by the BIG-IP Next control plane components to talk to the peer.
- ``dataPlanePrimaryAddress`` and ``dataPlaneSecondaryAddress``: A self IP to set on the data plane HA VLAN.
- ``dataPlaneVlan``: Used by the BIG-IP MA HA data plane components to talk to the peer data plane.

**Important:** The control plane HA VLAN and data plane HA VLAN cannot be the same.

.. _Check the Job:

---------------
Check the Job
---------------

In the Body response, wait until ``status`` changes from ``RUNNING`` to ``SUCCEEDED``.


``GET {{bigip_next_mgmt_floating_ip}}:5443/api/v1/jobs/{{JOB_ID}}``

.. code-block:: json

   {
     "authn": "local"
     "aughz": "global",
     "creationTime": "2022-02-08T23:41:31.226929Z",
     "id": "d568a694-86d2-41b5-84ef-af20f004cc70",
     "message": {
       "code": "16528-00200",
       "detail": {"16528-00200" :[]},
       "title": "clusterConfigSucceeded"
     }
     "owner": "admin",
     "request": {"clusterControlPlaneIP":"10.11.0.19" "clusterManagementIP":"10.144.191.19"}
     "role": "CLUSTERCONFIG",
     "status": "SUCCEEDED",
     "updateTime": "2022-02-08T23:43:22.646159Z",
     "uri": "/services/35dd0822-e58f-45d1-8779-75867978b803/cluster",
     "verb": "PUT"
   }


.. _Get instance(s) health:

----------------------
Get instance(s) health
----------------------

In the Body response, wait until ``isHealthy`` changes from  ``false`` to ``true.``

``GET {{bigip_next_mgmt_floating_ip}}:5443/api/v1/health``

.. code-block:: json

   {
     "_embedded": {
       "health": [
         {
           "id": "49ac95cc-21f6-12d6-f292-6da09c002d83",
           "systemID": "49ac95cc-21f6-12d6-f292-6da09c002d83",
           "state": "ACTIVE",
           "address": "10.11.0.16",
           "address": "10.11.0.16",
           "isHealthy": "true"
         },
         {
           "id": "49ac95cc-51f6-12d6-f292-6da09c002d78",
           "systemId": "49ac95cc-51f6-12d6-f292-6da09c002d78",
           "state": "STANDBY",
           "address": "10.11.0.57",
           "isHealthy": "true"
         }
       ]
     }
   }


.. _Create self IPs:

---------------
Create self IPs
---------------

**Important**: Starting with this step, use the cluster management floating IP address for all API interactions.

Create self-IPs on data plane VLANs:


``PUT https://{{bigip_next_mgmt_floating_ip}}:5443/api/v1/L1-networks/{{L1_network_id}}``

.. code-block: json

   {
     "id": "{{L1_network_id}}",
     "name": "Default L1-Network",
     "vlans": [
       {
         "name": "{{bigip_next_internal_vlan_name}}",
         "tag": {{bigip_next_internal_vlan_tag}},
         "taggedInterfaces": [
           "1.1"
         ],
         "selfIps": [
           {
             "address": "{{bigip_next_1_internal_ip}}/{{bigip_next_internal_network_mask}}",
             "deviceName": "node1"
           },
           {
             "address": "{{bigip_next_2_internal_ip}}/{{bigip_next_internal_network_mask}}",
             "deviceName": "node2"
           },
           {
             "address": "{{bigip_next_internal_floating_ip}}/{{bigip_next_internal_network_mask}}"
           }
         ]
       },
       {
         "name": "{{bigip_next_external_vlan_name}}",
         "tag": {{bigip_next_external_vlan_tag}},
         "taggedInterfaces": [
           "1.1"
         ],
         "selfIps": [
           {
             "address": "{{bigip_next_1_external_ip}}/{{bigip_next_external_network_mask}}",
             "deviceName": "node1"
           },
           {
             "address": "{{bigip_next_2_external_ip}}/{{bigip_next_external_network_mask}}",
             "deviceName": "node2"
           },
           {
             "address": "{{bigip_next_external_floating_ip}}/{{bigip_next_external_network_mask}}"
           }
         ]
       }
     ]
   }


.. _Create an application:

---------------------
Create an application
---------------------

Create an application using AS3 or FAST templates with or without BIG-IP Next Central Manager. Refer to the How to documents listed in the `Application Configuration and Management <https://techcomm.pages.gitswarm.f5net.com/f5-mbip-ve-docs/>`_ section.

**Example** Create an application on BIG-IP Next using AS3 through BIG-IP Next Central Manager


``POST https://{{bigip_next_cm_mgmt_ip}}/mgmt/shared/appsvcs/declare``

.. code-block:: json

   {
     "class": "AS3",
     "action": "deploy",
     "declaration": {
       "class": "ADC",
       "schemaVersion": "3.0.0",
       "id": "openapi-spec",
       "target": {
         "address": "10.1.1.9"
       },
       "ecommerce": {
         "class": "Tenant",
         "http_demo_app_direct": {
           "class": "Application",
           "http_demo_app_direct-virtual": {
             "class": "Service_HTTP",
             "virtualPort": 80,
             "virtualAddresses": [
               "10.1.10.100"
             ],
             "pool": "http_demo_app_direct-pool"
           },
           "http_demo_app_direct-pool": {
             "class": "Pool",
             "members": [
               {
                 "serverAddresses": [
                   "10.1.20.100",
                   "10.1.20.101"
                 ],
                 "servicePort": 8080
               }
             ],
             "monitors": [
               "http"
             ]
           }
         }
       }
     }
   }

Response

.. code-block:: json

   {
     "results": [
       {
         "code": 200,
         "message": "success",
         "host": "f5-appsvcs",
         "tenant": "ecommerce",
         "runTime": 648
       }
     ]
   }


.. _Run failover:

------------
Run failover
------------

``PUT https://{{bigip_next_mgmt_floating_ip}}:5443/api/v1/jobs/{{failover_job}}``


.. _Retrieve system health statistics:

---------------------------------
Retrieve system health statistics
---------------------------------

Refer to: `How to: Retrieve system health statistics <../support/ve_retrieve_health_statistics.md>`_
