Examples of TextFSM usage
-----------------------------

This section discusses examples of templates and TextFSM usage.

Section uses parse_output.py script to process command output by template. It is not tied to a specific template and output: template and command output will be passed as arguments:

.. code:: python

    import sys
    import textfsm
    from tabulate import tabulate

    template = sys.argv[1]
    output_file = sys.argv[2]

    with open(template) as f, open(output_file) as output:
        re_table = textfsm.TextFSM(f)
        header = re_table.header
        result = re_table.ParseText(output.read())
        print(result)
        print(tabulate(result, headers=header))


Example of script run:

::

    $ python parse_output.py template command_output

.. note::

    Module **tabulate** is used to display data in tabular form (it must be installed if you want to use this script). A similar output could be received with string formatting but with tabulate it is easier to do.

Data processing by template is always done in the same way. Therefore, script will be the same only template and data will be different.

Starting with a simple example we’ll figure out how to use TextFSM.

show clock
~~~~~~~~~~

The first example is a review of *sh clock* command output (output/sh_clock.txt file):

::

    15:10:44.867 UTC Sun Nov 13 2016

First of all, you have to define variables in template:

* at the beginning of each line there must be a keyword Value
* each variable defines column in table
* next word - variable name
* after name, in parentheses - a regular expression that describes value of a variable

Definition of variables is as follows:

::

    Value Time (..:..:..)
    Value Timezone (\S+)
    Value WeekDay (\w+)
    Value Month (\w+)
    Value MonthDay (\d+)
    Value Year (\d+)

Tips on special symbols: 

* ``.`` - any character 
* ``+`` - one or more repetitions of previous character 
* ``\S`` - all characters except whitespace
* ``\w`` - any letter or number
* ``\d`` - any number

Once variables are defined, an empty line and **Start** state must follow, and then the rule follows starting with space and ``^`` symbol (templates/sh_clock.template file):

::

    Value Time (..:..:..)
    Value Timezone (\S+)
    Value WeekDay (\w+)
    Value Month (\w+)
    Value MonthDay (\d+)
    Value Year (\d+)

    Start
      ^${Time}.* ${Timezone} ${WeekDay} ${Month} ${MonthDay} ${Year} -> Record

Because in this case only one line in the output, it is not necessary to write Record action in template. But it is better to use it in situations where you have to write values and get used to this syntax and not make mistakes when you need to process multiple lines.

When TextFSM handles output strings it substitutes variable by its values. In the end, rule will look like:

::

    ^(..:..:..).* (\S+) (\w+) (\w+) (\d+) (\d+)

When this regular expression applies to *show clock* output, each regular expression group will have a corresponding value:

* 1 group: 15:10:44 
* 2 group: UTC 
* 3 group: Sun 
* 4 group: Nov
* 5 group: 13 
* 6 group: 2016

In rule, in addition to explicit Record action which specifies that record should be placed in final table, the Next rule is also used by default. It specifies that you want to go to the next line of text. Since there is only one line in *sh clock* command output, the processing is completed.

The result of script is:

::

    $ python parse_output.py templates/sh_clock.template output/sh_clock.txt
    Time      Timezone    WeekDay    Month      MonthDay    Year
    --------  ----------  ---------  -------  ----------  ------
    15:10:44  UTC         Sun        Nov              13    2016

    show ip interface brief
~~~~~~~~~~~~~~~~~~~~~~~

In case when you need to process data displayed in columns, TextFSM template is the most convenient.

Template for *show ip interface brief* output (templates/sh_ip_int_br.template file):

::

    Value INTF (\S+)
    Value ADDR (\S+)
    Value STATUS (up|down|administratively down)
    Value PROTO (up|down)

    Start
      ^${INTF}\s+${ADDR}\s+\w+\s+\w+\s+${STATUS}\s+${PROTO} -> Record

In this case, the rule can be described in one line.

Output command (output/sh_ip_int_br.txt file):

