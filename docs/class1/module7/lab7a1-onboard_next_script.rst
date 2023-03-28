===================================================================
How to: Onboard BIG-IP Next and Central Manager with a setup script
===================================================================

There are common Prerequisites, but procedures with separate scripts.

- :ref:`BIG-IP Next Central Manager`
- :ref:`BIG-IP Next`
- :ref:`Add BIG-IP Next instance to BIG-IP Central Manager`
- :ref:`Troubleshooting: BIG-IP Next data plane`


Prerequisites
=============
- VMware ESXi version 7.x
- Instance: 8 vCPUs, 16GB RAM
- Disk resource: 

  - BIG-IP Next: 80GB of disk per BIG-IP Next virtual machine (VM)
  - BIG-IP Next Central Manager (CM): 160GB of disk per BIG-IP Next CM VM

- Access to the Early Access (EA) program on the `Beta portal <https://beta.f5.com>`_ to download an OVA file
- Inputs for the setup script from your network; refer to sections for type inputs 

**Important:** Known issue in v0.11.0: Installation may fail on networks that also provide IPv6 via SLAAC or DHCPv6.  Contact F5 before installing in these environments for workaround recommendations.


.. _BIG-IP Next Central Manager:

BIG-IP Next Central Manager 
===========================

-------
Summary
-------

- :ref:`Install Image CM`
- :ref:`Launch the console and login CM`
- :ref:`Start the script and type inputs CM`

Procedures
----------

.. _Install Image CM:

Install image (CM)
^^^^^^^^^^^^^^^^^^
**Download OVA file**

#. Log in to the `Beta portal <https://beta.f5.com>`_: type your **Email address** and **Password**.
#. Click **Log In**.
#. From the top menu, click the **BIG-IP Next EA Program** list and select **Documents & Builds**.
#. Click a build number.

   **Example**: *<build number>*-BIG-IP Next VE *<date>*.

#. Scroll down to the list of files.
#. Click an OVA file (or the download icon).

   **Example**: BIG-IP-Next-Central-Manager-*<build number>*.ova

#. Move the OVA file to a desired location.

**Deploy OVF template**

#. Log in to the VMware vSphere Client.
#. In the right pane, click **ACTIONS** -> **Deploy OVF Template**. 
#. Locate the previously downloaded OVA file to use to install a VM:

    #. Select **Local file** and then click **UPLOAD FILES**.
    #. Select the OVA file, and click **Open**.

#. Click **NEXT**.
#. Type a VM name and select a location. Click **NEXT**.
#. Select a location for the compute resource and click **NEXT**.
#. Select the storage for the configuration and disk files, and click **NEXT**.
#. For select clone options, click the **Customize this virtual machine's hardware** and **Power on virtual machine after creation** check boxes, and click **NEXT**.
#. For the **Network adapter 1**, make a selection for your management network.

      #. From the list, click **Browse**.
         The Select Network window opens.
      #. Type a search team and then select from the list.
      #. Click **OK**.     

#. Review the settings for Customize hardware and click **NEXT**.
#. Review the settings for Ready to complete and click **FINISH**.

.. _Launch the console and login CM:

Launch the console and login (CM)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
#. In left pane, click the icon for the **Hosts and Cluster** menu.
   The **Hosts and Cluster** icon is the first one from the left.
#. Navigate to a desired location.
#. To open the console for the VM, in the right pane, click inside the black box.
   The web console opens.
#. For both the **central manager login** and **Password**, type ``admin``.
   You are required to change your password... displays
#. Change your password. Type:
   - **Current password**
   - **New password**
   - **Retype new password**
   The Welcome information displays.

.. _Start the script and type inputs CM:

Start the script and type inputs (CM)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
#. At the ``$`` prompt, type ``setup``
   Welcome... and instructions display.
   A default value for each parameter is in brackets. Example inputs are in parentheses.
#. Type inputs

