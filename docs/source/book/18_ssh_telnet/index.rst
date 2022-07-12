.. raw:: latex

   \newpage

.. _ssh_telnet_index:

18. Connection to network devices
==============================

This section discusses how to connect to network devices via:

* SSH 
* Telnet

Python has several modules that allow you to connect to network devices and execute commands:

* ``pexpect`` - an implementation of ``expect`` in Python

  * this module allows working with any interactive session: ssh, telnet, sftp, etc. 
  * in addition, it makes possible to execute different commands in OS (this can also be done with other modules)
  * while pexpect may be less user-friendly than other modules, it implements
    a more general functionality and allows it to be used in situations where other modules do not work

* ``telnetlib`` - this module allows you connecting via Telnet
  
  * ``netmiko`` version >= 1.0 also has Telnet support, so if ``netmiko`` supports
    the network devices you use, it is more convenient to use it

* ``paramiko`` - his module allows you connecting via SSHv2

  * it is more convenient to use than ``pexpect`` but with narrower functionality (only supports SSH)
  
* ``netmiko`` - module that simplifies the use of paramiko for network devices 

  * ``netmiko`` is a "wrapper" which is oriented to work with network devices

* ``scrapli`` - is a module that allows you to connect to network equipment
  using Telnet, SSH or NETCONF


This section covers all five modules and describes how to connect to several
devices in parallel. Three routers are used in section examples. There are no
requirements for them, only configured SSHv2 and Telnet.

Parameters used in these section:

* user: cisco 
* password: cisco 
* password for enable mode: cisco 
* SSH version 2, Telnet
* IP addresses: 192.168.100.1, 192.168.100.2, 192.168.100.3

.. toctree::
   :maxdepth: 1
   :hidden:

   password
   pexpect
   telnetlib
   paramiko
   netmiko
   scrapli
   further_reading
   ../../exercises/18_exercises