::

    R1#show ip interface brief
    Interface                  IP-Address      OK? Method Status                Protocol
    FastEthernet0/0            15.0.15.1       YES manual up                    up
    FastEthernet0/1            10.0.12.1       YES manual up                    up
    FastEthernet0/2            10.0.13.1       YES manual up                    up
    FastEthernet0/3            unassigned      YES unset  up                    up
    Loopback0                  10.1.1.1        YES manual up                    up
    Loopback100                100.0.0.1       YES manual up                    up

The result will be:

::

    $ python parse_output.py templates/sh_ip_int_br.template output/sh_ip_int_br.txt
    INT              ADDR        STATUS    PROTO
    ---------------  ----------  --------  -------
    FastEthernet0/0  15.0.15.1   up        up
    FastEthernet0/1  10.0.12.1   up        up
    FastEthernet0/2  10.0.13.1   up        up
    FastEthernet0/3  unassigned  up        up
    Loopback0        10.1.1.1    up        up
    Loopback100      100.0.0.1   up        up

show cdp neighbors detail
~~~~~~~~~~~~~~~~~~~~~~~~~

Now try to process output of command *show cdp neighbors detail*.

Peculiarity of this command is that the data are not in the same line but in different lines.

File output/sh_cdp_n_det.txt contains output of *show cdp
neighbors detail*:

::

    SW1#show cdp neighbors detail
    -------------------------
    Device ID: SW2
    Entry address(es):
      IP address: 10.1.1.2
    Platform: cisco WS-C2960-8TC-L,  Capabilities: Switch IGMP
    Interface: GigabitEthernet1/0/16,  Port ID (outgoing port): GigabitEthernet0/1
    Holdtime : 164 sec

    Version :
    Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 12.2(55)SE9, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2014 by Cisco Systems, Inc.
    Compiled Mon 03-Mar-14 22:53 by prod_rel_team

    advertisement version: 2
    VTP Management Domain: ''
    Native VLAN: 1
    Duplex: full
    Management address(es):
      IP address: 10.1.1.2

    -------------------------
    Device ID: R1
    Entry address(es):
      IP address: 10.1.1.1
    Platform: Cisco 3825,  Capabilities: Router Switch IGMP
    Interface: GigabitEthernet1/0/22,  Port ID (outgoing port): GigabitEthernet0/0
    Holdtime : 156 sec

    Version :
    Cisco IOS Software, 3800 Software (C3825-ADVENTERPRISEK9-M), Version 12.4(24)T1, RELEASE SOFTWARE (fc3)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2009 by Cisco Systems, Inc.
    Compiled Fri 19-Jun-09 18:40 by prod_rel_team

    advertisement version: 2
    VTP Management Domain: ''
    Duplex: full
    Management address(es):

    -------------------------
    Device ID: R2
    Entry address(es):
      IP address: 10.2.2.2
    Platform: Cisco 2911,  Capabilities: Router Switch IGMP
    Interface: GigabitEthernet1/0/21,  Port ID (outgoing port): GigabitEthernet0/0
    Holdtime : 156 sec

    Version :
    Cisco IOS Software, 2900 Software (C3825-ADVENTERPRISEK9-M), Version 15.2(2)T1, RELEASE SOFTWARE (fc3)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2009 by Cisco Systems, Inc.
    Compiled Fri 19-Jun-09 18:40 by prod_rel_team

    advertisement version: 2
    VTP Management Domain: ''
    Duplex: full
    Management address(es):

From command output you need to get such fields:

* LOCAL_HOST - name of device from prompt
* DEST_HOST - neighbor name
* MGMNT_IP - neighbor IP address 
* PLATFORM - model of neighbor device
* LOCAL_PORT - local interface that connects to a neighbor
* REMOTE_PORT - neighbor port
* IOS_VERSION - neighbor IOS version

Template looks like this (templates/sh_cdp_n_det.template file):

