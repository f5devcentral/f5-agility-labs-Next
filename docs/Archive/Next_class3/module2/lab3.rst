Lab 2.3 - Instance Networking
===============================================

Overview
~~~~~~~~

Before you can create an application you need to configure the Instance Networking.

"L1-Networks" refers to the mapping of a network to an interface.  On Virtual Edition the first interface (1.0) is reserved for the management interface and the second interface would be for the data-plane (1.1).  On VELOs and rSeries that are r5k and higher there will be a single "1.1" interface that is used for all data-plane networking and VLAG tags can be used to separate networks.  On r2k and r4k rSeries devices that interface will depend on how you have your port bundle configured (i.e. it could start with "1.5").  In this lab exercise you will create two L1-networks.  One for the 1.1 interface called "external-net" and another for the 1.2 interface called "internal-net".

VLANs are associated with "L1-networks".  When you use untagged VLANs you can either specify no VLAN tag or a tag value of "0" (it is treated internally as an untagged VLAN).  You also need to specify whether the VLAN is a member of the "Default VRF".  The "Default VRF" is equivalent to "route domain 0" that is from TMOS and refers to the default network segment.  VRFs can be used to isolate application network traffic to only allow applications to receive/connect to network resources that are part of the attached VRF.

"IP Addresses" are associated with a VLAN.  In a standalone deployment you do not need to specify the "device name".  When HA is configured (later exercise) the IP Address can be associated with specific devices or "float".

Networking
~~~~~~~~~~

#. Click the workspace switcher next to the F5 icon, then **Infrastructure**.

    .. image:: ./lab2_img03_navigation_to_infrastructure2.png
        :scale: 25%

#. Click on the Name of the instance that you previously added ("big-ip-next-01.example.com").

    .. image:: ./click-on-big-ip-next-01.png
        :scale: 50%

#. Next, click on the "Edit" in the top right of the screen.

#. Next, click on the **Networking** tab. You should see that it is empty.

#. Click on "L1 Networks" and click on +Create  

  .. image:: ./create-l1-ntework.png
    :scale: 25%

Use the following name and interfaces

  .. list-table:: L1 Networks 
    :widths: 50 50
    :header-rows: 1

    * - L1 Network Name 
      - Interface Name
    * - external-net   
      - 1.1
    * - internal-net
      - 1.2

  .. image:: ./l1-networks.png
    :scale: 50%


6. Click on VLANs, enter the following values
  
  .. list-table:: VLANs
    :widths: 50 50
    :header-rows: 1

    * - Name
      - L1 Network
    * - external-vlan   
      - external-net
    * - external-vlan
      - internal-net
  
  .. image:: ./vlans.png
    :scale: 50%
  
7. Click on IP Addresses
  
  .. list-table:: IP Addresses
    :widths: 50 50
    :header-rows: 1

    * - Address
      - VLAN
    * - 10.1.10.7/24
      - external-vlan
    * - 10.1.20.7/24
      - internal-vlan
  
  .. image:: ./ip-addresses.png
    :scale: 50%
  
8. Click on "Next"
9. Click on "Deploy"
  
  .. image:: ./networking-deploy.png
    :scale: 25%

Routing & Forwarding
~~~~~~~~~~~~~~~~~~~~

Next we will setup a default route that will be used by the data-plane.

In this lab exercise we are using the "Default VRF".  You could create separate VRFs to isolate VLANs, but that is out of scope for this particular lab.

A static route is associated with a VRF.  This allows routes to be associated with distinct VRFs to control the flow of traffic.  

#. Click the workspace switcher next to the F5 icon, then **Infrastructure**.

    .. image:: ./lab2_img03_navigation_to_infrastructure2.png
        :scale: 25%

#. Click on the Name of the instance that you previously added ("big-ip-next-01.example.com").

    .. image:: ./click-on-big-ip-next-01.png
        :scale: 50%

#. Click on "Routing & Forwarding" and then click on "Default"

.. image:: ./routing-and-forwarding-menu.png
  :scale: 50%

4. Click on "Static Routes" and then "Start Adding"

  .. image:: ./routing-and-forwarding-route-start-adding.png
    :scale: 50%

5. Create a Static Route

  Create a static route using the following inputs and then click on "Save"

    .. list-table:: Static Route
      :widths: 50 50
      :header-rows: 1
      
      * - Name
        - Value
      * - Name
        - default-route
      * - IPv4 Address Prefix
        - 0.0.0.0/0
      * - Resource
        - Route Gateway
      * - IPv4 Address
        - 10.1.10.1  
  
  .. image:: ./static-route-inputs.png
    :scale: 50%

6. Click on "Save" again
7. Click on "Cancel and Exit" to return to "My Instances"