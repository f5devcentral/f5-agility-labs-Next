..
  Tami Skelton
  Updated: 10/10/2022.

Review: BIG-IP Next Instance Lifecycle with Central Manager
===========================================================

Overview
~~~~~~~~
Having completed some work with BIG-IP Next instance management, review the benefits of this approach as compared with the traditional BIG-IP paradigm

Benefits
~~~~~~~~

#. Central Management as standard management model, as compared to instance-by-instance management with optional bolt-on Central Management via BIG-IQ
#. Simplified addition of new instances via Central Manager
#. Central Management can orchestrate upgrades of HA pairs (upgrade standby first, initiate failover, upgrade original active, automatic failback if desired); rollback if upgrade fails.

Summary
~~~~~~~
While the BIG-IP Next model is a significant change, there are tremendous benefits to the everyday operational tasks associated with managing a large collection of BIG-IP instances.

Optional WAF Lab
~~~~~~~~~~~~~~~~

If you're able to finish the lab in sufficient time, there is an optional WAF lab that can give you a deeper experience with the WAF features currenting in BIG-IP Next.

To access the lab, return to the UDF Components tab, and click on "Access" under the Ubuntu Jump Host and select "WAF Lab Guide (optional lab exercises) as seen in the screenshot below.

   .. image:: ./Optional_WAF_Lab.png
      :scale: 25%

.. note:: The lab guide is fully intended to not overlap with the work done for the Agility lab.  You willl be creating separate resources that will not impact your exiting lab progress.
