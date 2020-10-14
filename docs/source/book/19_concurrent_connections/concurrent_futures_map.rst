Method map
~~~~~~~~~

Method syntax:

.. code:: python

    map(func, *iterables, timeout=None)

Method map() is similar to built-in map function: applying func() function to one or more iterable objects. Each call to a function is then started in a separate thread/process. Method map() returns an iterator with function results for each element of object being iterated. The results are arranged in the same order as elements in iterable object.

When working with thread/process pools, a certain number of threads/processes are created and the code is executed in these threads. For example, if the pool is created with 5 threads and function has to be started for 10 different devices, connection will be performed first to the first five devices and then, as they liberated, to the others.

An example of using a map() function with ThreadPoolExecutor (netmiko_threads_map_basics.py file):

.. code:: python

    from datetime import datetime
    import time
    from itertools import repeat
    from concurrent.futures import ThreadPoolExecutor
    import logging

    import netmiko
    import yaml


    logging.getLogger('paramiko').setLevel(logging.WARNING)

    logging.basicConfig(
        format = '%(threadName)s %(name)s %(levelname)s: %(message)s',
        level=logging.INFO)


    def send_show(device, show):
        start_msg = '===> {} Connection: {}'
        received_msg = '<=== {} Received:   {}'
        ip = device['ip']
        logging.info(start_msg.format(datetime.now().time(), ip))
        if ip == '192.168.100.1':
            time.sleep(5)

        with netmiko.ConnectHandler(**device) as ssh:
            ssh.enable()
            result = ssh.send_command(show)
            logging.info(received_msg.format(datetime.now().time(), ip))
            return result


    with open('devices.yaml') as f:
        devices = yaml.safe_load(f)

    with ThreadPoolExecutor(max_workers=3) as executor:
        result = executor.map(send_show, devices, repeat('sh clock'))
        for device, output in zip(devices, result):
            print(device['ip'], output)


Since function should be passed to map() method, send_show() function is created which connects to devices, passes specified show command and returns the result with command output.

.. code:: python

    def send_show(device, show):
        start_msg = '===> {} Connection: {}'
        received_msg = '<=== {} Received:   {}'
        ip = device['ip']
        logging.info(start_msg.format(datetime.now().time(), ip))
        if ip == '192.168.100.1':
            time.sleep(5)

        with netmiko.ConnectHandler(**device) as ssh:
            ssh.enable()
            result = ssh.send_command(show)
            logging.info(received_msg.format(datetime.now().time(), ip))
            return result

Function send_show() outputs log message at the beginning and at the end of work. This will determine when function has worked for the particular device. Also within function it is specified that when connecting to device with address 192.168.100.1, the pause for 5 seconds is required - thus router with this address will respond longer.

Last 4 lines of code are responsible for connecting to devices in separate threads:

.. code:: python

    with ThreadPoolExecutor(max_workers=3) as executor:
        result = executor.map(send_show, devices, repeat('sh clock'))
        for device, output in zip(devices, result):
            print(device['ip'], output)

* ``with ThreadPoolExecutor(max_workers=3) as executor:`` - ThreadPoolExecutor class is initiated in *with* block with indicated number of threads.
* ``result = executor.map(send_show, devices, repeat('sh clock'))`` - map() method is similar to map() function, but here the send_show() function is called in different threads. However, in different threads the function will be called with different arguments:

  * elements of iterable object *devices* and the same command *sh clock*.
  * since instead of a list of commands only one command is used, it must be repeated in some way, so that map() method will set this command to different devices. It uses repeat() function - it repeats command exactly as many times as map() requests
  
*  map() method returns generator. This generator contains results of functions. Results are in the same order as devices in the list of devices, so zip() function is used to combine device IP addresses and command output.

Execution result:

