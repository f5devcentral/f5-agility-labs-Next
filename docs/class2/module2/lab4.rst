Lab 2.3 - Instance Networking
===============================================

Overview
~~~~~~~~

Before you can create an application you need to configure the Instance Networking.

Procedure
~~~~~~~~~

#. Click the workspace switcher next to the F5 icon, then **Infrastructure**.

    .. image:: ./lab2_img03_navigation_to_infrastructure2.png
        :scale: 25%

#. Click on the Name of the instance that you previously added ("big-ip-next-01.example.com").

    .. image:: ./click-on-big-ip-next-01.png
        :scale: 50%

#. Next, click on the "Edit" in the top right of the screen.

#. Next, click on the **Networking** tab. You should see that it is empty.

#. Click on "L1 Networks" and click on +Create

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

#. Click on VLANs, enter the following values

.. list-table:: VLANs
  :widths: 50 50
  :header-rows: 1

  * - Name
    - L1 Network
  * - external-vlan   
    - external-net
  * - external-vlan
    - internal-net

#. Click on IP Addresses

.. list-table:: IP Addresses
  :widths: 50 50
  :header-rows: 1

  * - Address
    - VLAN
  * - 10.1.10.7/24
    - external-vlan
  * - 10.1.20.7/24
    - internal-vlan
  
#. Click on "Next"
#. Click on "Deploy"