..  Author: Tami Skelton 09/15/2022

===============================================================================
How to: Create a BIG-IP Next HA instance on VE from BIG-IP Next Central Manager
===============================================================================

Overview
========
A BIG-IP Next HA instance consists of an active and a standby node. Create a BIG-IP Next HA instance to safe-guard your applications and traffic in the event there is an issue with the provider the BIG-IP Next is installed on. In such an event, the active node in your BIG-IP Next HA instance will failover to the standby node with no disruption to current traffic or applications.

Prerequisites
=============
Before you can create a BIG-IP Next HA instance, you must have two standalone instances already onboarded to BIG-IP Next Central Manager. Refer to `How to: Onboard BIG-IP Next (GUI) <../configure_new_big_ip_ma_instance>`_ for details.

- Two standalone BIG-IP Next instances running the same software version in the same provider (VE) and in a healthy state.
- A license for the BIG-IP Next instance you want to designate as the active node.
- Between five and seven self IP addresses that are not assigned anywhere else in your network
- Traffic VLANS - One IP address for each BIG-IP Next instance, plus a floating IP address
- Control Plane HA VLAN - One IP address for each BIG-IP Next instance (by default, this VLAN will use two additional IP addresses on the management network, but you have the option to create a separate VLAN)
- Data Plane HA VLAN - One IP address for each BIG-IP Next instance

---------
Procedure
---------
#. Navigate back to the **Infrastructure** page of BIG-IP Next Central Manager that was used during module 2.
#. Add a second BIG-IP Next instance to your infrastructure (the second instance is at 10.1.1.8)
   
   .. image:: ./lab6_img01_add_second_instance.png
      :scale: 25%
#. Follow procedure that was used to onboard initial device with instance 2 (10.1.1.8). Upon completion you should have four total instances as shown in below screenshot.
   
   .. image:: ./lab6_img02_my_instances_list_4_instances.png
      :scale: 25%
#. Click the mode link (the word "Standalone" under the "Mode" column) of the standalone BIG-IP Next instance (big-ip-next-01.f5demo.com) you want to become the active node in the BIG-IP Next HA instance.
   
   .. image:: ./lab6_img03_enable_ha_on_instance1.png
      :scale: 25%
#. The High Availability panel opens.
#. Click the **Enable HA** button.
#. The HA Nodes panel opens.
   The name and IP address for the BIG-IP Next instance that you selected displays.
   This will be the active node in the BIG-IP Next HA instance after it’s created.
#. From the **Available Standby Instances** list, select the BIG-IP Next instance (big-ip-next-02.f5demo.com) you want to designate as the standby node in the BIG-IP Next HA instance, and click the **Next** button.
   
   .. image:: ./lab6_img05_add_ha_select_ha_node.png
      :scale: 25%
#. BIG-IP Next Central Manager displays only BIG-IP Next instances that meet the requirements to be a standby node. That is, only healthy BIG-IP Next instances running the same software version as the future active node on the same provider (VE).
#. From the **Control Plane VLAN** list, you'll need to create a new Control Plane VLAN for this BIG-IP Next HA instance by clicking the **Create VLAN** link. 
   
   .. image:: ./lab6_img06_add_ha_vlan_config.png
      :scale: 25%
   Use the name "control-plane-vlan" for this and Tag "3999"

   VLAN Name:

   .. code-block:: console

      control-plane-vlan

   Tag:

   .. code-block:: console

      3999
   
   .. image:: ./lab6_img07_add_ha_create_controlplane_vlan.png
      :scale: 25%
   Click **Save**
#. From the **Data Plane VLAN** list for this BIG-IP Next HA instance, you'll need to create a new Data Plane VLAN by clicking the **Create VLAN** link.
   
   .. image:: ./lab6_img08_add_ha_vlan_data_plane.png
      :scale: 25%
   Use the name "data-plane-vlan" for this and select Untagged Interface: "1.3"

   VLAN Name:

   .. code-block:: console

      data-plane-vlan
   
   Untagged Interface:

   .. code-block:: console

      1.3
   
   .. image:: ./lab6_img09_add_ha_create_dataplane_vlan.png
      :scale: 25%
   Click **Save**
#. After you select the VLANs, click the **Next** button.
   
   .. image:: ./lab6_img10_add_ha_vlan_configuration_complete.png
      :scale: 25%
