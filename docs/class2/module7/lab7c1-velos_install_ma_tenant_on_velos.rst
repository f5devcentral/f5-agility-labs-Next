===========================================
How to: Install BIG-IP Next tenant on VELOS
===========================================

Prerequisites
=============

* Postman or a similar REST API client.
* Install the VELOS chassis and power on in your data center.
* Complete the `initial configuration of a VELOS system <../install/velos_initial_config.md>`_.

Summary
=======

* :ref:`Download the bundle file`
* :ref:`Update the system controller software`
* :ref:`Create a chassis partition`
* :ref:`Login to the chassis partition WebUI`
* :ref:`Upload a tenant image onto the chassis partition`
* :ref:`Create VLANs in the VELOS partition`
* :ref:`Create a tenant`


Procedures
==========

.. _Download the bundle file:

------------------------
Download the bundle file
------------------------

#. Log in to the `F5 Beta portal <https://beta.f5.com>`_.
#. From the top menu, click **BIG-IP Next EA Program**, and select **Documents & Builds**.
#. Click a release.
#. In the Downloads area, hover over **...tar.bundle** to display the download icon.
#. To start the download, click the **download icon**.
#. After the download is ready, first open and then save the bundle file to a desired location on a local system.


.. _Update the system controller software:

-------------------------------------
Update the system controller software
-------------------------------------

Update the system controller software (F5OS) to the required controller version: v1.4.x.

#. Log in to the system controller webUI using an account with admin access.
#. On the left, click **SYSTEM SETTINGS > Controller Management**.
#. For **Update Software**, select **Bundled**.
#. For the **ISO Image**, select the full version release ISO image.
#. Click **Save**.

The software on the system controllers is updated.


.. _Create a chassis partition:

--------------------------
Create a chassis partition
--------------------------

#. Log in to the system controller webUI using an account with admin access.
#. On the left, click **CHASSIS PARTITIONS**.
   The Chassis Partitions screen opens with a graphical view of the VELOS chassis.
#. On the chassis graphic, select the available blade where you want to create a partition.
#. Click **Create**.
#. For **Name**, type a name for the chassis partition.
#. In the IPv4 section, type the values for **IP Address**, **Prefix Length**, and **Gateway**.
#. In the IPv6 section, click **Bundled**. For the **ISO image**, select the previously uploaded software image to run on the chassis partition.
#. Click **Save**.
   In the chassis partition list, for the new parition, the Operational State goes from **Starting** to **Running**.

You can now log into the chassis partition using its management IP address to access the partition webUI.


.. _Login to the chassis partition webUI:

------------------------------------
Login to the chassis partition webUI
------------------------------------

#. First-time login after creating a chassis partition requires using default credentials: for both the **Username** and **Password**, type **admin**, and click **Login**.
#. When prompted, type a **New Password**, **Confirm New Password**, and then click **Save**.
#. Login with the new credentials (**Username** and **Password**), and click **Login**.
   The F5OS|VELOS DASHBOARD opens.

.. _Upload a tenant image onto the chassis partition:

------------------------------------------------
Upload a tenant image onto the chassis partition
------------------------------------------------

Upload with the **(1)** GUI, **(2)** API, or **(3)** SSH. --OR-- **(4)** Import from the CLI.


**(1)** GUI

#. With the DASHBOARD open, on the left, click **TENANT MANAGEMENT > Tenant Images**.
#. Click **Upload**.
   The Tenant Images window opens.
#. Select the bundle file.
#. Click **Open**.
   The upload process starts.

   After the upload to the VELOS partition is complete, the bundle file is unbundled into 1) an image file, and 2) a ``.yaml`` deployment file, and replicated across the blades assigned to the partition.

**(2)** API

#. Use SCP via the REST API pointing at the IP address of the chassis partition.
   **Example**

   ``POST https://{{velos_partition_1_mgmt_ip}}:8888/api/data/f5-utils-file-transfer:file/import``

   .. code-block:: json

      {
         "input": [
           {
              "protocol": "scp",
              "remote-host": "{{web-server-ip}}",
              "remote-file": "/path/to/the/bundle/file/f5-mov-tarball-vx.x.x.tgz.bundle",
              "username": "{{web-server-user}}",
              "password": "{{web-server-password}}",
              "local-file": "images",
              "insecure": ""
           }
         ]
      }

#. View the image transfer status for the partition:
   ``POST https://{{velos_partition_mgmt_ip}}:8888/api/data/f5-utils-file-transfer:file/transfer-status``

#. List VELOS tenant images:
   ``GET https://{{velos_partition_mgmt_ip}}:8888/api/data/f5-tenant-images:images``


For detailed usage of the VELOS API, refer to: `F5OS/VELOS - API <https://clouddocs.f5.com/api/velos-api/>`_.

**(3)** SSH

Upload a tenant image using SCP directly to the chassis partition. From a client machine to the IP address of the partition using the IMAGES directory.

**Example**

``scp f5-mov-tarball-vx.x.x.tgz.bundle admin@10.255.0.148:IMAGES``


**(4)** Import from the CLI

