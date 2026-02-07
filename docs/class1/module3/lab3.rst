The whole point of BIG-IP Next for Kubernetes is the ability to control application delivery to Kubernetes services. Let's create a service 
in our red tenant.

Create a red tenant Deployment and Service
------------------------------------------

We will deploy a nginx TCP demonstration pod as a Kubernetes **Deployment** and then create a simple **Service** pointing to our **Deployment** 
to give it a stable IP address. 

Let's deploy by running the command below:

.. code-block:: bash 
    :caption: Red Deployment

    kubectl apply -f resources/nginx-red-deployment.yaml

Your output should look like this:

.. code-block:: bash 
    :caption: Red Deployment Output
    
    deployment.apps/nginx-deployment created
    service/nginx-app-svc created


Create ingress GatewayType, Gateway, and TCPRoute
-------------------------------------------------

Now the fun part, put on your NetOps hat on and create a Gateway API **GatewayClass** and **Gateway** resources for our service. The NetOps 
user gets to decide what BIG-IP Next for Kubernetes instance to associate **Gateway`s** by creating a **GatewayClass**.

Next the NetOps user gets to decide data center addresses and listener ports to expose our **Service** to the outside world on red's VLAN. 

We can review the resources in the below YAML files:

.. code-block:: yaml 
   :caption: Gateway Class

   apiVersion: gateway.networking.k8s.io/v1
   kind: GatewayClass
   metadata:
     name: f5-gateway-class
     namespace: red
   spec:
     controllerName: "f5.com/f5-gateway-controller"
     description: "F5 BIG-IP Kubernetes Gateway"


.. code-block:: yaml 
   :caption: Gateway 

   apiVersion: gateway.k8s.f5net.com/v1
   kind: Gateway
   metadata:
     name: my-l4route-tcp-gateway
     namespace: red
   spec:
     addresses:
     - type: "IPAddress"
       value: 198.19.19.100
     gatewayClassName: f5-gateway-class
     listeners:
     - name: nginx
       protocol: TCP
       port: 80
       allowedRoutes:
         kinds:
         - kind: L4Route


Then as a DevOps person, we create a Gateway **L4Route** and use its **parentRefs** attribute to say what **Gateway** it should use as seen below. 

.. code-block:: yaml
   :caption: Red L4Route

   apiVersion: gateway.k8s.f5net.com/v1
   kind: L4Route
   metadata:
     name: l4-tcp-app
     namespace: red
   spec:
     protocol: TCP
     parentRefs:
     - name: my-l4route-tcp-gateway
       sectionName: nginx
     rules:
     - backendRefs:
       - name: nginx-app-svc
         namespace: red
         port: 80


Let's deploy these resources in Kubernetes by running the apply command:

.. code-block:: bash
   :caption: Red Gateway Deployment

   kubectl apply -f resources/nginx-red-gw-api.yaml

Your output should look like this:

.. code-block:: bash
   :caption: Gateway Output

  gatewayclass.gateway.networking.k8s.io/f5-gateway-class created
  f5bnkgateway.k8s.f5net.com/f5-bnkgateway created
  gateway.gateway.networking.k8s.io/my-l4route-tcp-gateway created
  l4route.gateway.k8s.f5net.com/l4-tcp-app created



Test BIG-IP Next for Kubernetes ingress
---------------------------------------

We are going to run a **curl** web client command from our docker deployed *infra-client-1* container and see if we can hit the virtual server 
we created in BIG-IP for 198.19.19.100 in our **Gateway** resource in the last step.

.. image:: images/TestingIngressforred.png


.. code-block:: bash
   :caption: Curl

   docker exec -ti infra-client-1 curl -I http://198.19.19.100

Your output should look like this:

.. code-block:: bash 
   :caption: Curl Output

   HTTP/1.1 200 OK
   Server: nginx/1.29.5
   Date: Sat, 07 Feb 2026 00:00:14 GMT
   Content-Type: text/html
   Content-Length: 615
   Last-Modified: Wed, 04 Feb 2026 15:12:20 GMT
   Connection: keep-alive
   ETag: "698361d4-267"
   Accept-Ranges: bytes


How did it get there? 

Let's see what the router container *infra-frr-1* between the *infra-client-1* and the *BIG-IP Next* instances has been peered with by running:

.. code-block:: bash
   :caption: Show BGP Summary

   docker exec -ti infra-frr-1 vtysh -c "show bgp summary"

BGP Summary Output:

.. code-block:: bash 
   :caption: Show BGP Summary

   IPv4 Unicast Summary (VRF default):
   BGP router identifier 192.0.2.250, local AS number 65500 vrf-id 0
   BGP table version 7
   RIB entries 11, using 2112 bytes of memory
   Peers 2, using 1434 KiB of memory
   Peer groups 1, using 64 bytes of memory
   
   Neighbor        V         AS   MsgRcvd   MsgSent   TblVer  InQ OutQ  Up/Down State/PfxRcd   PfxSnt Desc
   *192.0.2.201    4      64443        12        15        0    0    0 00:04:16            3        6 N/A
   *192.0.2.202    4      64443        13        16        0    0    0 00:04:49            3        6 N/A
   
   Total number of neighbors 2
   * - dynamic neighbor
   2 dynamic neighbor(s), limit 100
   
   IPv6 Unicast Summary (VRF default):
   BGP router identifier 192.0.2.250, local AS number 65500 vrf-id 0
   BGP table version 0
   RIB entries 0, using 0 bytes of memory
   Peers 2, using 1434 KiB of memory
   Peer groups 1, using 64 bytes of memory
   
   Neighbor        V         AS   MsgRcvd   MsgSent   TblVer  InQ OutQ  Up/Down State/PfxRcd   PfxSnt Desc
   *192.0.2.201    4      64443        12        15        0    0    0 00:04:16        NoNeg    NoNeg N/A
   *192.0.2.202    4      64443        13        16        0    0    0 00:04:49        NoNeg    NoNeg N/A
   
   Total number of neighbors 2
   * - dynamic neighbor
   2 dynamic neighbor(s), limit 100



Notice both BIG-IP Next instances, 192.168.2.201 and 192.168.2.202 are peered to our router!

What did our BIG-IP Next instances advertise for our red service virtual service? Let's review the routing table on the router:

.. code-block:: bash 
   :caption: Show IP Route

   docker exec -ti infra-frr-1 vtysh -c "show ip route"

Your output should look like this:

.. code-block:: bash
   :caption: Show IP Route Output

   Codes: K - kernel route, C - connected, S - static, R - RIP,
          O - OSPF, I - IS-IS, B - BGP, E - EIGRP, N - NHRP,
          T - Table, v - VNC, V - VNC-Direct, A - Babel, F - PBR,
          f - OpenFabric,
          > - selected route, * - FIB route, q - queued, r - rejected, b - backup
          t - trapped, o - offload failure
   
   K>* 0.0.0.0/0 [0/0] via 198.51.100.1, eth1, 00:38:46
   C>* 192.0.2.0/24 is directly connected, eth0, 00:38:46
   B>* 192.0.2.100/32 [20/0] via 192.0.2.201, eth0, weight 1, 00:05:09
   B>* 192.0.2.101/32 [20/0] via 192.0.2.202, eth0, weight 1, 00:05:45
   B>* 192.0.2.110/32 [20/0] via 192.0.2.201, eth0, weight 1, 00:05:09
   B>* 192.0.2.111/32 [20/0] via 192.0.2.202, eth0, weight 1, 00:05:45
   B>* 198.19.19.100/32 [20/0] via 192.0.2.201, eth0, weight 1, 00:02:33
     *                         via 192.0.2.202, eth0, weight 1, 00:02:33
   C>* 198.51.100.0/24 is directly connected, eth1, 00:38:46


**Class Discuss:** ECMP based ingress routing with BIG-IP Next to pod IP Endpoints with routing to node IPs

Our virtual server address, set by our NetOps user, can be reached at all BIG-IP Next instances peered to the router (we only have two).  
What happens then?

The BIG-IP Next instances could build pool members from **ClusterIP** addresses representing our service, forward to one of them and 
let the **kube-proxy** instance on a node proxy through to a **Endpoint** pod IP as it does for requests made inside the cluster.  

Let's run the below command to see the Red service allocated IP address:

.. code-block:: bash
   :caption: Red Service

   kubectl get service -n red

Red Service Output:

.. code-block:: bash
   :caption: Red Service Output

   NAME            TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
   nginx-app-svc   ClusterIP   10.96.70.44   <none>        80/TCP    4m9s


However, if we did that, it would mean our solution would be wasting CPU resources watching **kube-proxy**, use Linux kernel **netfilter** table 
NAT rules (**iptables**) to get you to the pod hosting an instance of your application **Endpoint**. 

BIG-IP Next for Kubernetes instead discovers the **Endpoint** pod IPs associated with the **Service**, builds a pool of pod IP address, 
discovers which nodes a given pod is deployed, and then routes the load balanced request to the right node IP withe the destination address 
of the pod IP pool member.


.. code-block:: bash
   :caption: Endpoints 

   kubectl get endpointslice -n red


Your output should look like this:

.. code-block:: bash
   :caption: Get Endpoints Output

   NAME                  ADDRESSTYPE   PORTS   ENDPOINTS        AGE
   nginx-app-svc-pdp4q   IPv4          80      10.244.227.213   6m13s


This removes the **kube-proxy** overhead for our ingress traffic. We keep telling everyone that we are saving significant CPU cycles. 
Now you know why!

Check Egress from red tenant container
--------------------------------------

Let's see what SNAT IP we put on traffic coming from our red tenant by running:

.. code-block:: bash
   :caption: Describe red-snat

   kubectl describe f5-spk-snatpool red-snat -n bnk-app

Your output should look like this:

.. code-block:: bash
   :caption: Describe red-snat Output

   Name:         red-snat
   Namespace:    bnk-app
   Labels:       <none>
   Annotations:  <none>
   API Version:  k8s.f5net.com/v1
   Kind:         F5SPKSnatpool
   Metadata:
     Creation Timestamp:  2026-02-06T23:56:11Z
     Finalizers:
       handletmmconfig_inconsistency
     Generation:        1
     Resource Version:  11833
     UID:               867bd4c0-43c4-4645-96c6-9e349d1b239a
   Spec:
     Address List:
       [192.0.2.100 2001::192:0:2:100]
       [192.0.2.101 2001::192:0:2:101]
     Name:                         red-snat
     Shared Snat Address Enabled:  false
   Status:
     Conditions:
       Last Transition Time:  2026-02-06T23:56:11Z
       Message:               
       Observed Generation:   0
       Reason:                Accepted
       Status:                True
       Type:                  Accepted
       Last Transition Time:  2026-02-06T23:56:11Z
       Message:               CR config sent to all grpc endpoints
       Observed Generation:   2
       Reason:                Programmed
       Status:                True
       Type:                  Programmed
     Generation Id:           0
   Events:
     Type    Reason         Age   From               Message
     ----    ------         ----  ----               -------
     Normal  Added/Updated  12m   f5-cne-controller  F5Snatpool bnk-app/red-snat was added/updated


If we did our job right we can generate traffic from the pod in the red namespace and it should show up at the *infra-client-1* container 
from either 192.0.2.100 or 192.0.2.101. 

.. image:: images/EgressTenancy.png

We will generate a outbound web request from the red pod and let the **infra-client-1** web service tell us what it sees about the request. 
We'll run a **curl** web client request through Kubernetes with the below command:


.. code-block:: bash
   :caption: Curl red command

   kubectl exec -ti -n red deploy/nginx-deployment -- curl http://198.51.100.100/txt

Curl output:

.. code-block:: bash
   :caption: Curl Output

   ================================================
    ___ ___   ___                    _
   | __| __| |   \ ___ _ __  ___    /_\  _ __ _ __
   | _||__ \ | |) / -_) '  \/ _ \  / _ \| '_ \ '_ \
   |_| |___/ |___/\___|_|_|_\___/ /_/ \_\ .__/ .__/
                                         |_|  |_|
   ================================================
   
         Node Name: F5 Docker vLab
        Short Name: nginx
   
         Server IP: 198.51.100.100
       Server Port: 80
   
         Client IP: 192.0.2.100
       Client Port: 62899
   
   Client Protocol: HTTP
    Request Method: GET
       Request URI: /txt
   
       host_header: 198.51.100.100
        user-agent: curl/8.14.1


Yeah! We have egress requests from pods in our *red* tenant namespace and are having their traffic SNAT applied appropriately! 

If we wanted to see the address shift to 192.0.2.101, we can continue to make requests, but we will have to wait for the ECMP packet 
flow to send us through a new forwarding virtual server on the BIG-IP. You can repeat the above command until you see it shift.

Let's try our blue tenant. We have the complete deployment for the blue tenant, pod **Deployment**, **Service**, **Gateway**, **TCPRoute** in one YAML 
file we can deploy with one command. Please run the below command:


.. code-block:: bash
   :caption: Apply Blue Deployment

   kubectl apply -f ./resources/nginx-blue-deployment.yaml

Your output should look like this:

.. code-block:: bash
   :caption: Blue Deployment Output

   deployment.apps/nginx-deployment created
   service/nginx-app-svc created
   gateway.gateway.networking.k8s.io/my-l4route-tcp-gateway created
   l4route.gateway.k8s.f5net.com/l4-tcp-app created



Let's see what the SNAT pool for blue looks like by running:

.. code-block:: bash
   :caption: Describe blue-snat 

   kubectl describe f5-spk-snatpool blue-snat -n bnk-app

Describe Output:

.. code-block:: bash
   :caption: Describe blue-snat Output

   Name:         blue-snat
   Namespace:    bnk-app
   Labels:       <none>
   Annotations:  <none>
   API Version:  k8s.f5net.com/v1
   Kind:         F5SPKSnatpool
   Metadata:
     Creation Timestamp:  2026-02-06T23:56:11Z
     Finalizers:
       handletmmconfig_inconsistency
     Generation:        1
     Resource Version:  11834
     UID:               90cc16ff-e9fe-4a9a-8b6c-9c7a47e1e5a3
   Spec:
     Address List:
       [192.0.2.110 2001::192:0:2:110]
       [192.0.2.111 2001::192:0:2:111]
     Name:                         blue-snat
     Shared Snat Address Enabled:  false
   Status:
     Conditions:
       Last Transition Time:  2026-02-06T23:56:11Z
       Message:               
       Observed Generation:   0
       Reason:                Accepted
       Status:                True
       Type:                  Accepted
       Last Transition Time:  2026-02-06T23:56:11Z
       Message:               CR config sent to all grpc endpoints
       Observed Generation:   2
       Reason:                Programmed
       Status:                True
       Type:                  Programmed
     Generation Id:           0
   Events:
     Type    Reason         Age   From               Message
     ----    ------         ----  ----               -------
     Normal  Added/Updated  16m   f5-cne-controller  F5Snatpool bnk-app/blue-snat was added/updated


So we should see the blue tenant make requests from 192.168.2.110 or 192.168.2.111. Let's test by running the curl command:

.. code-block:: bash
   :caption: Curl Command

   kubectl exec -ti -n blue deploy/nginx-deployment -- curl http://198.51.100.100/txt

Curl output:

.. code-block:: bash
   :caption: Curl Output

   ================================================
    ___ ___   ___                    _
   | __| __| |   \ ___ _ __  ___    /_\  _ __ _ __
   | _||__ \ | |) / -_) '  \/ _ \  / _ \| '_ \ '_ \
   |_| |___/ |___/\___|_|_|_\___/ /_/ \_\ .__/ .__/
                                         |_|  |_|
   ================================================
   
         Node Name: F5 Docker vLab
        Short Name: nginx
   
         Server IP: 198.51.100.100
       Server Port: 80
   
         Client IP: 192.0.2.111
       Client Port: 10764
   
   Client Protocol: HTTP
    Request Method: GET
       Request URI: /txt
   
       host_header: 198.51.100.100
        user-agent: curl/7.88.1


Logging, observability, and firewall rules can now identify our Kubernetes *red* and *blue* tenants by simply checking the source IPs of 
the egress traffic coming from their workloads.

Think how important this type of traffic network segmentation is when we are trying to secure traffic from tenant sharing expensive GPUs in 
a cluster, but making request for objects as part of a AI RAG (Retrieval Augmented Generation) pulling in data from a particular corpus of 
policy documents. You need the network segmentation to guarantee security.

Explore BIG-IP Next telemetry through Grafana
----------------------------------------------

We will access the deployed Grafana web user interface through the link provided as part of the lab. Open a browser to your lab Grafana URL.

.. image:: images/GrafanaLogin.png

The default credentials are:

username: **admin**
password: **admin**

You will be prompted to change the password. Go ahead and do that or Skip it. 

Navigate to Dashboard and then load the F5 BNK Dashboard 2.  You will see there are some example Visualization defined for TMM (data path), ACLs, and then per *Red* and *Blue* tenants. 

You've already seen the commands to generate traffic which ingresses and egress Red and Blue tenants. 

See if you can use **docker exec** to generate traffic from the **infra-client-1** ingressing the cluster to your red (http://198.19.19.100/txt) or 
blue (http://198.20.20.100/txt) virtual servers. Observe  their traffic visualizations. 

See if you can use **kubectl exec** to generate traffic from the 'red' and 'blue' pods to your egressing towards your **infra-client-1** (http:/http://198.51.100.100/txt) web services. 
Observe their traffic visualizations. 

In our lab we demonstrated a few things:

1) Kubernetes cluster concepts and details which make BIG-IP Next for Kubernetes valuable
2) How BIG-IP Next for Kubernetes is installed and works with the infrastructure and services
3) How Kubernetes workloads deployed in Kubernetes namespace tenants get BIG-IP Next for Kubernetes application delivery and security for both ingress and egress traffic.