#. The HA Properties panel opens.
#. Enter the **HA Name** (example: ha1) for this high availability pair of nodes. This name should be unique for each pair managed by the Next Central Manager instance.

Name:

.. code-block:: console

   ha1

#. Enter the **HA Management (Floating) IP Address** (should be 10.1.1.9) for use when failing over to the standby node.

HA Management IP:

.. code-block:: console

   10.1.1.9

#. Enter the **Control Plane** and **Data Plane IP** addresses that the active and standby nodes use for communication with each other. (See screenshot for suggested IP addresses)
   Address for big-ip-next-01.f5demo.com: control plane: 10.1.1.77/24; data plane: 10.1.40.7/24 and big-ip-next-02.f5demo.com: control plane: 10.1.1.88/24; data plane: 10.1.40.8/24

big-ip-next-01 control plane IP:

.. code-block:: console

   10.1.1.77/24

big-ip-next-01 data plane IP:

.. code-block:: console

   10.1.40.7/24

big-ip-next-02 control plane IP:

.. code-block:: console

   10.1.1.88/24

big-ip-next-02 data plane IP:

.. code-block:: console

   10.1.40.8/24

#. The subnet for the data plane address should not be shared with either management or control plane addresses. Management and control plane can share a common subnet or they can each use unique subnets. The addresses for the nodes in an HA pair must use corresponding subnets for management, control plane and data plane.
   
   .. image:: ./lab6_img11_add_ha_properties_configured.png
      :scale: 25%
#. Click **Next**.
#. The Traffic VLANs panel opens and displays the VLANs you specified for traffic.
   
   .. image:: ./lab6_img12_traffic_vlan_unconfigured.png
      :scale: 25%
#. You can have only one IP address for each active and standby node, but you can add additional floating IP addresses by clicking on the VLAN name.
#. Click each VLAN to open a panel to add the respective Active, Standby, and Floating IP addresses. You may need to reassign the IP address that shows up when you first enter the page from a Floating Self-IP to the Active Self-IP.
   
   .. image:: ./lab6_img13_add_ha_traffic_vlans_external.png
      :scale: 25%

   Active Node IP Address:

   .. code-block:: console
      
      10.1.10.7/24
   
   Standby Node IP Address:

   .. code-block:: console

      10.1.10.8/24

   Floating IP Address:
   
   .. code-block:: console

      10.1.10.9/24

   Click **Save**
   
   .. image:: ./lab6_img14_add_ha_traffic_vlans_internal.png
      :scale: 25%

   Active Node IP Address:
   
   .. code-block:: console

      10.1.20.7/24

   Standby Node IP Address:
   
   .. code-block:: console

      10.1.20.8/24

   Floating IP Address:
   
   .. code-block:: console

      10.1.20.9/24

   Click **Save**
#. When each VLAN displays a green check mark, you can proceed.
   
   .. image:: ./lab6_img15_add_ha_traffic_vlans_overview_complete.png
      :scale: 25%
   Click **Next**.
#. The Summary screen opens to display the BIG-IP Next HA configuration you specified.
   
   .. image:: ./lab6_img16_add_ha_summary.png
      :scale: 25%
   Click **Deploy to HA**.

   .. note::
      If Deploy to HA is not available, you may need to revisit the HA VLANs section of the HA Parameters panel (if there is not a green checkmark next to HA VLANs; workaround the missing green checkmark by pulling down each of the VLAN selections and re-selecting the items already selected; after this, you can return to the Summary Page to Deploy to HA).

#. You'll be prompted to confirm the deployment.
   
   .. image:: ./lab6_img17_add_ha_deploy_to_ha_confirm.png
      :scale: 25%
   Click **Yes, Deploy**
   It may take several minutes for the HA Deployment process to complete
#. Assuming a successful deployment, you'll see the properties of the HA instance you just created.
   
   .. image:: ./lab6_img18_ha_instance_upgrade.png

#. Click **Exit** and you'll be returned to your My Instances list and you'll see that you now have an instance of type HA under the "Mode" column.
   
   .. image:: ./lab6_img19_my_instances_list_ha_complete.png
      :scale: 25%

------
Result
------
BIG-IP Next Central Manager converts the standalone BIG-IP Next instance to a BIG-IP Next HA instance.