#. Login to the CLI for the chassis partition using an account with admin access.
#. Import a tenant image to the chassis partition.
   ``file import remote-port <port-number> username <user> password <password> remote-host <ip-address-or-fqdn> remote-file <remote-file-path> remote-url <full-remote-url> local-file images```

   **Example**

   Import a tenant image from server.company.com:

   .. code-block:: bash

      default-1(config)# file import username admin password remote-url \
      https://server.company.com/images/f5-mov-tarball-vx.x.x.tgz.bundle \
      local-file images


.. _Create VLANs in the VELOS partition:

-----------------------------------
Create VLANs in the VELOS partition
-----------------------------------

Create a VLAN and associate physical interfaces or LAGs with the VLAN. Any host that sends traffic to an interface is logically a member of the VLAN(s) to which that interface or LAG belongs. Create a VLAN before deploying a tenant.

Create VLANs with: **(1)** the CLI or **(2)** GUI.

**(1)** CLI:

#. Use SSH to access the chassis partition. This requires an account with admin access.
#. Create VLANs with the appropriate interface on the blade and VLAN tags:

   .. code-block:: bash

      default-1(config)# interfaces interface 1/1.0 ethernet switched-vlan config trunk-vlans 2001
      default-1(config)# interfaces interface 1/1.0 ethernet switched-vlan config trunk-vlans 3001
      default-1(config)# interfaces interface 1/1.0 ethernet switched-vlan config trunk-vlans 4001
      default-1(config)# interfaces interface 1/1.0 ethernet switched-vlan config trunk-vlans 5001

   **Note:** Make sure the VLANs names match the VLANs names defined in the VELOS chassis partition.

**(2)** GUI:

#. Login to the chassis partition webUI using an account with admin access.
#. On the left, click **NETWORK SETTINGS** &gt; **VLANs**.
   The screen displays VLANs configured for the chassis partition.
#. Click **Add**.
#. In the **Name** field, type a name for the VLAN.
#. In the **VLAN ID**, type a number between 1-4094 for the VLAN.
   The VLAN ID identifies the traffic from hosts in the associated VLAN for an associated interface or LAG.
#. Click **Add VLAN** to create the VLAN.
   The VLAN is created and displays in the VLAN list.
   You can use the VLANs when configuring interfaces and creating LAGs.

You can now deploy a tenant using the same chassis partition webUI.


.. _Create a tenant:

---------------
Create a tenant
---------------

Before starting, decide on which slots to deploy the tenant.
You must have first created a VLAN in the chassis partition.

**Note:**
- A tenant name may only be a maximum of twelve characters.
- For high availability (HA): A tenant name needs to be the same for both tenants in a single HA pair, and created on two different chassis.
- There is support for multi-tenancy; deploying more than one tenant per plade. For more information: `How to: Configure multi-tenancy - BIG-IP Next on VELOS <../install/velos_configure_multi_tenancy.md>`_

Create the tenant with: **(1)** the CLI, **(2)** the API or **(3)** GUI.
If you are deploying HA, create one BIG-IP Next tenant on each chassis partition using the appropriate network information.

**(1)** CLI:

**Example**

.. code-block:: bash

   tenants tenant tenant-bigip-ma1 config type BIG-IP-Next \
   image f5-mov-tarball-vx.x.x deployment-file f5-mov-tarball-vx.x.x.yaml \
   mgmt-ip 10.1.1.7 gateway 10.1.1.1 prefix-length 24 vcpu-cores-per-node 22 \
   memory 79360 running-state deployed vlans [ 5001 2001 3001 ] nodes 2

Wait for ~4 minutes after creating a tenant to allow all the pods to be running in state.


**(2)** API:

**Example**

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

**Note:** VELOS tenants: support in a single blade: 10/12/22 vCPUs. The number of tenants per blade is restricted to two tenants.

For detailed usage of the VELOS CLI, refer to the `F5OS/VELOS - API <https://clouddocs.f5.com/api/velos-api/>`_.

**(3)** GUI:

#. Login to the chassis partition webUI using an account with admin access.
#. On the left, click **TENANT MANAGEMENT** &gt; **Tenant Deployments**.
   The Tenant Deployment screen displays showing the existing tenant deployments and associated details.
#. To add a tenant deployment, click **Add**.
   The Add Tenant Deployment screen displays.
#. For **Name**, type a name for the tenant deployment (up to 12 characters).
   **Note:** The first character in the name cannot be a number. After that, only lowercase alphanumeric characters and hyphens are allowed.
#. Leave **Type** set to the default.
#. For **Image**, select a software image.
#. For **Allowed Slots**, first select the appropriate option:

   - **Partition Member Slots** lists only slots that the chassis partition includes.
   - **Any Slots** lists any slot on the chassis, even if not associated with the chassis partition, and even if no blade is installed in that slot. There is the option of selecting slots 1-8 whether or not they are associated with the chassis partition. This allows for preconfiguring tenant deployments before the hardware is installed and before the partition is configured to include it.

   Then, select the slots (or blades) that you want the tenant to span from the list.

#. For **IP Address**, type the IP address of the tenant.
#. For **Prefix Length**, type a number from 1-32 for the length of the prefix.
#. For **Gateway**, type the IP address of the gateway.
#. For **VLANs**, select the VLAN that you created.
#. For **Resource Provisioning**, select **Recommended**.
    This specifies recommended values for vCPUs and memory for the tenant.
#. For **vCPUs Per Slot**, only select **10**, **12**, or **22**.
#. For **Memory Per Slot**, accept the default values.
#. For **State**, choose **Deployed**.
    This changes the tenant to the Deployed state. The tenant is set up, resources are allocated to the tenant, the image is moved onto the blade, and the software is installed. After those tasks are complete, the tenant is fully deployed and running. It takes a few minutes to complete the deployment and bring up the system.
#. For **Crypto/Compression Acceleration**, select **Enabled**.
    When this option is enabled, the tenant receives dedicated crypto devices proportional to number of vCPU cores. Crypto processing and compression are offloaded to the hardware.
#. For **Appliance Mode**, accept the default value (**Disabled**).
#. Click **Save & Close**.

The tenant is now configured and in the deployed state. When the status is Running, the tenant administrator can use the management IP address to connect to the web based user interface or API, and then continue configuring the tenant system.

The tenant administrator can also connect using SSH to the CLI through the VELOS System Controller.
