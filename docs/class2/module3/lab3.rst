..
  Tami Skelton
  Updated: 10/10/2022.

Lab 3.3 - Review: BIG-IP Next Instance Lifecycle with Central Manager
=====================================================================

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
