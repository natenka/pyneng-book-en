.. raw:: latex

   \newpage

.. _working_env:

Working environment
-------------------

To complete tasks, you can use several options:

-  prepare a virtual machine
-  use one of the cloud services
-  work without creating a virtual machine


Preparing the virtual machine/host
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
* tips for :ref:`additional_info_pyneng_windows`
* tips for :ref:`additional_info_pyneng_linux`


List of modules to be installed:

::

    pip install pytest pytest-clarity pyyaml tabulate jinja2 textfsm pexpect netmiko graphviz

You also need to install graphviz (example for debian):

::

    apt-get install graphviz

Cloud service
~~~~~~~~~~~~~

Another option is to use one of the following services:
 
-  `repl.it <https://repl.it/>`__ – this service provides an online Python interpreter as well as a graphics editor.
-  `PythonAnywhere <https://www.pythonanywhere.com/>`__ - a separate virtual machine. In the free version you can work only from the command line,
   that is, there is no graphical text editor

Network equipment
~~~~~~~~~~~~~~~~~

For the 18th section of the book, you need to prepare virtual or real
network equipment.

All examples and tasks in which network equipment is used use the same
number of devices: three routers with the following basic settings:

* user: cisco
* password: cisco
* password for enable mode: cisco
* SSH version 2 (version 2 is required), Telnet
* Router IPs: 192.168.100.1, 192.168.100.2, 192.168.100.3
* IP addresses must be accessible from the virtual machine on which you perform tasks
  and can be assigned on physical/logical/loopback interfaces

The topology can be arbitrary. Example topology:

.. figure:: https://raw.githubusercontent.com/pyneng/pyneng.github.io/master/assets/images/gns3_network.png


Basic config:

::

    hostname R1
    !
    no ip domain lookup
    ip domain name pyneng
    !
    crypto key generate rsa modulus 1024
    ip ssh version 2
    !
    username cisco password cisco
    enable secret cisco
    !
    line vty 0 4
     logging synchronous
     login local
     transport input telnet ssh


On some interface, you need to configure an IP address

::

    interface ...
     ip address 192.168.100.1 255.255.255.0


Aliases (optional)

::

    !
    alias configure sh do sh
    alias exec ospf sh run | s ^router ospf
    alias exec bri show ip int bri | exc unass
    alias exec id show int desc
    alias exec top sh proc cpu sorted | excl 0.00%__0.00%__0.00%
    alias exec c conf t
    alias exec diff sh archive config differences nvram:startup-config system:running-config
    alias exec desc sh int desc | ex down
    alias exec bgp sh run | s ^router bgp


Optionally, you can configure the `EEM applet <http://xgu.ru/wiki/Embedded_Event_Manager>`__
to display the commands that the user enters:

::

    !
    event manager applet COMM_ACC
     event cli pattern ".*" sync no skip no occurs 1
     action 1 syslog msg "User $_cli_username entered $_cli_msg on device $_cli_host "
    !