::

    $ python netmiko_threads_map_basics.py
    ThreadPoolExecutor-0_0 root INFO: ===> 08:28:55.950254 Connection: 192.168.100.1
    ThreadPoolExecutor-0_1 root INFO: ===> 08:28:55.963198 Connection: 192.168.100.2
    ThreadPoolExecutor-0_2 root INFO: ===> 08:28:55.970269 Connection: 192.168.100.3
    ThreadPoolExecutor-0_1 root INFO: <=== 08:29:11.968796 Received:   192.168.100.2
    ThreadPoolExecutor-0_2 root INFO: <=== 08:29:15.497324 Received:   192.168.100.3
    ThreadPoolExecutor-0_0 root INFO: <=== 08:29:16.854344 Received:   192.168.100.1
    192.168.100.1 *08:29:16.663 UTC Thu Jul 4 2019
    192.168.100.2 *08:29:11.744 UTC Thu Jul 4 2019
    192.168.100.3 *08:29:15.374 UTC Thu Jul 4 2019

The first three messages indicate when connection was made and to which device:

::

    ThreadPoolExecutor-0_0 root INFO: ===> 08:28:55.950254 Connection: 192.168.100.1
    ThreadPoolExecutor-0_1 root INFO: ===> 08:28:55.963198 Connection: 192.168.100.2
    ThreadPoolExecutor-0_2 root INFO: ===> 08:28:55.970269 Connection: 192.168.100.3

The following three messages show time of receipt of information and completion of the function:

::

    ThreadPoolExecutor-0_1 root INFO: <=== 08:29:11.968796 Received:   192.168.100.2
    ThreadPoolExecutor-0_2 root INFO: <=== 08:29:15.497324 Received:   192.168.100.3
    ThreadPoolExecutor-0_0 root INFO: <=== 08:29:16.854344 Received:   192.168.100.1

Since *sleep* was added for the first device for 5 seconds, information from the first router was actually received later. However, since map() method returns values in the same order as devices in *device* list, the result is:

::

    192.168.100.1 *08:29:16.663 UTC Thu Jul 4 2019
    192.168.100.2 *08:29:11.744 UTC Thu Jul 4 2019
    192.168.100.3 *08:29:15.374 UTC Thu Jul 4 2019

 
Map exception handling
^^^^^^^^^^^^^^^^^^^^^^^^^^

Example of map() with exception handling:

.. code:: python

    from concurrent.futures import ThreadPoolExecutor
    from pprint import pprint
    from datetime import datetime
    import time
    from itertools import repeat
    import logging

    import yaml
    from netmiko import ConnectHandler, NetMikoAuthenticationException


    logging.getLogger('paramiko').setLevel(logging.WARNING)

    logging.basicConfig(
        format = '%(threadName)s %(name)s %(levelname)s: %(message)s',
        level=logging.INFO)


    def send_show(device_dict, command):
        start_msg = '===> {} Connection: {}'
        received_msg = '<=== {} Received:   {}'
        ip = device_dict['ip']
        logging.info(start_msg.format(datetime.now().time(), ip))
        if ip == '192.168.100.1': time.sleep(5)

        try:
            with ConnectHandler(**device_dict) as ssh:
                ssh.enable()
                result = ssh.send_command(command)
                logging.info(received_msg.format(datetime.now().time(), ip))
            return result
        except NetMikoAuthenticationException as err:
            logging.warning(err)


    def send_command_to_devices(devices, command):
        data = {}
        with ThreadPoolExecutor(max_workers=2) as executor:
            result = executor.map(send_show, devices, repeat(command))
            for device, output in zip(devices, result):
                data[device['ip']] = output
        return data


    if __name__ == '__main__':
        with open('devices.yaml') as f:
            devices = yaml.safe_load(f)
        pprint(send_command_to_devices(devices, 'sh ip int br'))


Example is generally similar to the previous one but  NetMikoAuthenticationException was introduced in send_show() function, and the code that started send_show() function in threads is now in send_command_to_devices() function.

When using map() method, exception handling is best done within a function that runs in threads, in this case send_show() function.