::

    Value LOCAL_HOST (\S+)
    Value DEST_HOST (\S+)
    Value MGMNT_IP (.*)
    Value PLATFORM (.*)
    Value LOCAL_PORT (.*)
    Value REMOTE_PORT (.*)
    Value IOS_VERSION (\S+)

    Start
      ^${LOCAL_HOST}[>#].
      ^Device ID: ${DEST_HOST}
      ^.*IP address: ${MGMNT_IP}
      ^Platform: ${PLATFORM},
      ^Interface: ${LOCAL_PORT},  Port ID \(outgoing port\): ${REMOTE_PORT}
      ^.*Version ${IOS_VERSION},

The result of script execution:

::

    $ python parse_output.py templates/sh_cdp_n_det.template output/sh_cdp_n_det.txt
    LOCAL_HOST    DEST_HOST    MGMNT_IP    PLATFORM    LOCAL_PORT             REMOTE_PORT         IOS_VERSION
    ------------  -----------  ----------  ----------  ---------------------  ------------------  -------------
    SW1           R2           10.2.2.2    Cisco 2911  GigabitEthernet1/0/21  GigabitEthernet0/0  15.2(2)T1

Although rules with variables are described in different lines and accordingly work with different lines, TextFSM collects them into one line of the table. That is, variables that are defined at the beginning of template determine the string of resulting table.

Note that sh_cdp_n_det.txt file has three neighbors, but table has only one neighbor, the last one.

Record
^^^^^^

This is because **Record** action is not specified in template. And only the last line left in final table.

Corrected template:

::

    Value LOCAL_HOST (\S+)
    Value DEST_HOST (\S+)
    Value MGMNT_IP (.*)
    Value PLATFORM (.*)
    Value LOCAL_PORT (.*)
    Value REMOTE_PORT (.*)
    Value IOS_VERSION (\S+)

    Start
      ^${LOCAL_HOST}[>#].
      ^Device ID: ${DEST_HOST}
      ^.*IP address: ${MGMNT_IP}
      ^Platform: ${PLATFORM},
      ^Interface: ${LOCAL_PORT},  Port ID \(outgoing port\): ${REMOTE_PORT}
      ^.*Version ${IOS_VERSION}, -> Record

Now the result is:

::

    $ python parse_output.py templates/sh_cdp_n_det.template output/sh_cdp_n_det.txt
    LOCAL_HOST    DEST_HOST    MGMNT_IP    PLATFORM              LOCAL_PORT             REMOTE_PORT         IOS_VERSION
    ------------  -----------  ----------  --------------------  ---------------------  ------------------  -------------
    SW1           SW2          10.1.1.2    cisco WS-C2960-8TC-L  GigabitEthernet1/0/16  GigabitEthernet0/1  12.2(55)SE9
                  R1           10.1.1.1    Cisco 3825            GigabitEthernet1/0/22  GigabitEthernet0/0  12.4(24)T1
                  R2           10.2.2.2    Cisco 2911            GigabitEthernet1/0/21  GigabitEthernet0/0  15.2(2)T1

Output from all three devices. But LOCAL_HOST variable is not displayed in every line, only in the first one.

Filldown
^^^^^^^^

This is because the prompt from which variable value is taken appears only once. And in order to make it appear in the next lines, use **Filldown** action for LOCAL_HOST variable:

::

    Value Filldown LOCAL_HOST (\S+)
    Value DEST_HOST (\S+)
    Value MGMNT_IP (.*)
    Value PLATFORM (.*)
    Value LOCAL_PORT (.*)
    Value REMOTE_PORT (.*)
    Value IOS_VERSION (\S+)

    Start
      ^${LOCAL_HOST}[>#].
      ^Device ID: ${DEST_HOST}
      ^.*IP address: ${MGMNT_IP}
      ^Platform: ${PLATFORM},
      ^Interface: ${LOCAL_PORT},  Port ID \(outgoing port\): ${REMOTE_PORT}
      ^.*Version ${IOS_VERSION}, -> Record

Now we get this output:

::

    $ python parse_output.py templates/sh_cdp_n_det.template output/sh_cdp_n_det.txt
    LOCAL_HOST    DEST_HOST    MGMNT_IP    PLATFORM              LOCAL_PORT             REMOTE_PORT         IOS_VERSION
    ------------  -----------  ----------  --------------------  ---------------------  ------------------  -------------
    SW1           SW2          10.1.1.2    cisco WS-C2960-8TC-L  GigabitEthernet1/0/16  GigabitEthernet0/1  12.2(55)SE9
    SW1           R1           10.1.1.1    Cisco 3825            GigabitEthernet1/0/22  GigabitEthernet0/0  12.4(24)T1
    SW1           R2           10.2.2.2    Cisco 2911            GigabitEthernet1/0/21  GigabitEthernet0/0  15.2(2)T1
    SW1

LOCAL_HOST now appears in all three lines. But there was another strange effect - the last line in which only LOCAL_HOST column is filled.

Required
^^^^^^^^

The thing is, all variables we’ve determined are optional. Also, one variable with Filldown parameter. And to get rid of the last line, you have to make at least one variable mandatory by using **Required** option:

::

    Value Filldown LOCAL_HOST (\S+)
    Value Required DEST_HOST (\S+)
    Value MGMNT_IP (.*)
    Value PLATFORM (.*)
    Value LOCAL_PORT (.*)
    Value REMOTE_PORT (.*)
    Value IOS_VERSION (\S+)

    Start
      ^${LOCAL_HOST}[>#].
      ^Device ID: ${DEST_HOST}
      ^.*IP address: ${MGMNT_IP}
      ^Platform: ${PLATFORM},
      ^Interface: ${LOCAL_PORT},  Port ID \(outgoing port\): ${REMOTE_PORT}
      ^.*Version ${IOS_VERSION}, -> Record

Now we get the correct output:

::

    $ python parse_output.py templates/sh_cdp_n_det.template output/sh_cdp_n_det.txt
    LOCAL_HOST    DEST_HOST    MGMNT_IP    PLATFORM              LOCAL_PORT             REMOTE_PORT         IOS_VERSION
    ------------  -----------  ----------  --------------------  ---------------------  ------------------  -------------
    SW1           SW2          10.1.1.2    cisco WS-C2960-8TC-L  GigabitEthernet1/0/16  GigabitEthernet0/1  12.2(55)SE9
    SW1           R1           10.1.1.1    Cisco 3825            GigabitEthernet1/0/22  GigabitEthernet0/0  12.4(24)T1
    SW1           R2           10.2.2.2    Cisco 2911            GigabitEthernet1/0/21  GigabitEthernet0/0  15.2(2)T1


show ip route ospf
~~~~~~~~~~~~~~~~~~

Consider the case where we need to process output of *show ip route ospf* command and in routing table there are several routes to the same network.

For routes to the same network, instead of multiple lines where network is repeated, one record will be created in which all available next-hop addresses are in list.

Example of *show ip route ospf* output (output/sh_ip_route_ospf.txt file):

::

    R1#sh ip route ospf
    Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
           D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
           N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
           E1 - OSPF external type 1, E2 - OSPF external type 2
           i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
           ia - IS-IS inter area, * - candidate default, U - per-user static route
           o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
           + - replicated route, % - next hop override

    Gateway of last resort is not set

          10.0.0.0/8 is variably subnetted, 10 subnets, 2 masks
    O        10.1.1.0/24 [110/20] via 10.0.12.2, 1w2d, Ethernet0/1
    O        10.2.2.0/24 [110/20] via 10.0.13.3, 1w2d, Ethernet0/2
    O        10.3.3.3/32 [110/11] via 10.0.12.2, 1w2d, Ethernet0/1
    O        10.4.4.4/32 [110/11] via 10.0.13.3, 1w2d, Ethernet0/2
                         [110/11] via 10.0.14.4, 1w2d, Ethernet0/3
    O        10.5.5.5/32 [110/21] via 10.0.13.3, 1w2d, Ethernet0/2
                         [110/21] via 10.0.12.2, 1w2d, Ethernet0/1
                         [110/21] via 10.0.14.4, 1w2d, Ethernet0/3
    O        10.6.6.0/24 [110/20] via 10.0.13.3, 1w2d, Ethernet0/2


For this example we simplify the task and assume that routes can only be OSPF and only with “O” designation (i.e., only intra-zone routes).

The first version of template:

::

    Value network (\S+)
    Value mask (\d+)
    Value distance (\d+)
    Value metric (\d+)
    Value nexthop (\S+)

    Start
      ^O +${network}/${mask}\s\[${distance}/${metric}\]\svia\s${nexthop}, -> Record


The result is:

::

    network      mask    distance    metric  nexthop
    ---------  ------  ----------  --------  ---------
    10.1.1.0       24         110        20  10.0.12.2
    10.2.2.0       24         110        20  10.0.13.3
    10.3.3.3       32         110        11  10.0.12.2
    10.4.4.4       32         110        11  10.0.13.3
    10.5.5.5       32         110        21  10.0.13.3
    10.6.6.0       24         110        20  10.0.13.3


All right, but we’ve lost path options for routes 10.4.4.4/32 and 10.5.5.5/32. This is logical, because there is no rule that would be appropriate for such a line.

Add a rule to template for lines with partial entries:

::

    Value network (\S+)
    Value mask (\d+)
    Value distance (\d+)
    Value metric (\d+)
    Value nexthop (\S+)

    Start
      ^O +${network}/${mask}\s\[${distance}/${metric}\]\svia\s${nexthop}, -> Record
      ^\s+\[${distance}/${metric}\]\svia\s${nexthop}, -> Record

Now the output is:

::

    network    mask      distance    metric  nexthop
    ---------  ------  ----------  --------  ---------
    10.1.1.0   24             110        20  10.0.12.2
    10.2.2.0   24             110        20  10.0.13.3
    10.3.3.3   32             110        11  10.0.12.2
    10.4.4.4   32             110        11  10.0.13.3
                              110        11  10.0.14.4
    10.5.5.5   32             110        21  10.0.13.3
                              110        21  10.0.12.2
                              110        21  10.0.14.4
    10.6.6.0   24             110        20  10.0.13.3


Partial entries are missing networks and masks, but in previous examples we have already considered Filldown and, if desired, it can be applied here. But for this example we will use another option - List.


List
^^^^

Use List option for *nexthop* variable:

::

    Value network (\S+)
    Value mask (\d+)
    Value distance (\d+)
    Value metric (\d+)
    Value List nexthop (\S+)

    Start
      ^O +${network}/${mask}\s\[${distance}/${metric}\]\svia\s${nexthop}, -> Record
      ^\s+\[${distance}/${metric}\]\svia\s${nexthop}, -> Record


Now the output is:

::

    network    mask      distance    metric  nexthop
    ---------  ------  ----------  --------  -------------
    10.1.1.0   24             110        20  ['10.0.12.2']
    10.2.2.0   24             110        20  ['10.0.13.3']
    10.3.3.3   32             110        11  ['10.0.12.2']
    10.4.4.4   32             110        11  ['10.0.13.3']
                              110        11  ['10.0.14.4']
    10.5.5.5   32             110        21  ['10.0.13.3']
                              110        21  ['10.0.12.2']
                              110        21  ['10.0.14.4']
    10.6.6.0   24             110        20  ['10.0.13.3']



Now *nexthop* column displays a list but so far with one element. When using List the value is a list, and each match with a regular expression will add an item to the list. By default, each next match overwrites the previous one. If, for example, leave Record action for full lines only:

::

    Value network (\S+)
    Value mask (\d+)
    Value distance (\d+)
    Value metric (\d+)
    Value List nexthop (\S+)

    Start
      ^O +${network}/${mask}\s\[${distance}/${metric}\]\svia\s${nexthop}, -> Record
      ^\s+\[${distance}/${metric}\]\svia\s${nexthop},

The result will be:

::

    network      mask    distance    metric  nexthop
    ---------  ------  ----------  --------  ---------------------------------------
    10.1.1.0       24         110        20  ['10.0.12.2']
    10.2.2.0       24         110        20  ['10.0.13.3']
    10.3.3.3       32         110        11  ['10.0.12.2']
    10.4.4.4       32         110        11  ['10.0.13.3']
    10.5.5.5       32         110        21  ['10.0.14.4', '10.0.13.3']
    10.6.6.0       24         110        20  ['10.0.12.2', '10.0.14.4', '10.0.13.3']

Now the result is not quite correct, address hops are assigned to wrong routes. This happens because writing is done on full route entry, then hops of incomplete route entries are collected in the list (other variables are overwritten) and when the next full route entry appears, the list is written to it.

::

    O        10.4.4.4/32 [110/11] via 10.0.13.3, 1w2d, Ethernet0/2
                         [110/11] via 10.0.14.4, 1w2d, Ethernet0/3
    O        10.5.5.5/32 [110/21] via 10.0.13.3, 1w2d, Ethernet0/2
                         [110/21] via 10.0.12.2, 1w2d, Ethernet0/1
                         [110/21] via 10.0.14.4, 1w2d, Ethernet0/3
    O        10.6.6.0/24 [110/20] via 10.0.13.3, 1w2d, Ethernet0/2


In fact, incomplete route entry should really be written when the next full route entry appears, but at the same time they should be written to appropriate route. The following should be done: once full route entry is met, the previous values should be written down and then continue to process the same full route entry to get its information. In TextFSM, you can do this with Continue.Record:

::

      ^O -> Continue.Record

Here, **Record** action tells you to write down the current value of variables. Since there are no variables in this rule, what was in the previous values is written.

**Continue** action says to continue working with the current line as if there was no match. So, the next line of template will work. The resulting template looks like (templates/sh_ip_route_ospf.template):

::

    Value network (\S+)
    Value mask (\d+)
    Value distance (\d+)
    Value metric (\d+)
    Value List nexthop (\S+)

    Start
      ^O -> Continue.Record
      ^O +${network}/${mask}\s\[${distance}/${metric}\]\svia\s${nexthop},
      ^\s+\[${distance}/${metric}\]\svia\s${nexthop},


The result is:

::

    network      mask    distance    metric  nexthop
    ---------  ------  ----------  --------  ---------------------------------------
    10.1.1.0       24         110        20  ['10.0.12.2']
    10.2.2.0       24         110        20  ['10.0.13.3']
    10.3.3.3       32         110        11  ['10.0.12.2']
    10.4.4.4       32         110        11  ['10.0.13.3', '10.0.14.4']
    10.5.5.5       32         110        21  ['10.0.13.3', '10.0.12.2', '10.0.14.4']
    10.6.6.0       24         110        20  ['10.0.13.3']


show etherchannel summary
~~~~~~~~~~~~~~~~~~~~~~~~~

TextFSM is convenient to use to parse output that is displayed by columns or to process output that is in different lines. Templates are less convenient when it is necessary to get several identical elements from one line.

Example of *show etherchannel summary* output (output/sh_etherchannel_summary.txt file):

::

    sw1# sh etherchannel summary
    Flags:  D - down        P - bundled in port-channel
            I - stand-alone s - suspended
            H - Hot-standby (LACP only)
            R - Layer3      S - Layer2
            U - in use      f - failed to allocate aggregator

            M - not in use, minimum links not met
            u - unsuitable for bundling
            w - waiting to be aggregated
            d - default port


    Number of channel-groups in use: 2
    Number of aggregators:           2

    Group  Port-channel  Protocol    Ports
    ------+-------------+-----------+-----------------------------------------------
    1      Po1(SU)         LACP      Fa0/1(P)   Fa0/2(P)   Fa0/3(P)
    3      Po3(SU)          -        Fa0/11(P)   Fa0/12(P)   Fa0/13(P)   Fa0/14(P)

In this case, it is necessary to obtain:

* port-channel name and number. For example, Po1 
* list of all the ports in it. For example, ['Fa0/1', 'Fa0/2', 'Fa0/3']

The difficulty is that ports are in the same line and TextFSM cannot specify the same variable multiple times in line. But it is possible to search multiple times for a match in a line.

The first version of template:

::

    Value CHANNEL (\S+)
    Value List MEMBERS (\w+\d+\/\d+)

    Start
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +${MEMBERS}\( -> Record

Template has two variables:

* CHANNEL - name and number of aggregated port
* MEMBERS - list of ports that are included in an aggregated port. List – type which is specified for this variable.

The result is:

::

    CHANNEL    MEMBERS
    ---------  ----------
    Po1        ['Fa0/1']
    Po3        ['Fa0/11']

So far, only the first port is in output but we need all ports to hit. In this case after match is found, you should continue processing string with ports. That is, use Continue action and describe the following expression.

The only line in template describes the first port. Add a line that describes the next port.

The next version of template:

::

    Value CHANNEL (\S+)
    Value List MEMBERS (\w+\d+\/\d+)

    Start
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +${MEMBERS}\( -> Continue
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +\S+ +${MEMBERS}\( -> Record

The second line describes the same expression, but MEMBERS variable is moved to the next port.

The result is:

::

    CHANNEL    MEMBERS
    ---------  --------------------
    Po1        ['Fa0/1', 'Fa0/2']
    Po3        ['Fa0/11', 'Fa0/12']

Similarly, lines that describe the third and fourth ports should be written to template. But, because the output can have a different number of ports, you have to move Record rule to separate line so that it is not tied to a specific number of ports in string.

For example, if Record is located after the line that describes four ports, for a situation with fewer ports in the line the entry will not be executed.

The resulting template (templates/sh_ether_channelsummary.txt file):

::

    Value CHANNEL (\S+)
    Value List MEMBERS (\w+\d+\/\d+)

    Start
      ^\d+.* -> Continue.Record
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +\S+ +${MEMBERS}\( -> Continue
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +(\S+ +){2} +${MEMBERS}\( -> Continue
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +(\S+ +){3} +${MEMBERS}\( -> Continue

The result of processing:

::

    CHANNEL    MEMBERS
    ---------  ----------------------------------------
    Po1        ['Fa0/1', 'Fa0/2', 'Fa0/3']
    Po3        ['Fa0/11', 'Fa0/12', 'Fa0/13', 'Fa0/14']

Now all ports are in output.

    The template assumes a maximum of four ports in line. If there are more ports, add the corresponding lines to template.

Another variant of *sh etherchannel summary* output (output/sh_etherchannel_summary2.txt file):

::

    sw1# sh etherchannel summary
    Flags:  D - down        P - bundled in port-channel
            I - stand-alone s - suspended
            H - Hot-standby (LACP only)
            R - Layer3      S - Layer2
            U - in use      f - failed to allocate aggregator

            M - not in use, minimum links not met
            u - unsuitable for bundling
            w - waiting to be aggregated
            d - default port


    Number of channel-groups in use: 2
    Number of aggregators:           2

    Group  Port-channel  Protocol    Ports
    ------+-------------+-----------+-----------------------------------------------
    1      Po1(SU)         LACP      Fa0/1(P)   Fa0/2(P)   Fa0/3(P)
    3      Po3(SU)          -        Fa0/11(P)   Fa0/12(P)   Fa0/13(P)   Fa0/14(P)
                                     Fa0/15(P)   Fa0/16(P)

In this output a new variant appears - lines containing only ports.

To process this variant you should modify template (templates/sh_etherchannel_summary2.txt file):

::

    Value CHANNEL (\S+)
    Value List MEMBERS (\w+\d+\/\d+)

    Start
      ^\d+.* -> Continue.Record
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +${MEMBERS}\( -> Continue
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +\S+ +${MEMBERS}\( -> Continue
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +(\S+ +){2} +${MEMBERS}\( -> Continue
      ^\d+ +${CHANNEL}\(\S+ +[\w-]+ +[\w ]+ +(\S+ +){3} +${MEMBERS}\( -> Continue
      ^ +${MEMBERS} -> Continue
      ^ +\S+ +${MEMBERS} -> Continue
      ^ +(\S+ +){2} +${MEMBERS} -> Continue
      ^ +(\S+ +){3} +${MEMBERS} -> Continue

The result will be:

::

    CHANNEL    MEMBERS
    ---------  ------------------------------------------------------------
    Po1        ['Fa0/1', 'Fa0/2', 'Fa0/3']
    Po3        ['Fa0/11', 'Fa0/12', 'Fa0/13', 'Fa0/14', 'Fa0/15', 'Fa0/16']

This concludes our work with TextFSM templates.

Examples of templates for Cisco and other vendors can be seen in project
`ntc-ansible <https://github.com/networktocode/ntc-templates/tree/89c57342b47c9990f0708226fb3f268c6b8c1549/templates>`__.