**Network with DHCP**
::

   - Hostname (example.com) []:
   - Primary NTP server address (0.pool.ntp.org) (optional):
   - Alternate NTP server address (1.pool.ntp.org (optional):
   - Continue to **Summary and Installation**

**Network with a management IP address/No DHCP**
::

   - Hostname (example.com) []:
   - Management IP Address & Network Mask [192.168.1.245/24]: 
   - Management Network Default Gateway [192.168.1.1]:
   - Primary DNS nameserver (192.168.1.2) []:
   - Alternate DNS nameserver (192.168.1.3) (optional):
   - Primary NTP server address (0.pool.ntp.org) (optional):
   - Alternate NTP server address (1.pool.ntp.org (optional):

**Summary and Installation**
::

   At the prompt to review your settings, type ``y`` to confirm.
   
**Sample output**
::

     Summary
     -------

      Hostname: bigipnext1
      DHCP for Management Network: False
      Management IP Address: 192.168.1.245/24
      Management Gateway: 192.168.1.1
      DNS Servers: 192.168.1.1
      NTP Servers: 0.pool.ntp.org
      Would you like to complete configuration with these parameters (Y/n) [N]: y


   After the setup of management networking is complete, there is a prompt to install
   Central Manager. Type ``y`` to confirm.

**Sample output**
::

   ...
   [info] k3s node 'node/central-manager-cabc00f4' is ready

   Would you like to start the Central Manager application installation (Y/n) [Y]:

**Note**: While waiting for BIG-IP Next Central Manager to complete installation, you can start to install BIG-IP Next.

.. _BIG-IP Next:

BIG-IP Next 
===========

-------
Summary
-------

- :ref:`Install Image Next`
- :ref:`Launch the console and login Next`
- :ref:`Start the script and type inputs Next`
- :ref:`Add BIG-IP Next instance to BIG-IP Central Manager`
- :ref:`Troubleshooting: BIG-IP Next data plane`

Procedures
----------

.. _Install Image Next:

Install image
^^^^^^^^^^^^^

**Download OVA file**

#. Log in to the `Beta portal <https://beta.f5.com>`_: type your **Email address** and **Password**.
#. Click **Log In**.
#. From the top menu, click the **BIG-IP Next EA Program** list and select **Documents & Builds**.
#. Click a build number. **Example**: &lt;build number&gt; BIG-IP Next VE &lt;date&gt;.
#. Scroll down to the list of files.
#. Click an OVA file (or the download icon). **Example**: BIG-IP-Next-&lt;build number&gt;.ova 
   The file downloads.
#. Move the OVA file to a desired location.

**Deploy OVF template**

#. Log in to the VMware vSphere Client.
#. In the right pane, click **ACTIONS** &gt; **Deploy OVF Template**. 
#. Locate the previously downloaded OVA file to use to install a VM:

    #. Select **Local file** and then click **UPLOAD FILES**.
    #. Select the OVA file, and click **Open**.

#. Click **NEXT**.
#. Type a VM name and select a location. Click **NEXT**.
#. Select a location for the compute resource and click **NEXT**.
#. Select the storage for the configuration and disk files, and click **NEXT**.
#. For select clone options, click the **Customize this virtual machine's hardware** and **Power on virtual machine after creation** check boxes, and click **NEXT**.
#. For the customize hardware settings, remove an error for **CD/DVD drive 1**:

    #. Start with the **Virtual Hardware** tab selected (default). Click anywhere in the row for **CD/DVD drive 1** to highlight. 
    #. To remove, click the icon that is a circle with an X. 

#. In the upper-right corner of the window, click **ADD NEW DEVICE** -> **Network Adapter**.
#. For the **Network adapter 1** and **New Network** settings, make a selection for each.

      #. From the list, click **Browse**.
         The Select Network window opens.
      #. Type a search team and then select from the list.
      #. Click **OK**.     

#. Review the settings for Customize hardware and click **NEXT**.
#. Review the settings for vApp properties and click **NEXT**.
#. Review the settings for Ready to complete and click **FINISH**.

.. _Launch the console and login Next:

Launch the console and login
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In the left pane, from the top menu, click the **VMs and Templates** icon.
   The icon is the second from the left.
#. Expand the links to navigate to the correct link. 
#. To open the console for the VM, in the right pane, click inside the black box.
   The web console opens and the prompt displays.
#. For both **login** and **Password**, type ``admin``.
   Welcome to Ubuntu... displays.


.. _Start the script and type inputs Next:

Start the script and type inputs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Use the setup script to configure management networking in a static IP address
environment and also assist in creating data plane networking configuration.

Help text for the script:

This script will help you initially configure the basic network settings for this instance.
  
Please answer the questions below. A default value for each parameter is indicated within the brackets. Example inputs are included within parentheses.

#. At the prompt, type ``setup``.
#. Type inputs.

**Network with DHCP**
::

   - Hostname:
   - Primary NTP server address (0.pool.ntp.org) (optional):
   - Alternate NTP server address (1.pool.ntp.org (optional):

   - Continue to: **Configure Data Plane Networking**

**Network with a management IP address/No DHCP**
::

   - Hostname:
   - Management IP Address & Network Mask [192.168.1.245/24]: 
   - Management Network Default Gateway [192.168.1.1]:
   - Primary DNS nameserver (192.168.1.2):
   - Alternate DNS nameserver (192.168.1.3) (optional):
   - Primary NTP server address (0.pool.ntp.org) (optional):
   - Alternate NTP server address (1.pool.ntp.org (optional):

**Configure Data Plane Networking**
::

   After configuring management networking, configure data plane networking.

   - VLAN1 name (external) (optional)
     - VLAN1 interface [1.1]
     - VLAN1 self IPv4 [10.0.0.1/24]
   - VLAN2 name (internal) (optional)
     - VLAN2 interface [1.2]
     - VLAN2 self IPv4 [172.16.0.1/24]
   - Configure Static Route? (Y/n) [N]
     - Data-Plane Route *Please use a /24 due to known issues* [10.254.254.0/24]
     - Data-Plane Route Gateway [10.0.0.254]
     - Data-Plane Interface [1.1]

**Important:** Known issue in v0.11.0 for static route: You cannot create a route that overlaps with 10.43.0.0/16. You can safely create a route outside of that range. The `setup` script will also only validate single VLAN (1-arm) configuration. If you need to create a static route with a 2-arm configuration, F5 recommends creating the route outside of the setup script (for example, via Postman).

**Admin Password**

   There is a prompt to set an admin password used by both 
   the API and Linux console. SSH access to the admin account is blocked in the SSH server configuration.

   - Please enter a new password for the 'admin' user: 
   - Please confirm the new password for the 'admin' user:
  
**Note:** The password must be a minimum of 12 characters and contain mixed-case letters and numbers.


**Summary**

   The Summary provides an opportunity to review the inputs. There is also the ``setup`` command with additional CLI arguments for running the setup script again or on another host (for example, set up a secondary instance).

**Sample output**
::

     Summary
     -------

      Hostname: bigipnext1
      DHCP for Management Network: False
      Management IP Address: 192.168.1.245/24
      Management Gateway: 192.168.1.1
      DNS Servers: 192.168.1.1
      NTP Servers: 0.pool.ntp.org

   - Would you like to complete configuration with these parameters (Y/n) [N]: --> type ``Y``.

   The script runs. It may take several minutes to complete.

**Sample output**
::

   ...
   [info] Setting hostname to bigipnext1...
   [info] Waiting for BIG-IP Next API to be ready.  This can take up to 5 minutes ...
   [info] Setting admin password
   [info] Getting login token
   [info] Waiting for all services to be ready
   [info] Sending initial onboarding request
   [info] Sending data plane route request
   [info] Setup completed successfully.

.. _Add BIG-IP Next instance to BIG-IP Central Manager:

Add BIG-IP Next instance to BIG-IP Central Manager
--------------------------------------------------

Refer to: `How to: Add a BIG-IP Next instance to BIG-IP Next Central Manager <../use_cm/cm_add_instance_to_big_ip_ma.html>`_

.. _Troubleshooting\: BIG-IP Next data plane:

Troubleshooting: BIG-IP Next data plane 
---------------------------------------

Optional tools to help verify your device is correctly connected to the network on BIG-IP Next devices:

  :ref:`setup --dataplane-debug`

  :ref:`setup --api [URI]`

.. _setup --dataplane-debug:

setup --dataplane-debug
^^^^^^^^^^^^^^^^^^^^^^^


The command `setup --dataplane-debug` allows you to run commands like ``ping``, ``tcpdump``, and ``ip`` from the data plane process (data plane is not visible from Linux console).

**Note**: You can review: `How to: Enable debug sidecar and run command line tools <../support/debug_sidecar.html>`_

**Example**: Verify self IP
::

    admin@mBIP:~$ setup --dataplane-debug
    ...
    5: external: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
        link/ether 00:50:56:8c:4b:20 brd ff:ff:ff:ff:ff:ff
        inet 10.0.0.1/24 brd 10.0.0.0 scope global external
           valid_lft forever preferred_lft forever
        inet6 fe80::250:56ff:fe8c:4b20/64 scope link
           valid_lft forever preferred_lft forever

**Example**: Verify data plane gateway
::

    admin@mBIP:~$ setup --dataplane-debug
    /ping -c 4 10.0.0.254
    PING 10.0.0.254 (10.0.0.254) 56(84) bytes of data.
    64 bytes from 10.0.0.254: icmp_seq=1 ttl=64 time=2.03 ms
    64 bytes from 10.0.0.254: icmp_seq=2 ttl=64 time=1.38 ms
    64 bytes from 10.0.0.254: icmp_seq=3 ttl=64 time=1.69 ms
    64 bytes from 10.0.0.254: icmp_seq=4 ttl=64 time=0.798 ms

    --- 10.0.0.1 ping statistics ---
    4 packets transmitted, 4 received, 0% packet loss, time 3002ms
    rtt min/avg/max/mdev = 0.798/1.474/2.027/0.452 ms

**Example**: Verify data plane route
::

    admin@mBIP:~$ setup --dataplane-debug
    /ip route
    ...
    10.254.254.0/24 via 10.0.0.254 dev external
    ...

**Example**: ``tcpdump``
::

    /tcpdump -i 0.0 -nn icmp
    tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
    listening on 0.0, link-type EN10MB (Ethernet), snapshot length 65535 bytes
    16:04:46.067480 IP 10.0.0.100 > 10.0.0.1: ICMP echo request, id 151, seq 1, length 64 in slot1/tmm6 lis= port=1.1 trunk=
    16:04:46.067603 IP 10.0.0.1 > 10.0.0.100: ICMP echo reply, id 151, seq 1, length 64 out slot1/tmm6 lis= port=1.1 trunk=

.. _setup --api [URI]:

setup --api [URI]
^^^^^^^^^^^^^^^^^

It can be helpful to inspect/update the API elements of BIG-IP Next.

There are utilities that can take your `admin` API password either as a file input or via an environment variable.  

**Example:** Setting the password via environment variable
::

  $ export PASSWORD=[your admin API password]

You can then perform ``GET`` requests on the API. The script will automatically prefix ``https://[Management IP]:5443/api/v1``
::

    admin@mBIP:~$ setup --api health/ready
    {
      "_embedded": {
          "systemReady": {
              "ready": true,
              "servicesNotReady": null
          }
      }
    }

**Example**: Changing self IP (advanced example)

.. code-block::
    :emphasize-lines: 3

    # capture existing output, calling setup --api L1-networks?include=vlans|more to get the correct IDs
    # [UUID] is meant for documentation purposes and is not the real value!
    admin@mBIP:~$ setup --api --strip L1-networks/[UUID]/vlans/[UUID2] > vlan.json
    admin@mBIP:~$ cat vlan.json
    {
      "id": "[UUID2]",
      "name": "external",
      "selfIps": [
          {
            "address": "10.1.10.9/24"
          }
      ],
      "untaggedInterfaces": [
        "1.1"
      ]
    }

**Example**: Edit to change self-ip value

.. code-block::
    :emphasize-lines: 34

    admin@mBIP:~$ vi vlan.json
    # send API call with updated value
    admin@mBIP:~$ setup --api -X PUT L1-networks/[UUID]/vlans/[UUID2] --data @./vlan.json
    {
        "authn": "local",
        "authz": "global",
        "creationTime": "2023-02-28T16:09:54.110335Z",
        "id": "52c4ecba-73d5-4989-bb71-b453bf6087ee",
        "message": {
            "code": "13167-02000",
            "detail": "{\"13167-02000\":[\"52c4ecba-73d5-4989-bb71-b453bf6087ee\",\"SUCCEEDED\"]}",
            "links": {
                "about": "",
                "resourceLink": "/L1-networks/[UUID]/vlans/[UUID2]"
            },
            "title": "jobUpdateSuccessful"
        },
        "messageHistory": [
            {
                "code": "13167-02000",
                "creationtime": "2023-02-28T16:09:54.209334Z",
                "detail": "{\"13167-02000\":[\"52c4ecba-73d5-4989-bb71-b453bf6087ee\",\"SUCCEEDED\"]}",
                "title": "jobUpdateSuccessful"
            }
        ],
        "owner": "admin",
        "request": "{\"id\":\"3fa5d769-3451-4a43-a9e4-5074e109d1a1\",\"name\":\"external\",\"selfIps\":[{\"address\":\"10.1.10.10/24\"}],\"untaggedInterfaces\":[\"1.1\"]}",
        "role": "AGENTCONFIG",
        "status": "SUCCEEDED",
        "updateTime": "2023-02-28T16:09:54.209334Z",
        "uri": "/L1-networks/[UUID]/vlans/[UUID2]",
        "verb": "PUT"
    }
    admin@mBIP:~$ setup --api --strip L1-networks/[UUID]/vlans/[UUID2]
    {
        "id": "3fa5d769-3451-4a43-a9e4-5074e109d1a1",
        "name": "external",
        "selfIps": [
            {
                "address": "10.1.10.10/24"
            }
        ],
        "untaggedInterfaces": [
            "1.1"
        ]
    }

