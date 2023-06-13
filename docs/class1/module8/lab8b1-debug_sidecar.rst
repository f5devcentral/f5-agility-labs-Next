=======================================================
How to: Enable debug sidecar and run command line tools
=======================================================

Prerequisites 
=============
* BIG-IP Next VE or VELOS instance
* Postman or a similar REST API client
* Download a Postman collection from an early access (EA) release at the `Beta portal <https://beta.f5.com>`_ site
* Login completed to a BIG-IP Next VE or VELOS instance 
* Management IP address
* SSH key generated

------------------
Command line tools
------------------

+----------------+--------------------------------------------------------------------------------+
| Tool           | Description                                                                    |
+================+================================================================================+
| **tmctl**      | Displays various TMM traffic processing statistics, such as pool and virtual   |
|                | server connections.                                                            |
+----------------+--------------------------------------------------------------------------------+
| **bdt_cli**    | Displays TMM networking information such as ARP and route entries.             |
+----------------+--------------------------------------------------------------------------------+
| **tcpdump**    | Displays packets sent and received on the specified network interface.         |
+----------------+--------------------------------------------------------------------------------+
| **ping**       | Send ICMP ECHO_REQUEST packets to remote hosts.                                | 
+----------------+--------------------------------------------------------------------------------+
| **traceroute** | Displays the packet route in hops to a remote host.                            | 
+---------------+---------------------------------------------------------------------------------+

---------
Procedure
---------
Commands in the downloaded Postman collection (refer to the `Prerequisites <#prerequisites)>`_ are located at:

Initial Setup BIG-IP Next Instance(s) VE > Setup Dataplane-debug (ONLY for Standalone)

#. Enable debug sidecar (Postman: **PUT Enable Dataplane-debug Setup**)

   .. code-block:: console

      PUT https://{{BIGIP_MGMT_ADDR}}:{{BIGIP_MGMT_PORT}}/api/v1/actions/systems/{systemId}/dataplane-debug/enable

   .. code-block::

    {
      "sshPublicKey": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDP36nLphAhvmardZO66VOLqwPVEzhKJbmaI8Trm+8I0xiDM4EfW9nw8pKzaa2EQ/CAaw+t2UmrapVcGEK5Ajljfd/mQu/Wa9gwKFkEZ0K10wJHq3U7VJLQFEM7fpQgkl/vxErf0xqw78rEmhfiHoBLWZlTXjO/bEyOWpT2UqYnrkIWxucu+B+dlfE9AFBfKziL9Xahhm+WaGicJMGE0jS4B54rkz092GefohiNUtFcANH4xnTWFHxxwfN6JeisTEalWoxO+vZqTpUgiKWb5uTM08pNRQTvv2JuSZ8EiL7oOs33ZibvqL1e4E0xlZdipqrdASiiDlqzGWSLF4H0lnuGYYVKU2cZ4Y3EOpAMcggFMVKblt7pZWqJKR9yLnUIa4A3YzSKNX30AFb69uev1EaOwA7AkTE/qrFIHY4evDXYFANfpPrp6ClaiccQuP+bzwUyEfrSChRHSluRq2YjUZywyJKX3tVrRpwxzKBClnU1jbpMGmLuO0brvoXJH5rwws= root@centos-8-base",
      "allowedIps": [
        "0.0.0.0/0"
      ],
      "username": "<name>",
      "port": <port number>
    }

   **Important:** Replace ``<name>`` and ``<port number>`` with your own username and port. Refer to the **Example** after step 2.

   **Note:** ``0.0.0.0/0`` in ``allowed IPs`` is a workaround only for the early access (EA) v0.10.0 release so that the BIG-IP Next debug sidecar will be available on any machine.

#. Log in to the debug sidecar

   .. code-block:: console

      ssh <username>@<management IP address> -p <port number>

   **Example**

   .. code-block:: console

      ssh david@10.144.191.#. -p 5444


#. Run a debug sidecar command line tool

   Refer to the table - `command line tools <#command-line-tools>`_ - for a list and description of debug sidecar command line tools.

   **Example**

   .. code-block:: console

      traceroute google.com

   <output>

#. Disable debug sidecar (Postman: **PUT Disable Dataplane-debug Setup**)

   .. code-block:: console

      PUT https://10.0.0.122:5443/api/v1/actions/systems/{systemId}/dataplane-debug/disable
