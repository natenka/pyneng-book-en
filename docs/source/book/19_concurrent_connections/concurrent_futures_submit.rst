Method submit and work with futures
-------------------------------

Method submit() differs from map() method:

* submit() runs only one function in thread
* submit() can run different functions with different unrelated arguments, when map() must run with iterable objects as arguments
* submit() immediately returns the result without having to wait for function execution
* submit() returns special Future object that represents execution of function.

  * submit() returns Future in order that the call of submit() does not block the code. Once submit() has returned Future, code can be executed further. And once all functions in threads are running, you can start requesting Future if results are ready. Or take advantage of special function as_completed(), which requests the result itself and code gets it when it’s ready

* submit() returns results in readiness order, not in argument order
* submit() can pass key arguments when map() only position arguments

Method submit() uses `Future <https://en.wikipedia.org/wiki/Futures_and_promises>`__ object - an object that represents a delayed computation. This object can be resquested for status (completed or not), and results or exceptions can be obtained from the job. Future does not need to create manually, these objects are created by submit().


Example of running a function in threads using submit() (netmiko_threads_submit_basics.py file)

.. code:: python

    from concurrent.futures import ThreadPoolExecutor, as_completed
    from pprint import pprint
    from datetime import datetime
    import time
    import logging

    import yaml
    from netmiko import ConnectHandler, NetMikoAuthenticationException


    logging.getLogger("paramiko").setLevel(logging.WARNING)

    logging.basicConfig(
        format = '%(threadName)s %(name)s %(levelname)s: %(message)s',
        level=logging.INFO)


    def send_show(device_dict, command):
        start_msg = '===> {} Connection: {}'
        received_msg = '<=== {} Received: {}'
        ip = device_dict['ip']
        logging.info(start_msg.format(datetime.now().time(), ip))
        if ip == '192.168.100.1':
            time.sleep(5)

        with ConnectHandler(**device_dict) as ssh:
            ssh.enable()
            result = ssh.send_command(command)
            logging.info(received_msg.format(datetime.now().time(), ip))
        return {ip: result}


    with open('devices.yaml') as f:
        devices = yaml.safe_load(f)

    with ThreadPoolExecutor(max_workers=2) as executor:
        future_list = []
        for device in devices:
            future = executor.submit(send_show, device, 'sh clock')
            future_list.append(future)
        # the same in the form of list comprehensions:
        # future_list = [executor.submit(send_show, device, 'sh clock') for device in devices]
        for f in as_completed(future_list):
            print(f.result())


The rest of the code has not changed, so you only need to deal with the block which runs send_show() function in threads:

.. code:: python

    with ThreadPoolExecutor(max_workers=2) as executor:
        future_list = []
        for device in devices:
            future = executor.submit(send_show, device, 'sh clock')
            future_list.append(future)
        for f in as_completed(future_list):
            print(f.result())


The rest of the code has not changed, so only block that runs send_show() needs an attention:

.. code:: python

    with ThreadPoolExecutor(max_workers=2) as executor:
        future_list = []
        for device in devices:
            future = executor.submit(send_show, device, 'sh clock')
            future_list.append(future)
        for f in as_completed(future_list):
            print(f.result())

Now block *with* has two cycles:

* ``future_list`` - a list of Future objects:

  * submit() function is used to create Future object
  * submit() expects the name of function to be executed and its arguments

* the next cycle runs through future_list using as_completed() function. This function returns a Future objects only when they have finished or been cancelled. Future is then returned as soon as work is completed, not in the order of adding to future_list


.. note::

    Creation of list with Future can be done with list comprehensions: 
    ``future_list = [executor.submit(send_show, device, 'sh clock') for device in devices]``

The result is:

::

    $ python netmiko_threads_submit_basics.py
    ThreadPoolExecutor-0_0 root INFO: ===> 17:32:59.088025 Connection: 192.168.100.1
    ThreadPoolExecutor-0_1 root INFO: ===> 17:32:59.094103 Connection: 192.168.100.2
    ThreadPoolExecutor-0_1 root INFO: <=== 17:33:11.639672 Received: 192.168.100.2
    {'192.168.100.2': '*17:33:11.429 UTC Thu Jul 4 2019'}
    ThreadPoolExecutor-0_1 root INFO: ===> 17:33:11.849132 Connection: 192.168.100.3
    ThreadPoolExecutor-0_0 root INFO: <=== 17:33:17.735761 Received: 192.168.100.1
    {'192.168.100.1': '*17:33:17.694 UTC Thu Jul 4 2019'}
    ThreadPoolExecutor-0_1 root INFO: <=== 17:33:23.230123 Received: 192.168.100.3
    {'192.168.100.3': '*17:33:23.188 UTC Thu Jul 4 2019'}


Please note that the order is not preserved and depends on which function was previously completed.

Future
~~~~~~

An example of running send_show() function with submit() and displaying information about Future (note the status of Future at different points in time):

.. code:: python

    In [1]: from concurrent.futures import ThreadPoolExecutor

    In [2]: from netmiko_threads_submit_futures import send_show

    In [3]: executor = ThreadPoolExecutor(max_workers=2)

    In [4]: f1 = executor.submit(send_show, r1, 'sh clock')
       ...: f2 = executor.submit(send_show, r2, 'sh clock')
       ...: f3 = executor.submit(send_show, r3, 'sh clock')
       ...:
    ThreadPoolExecutor-0_0 root INFO: ===> 17:53:19.656867 Connection: 192.168.100.1
    ThreadPoolExecutor-0_1 root INFO: ===> 17:53:19.657252 Connection: 192.168.100.2

    In [5]: print(f1, f2, f3, sep='\n')
    <Future at 0xb488e2ac state=running>
    <Future at 0xb488ef2c state=running>
    <Future at 0xb488e72c state=pending>

    ThreadPoolExecutor-0_1 root INFO: <=== 17:53:25.757704 Received: 192.168.100.2
    ThreadPoolExecutor-0_1 root INFO: ===> 17:53:25.869368 Connection: 192.168.100.3

    In [6]: print(f1, f2, f3, sep='\n')
    <Future at 0xb488e2ac state=running>
    <Future at 0xb488ef2c state=finished returned dict>
    <Future at 0xb488e72c state=running>

    ThreadPoolExecutor-0_0 root INFO: <=== 17:53:30.431207 Received: 192.168.100.1
    ThreadPoolExecutor-0_1 root INFO: <=== 17:53:31.636523 Received: 192.168.100.3

    In [7]: print(f1, f2, f3, sep='\n')
    <Future at 0xb488e2ac state=finished returned dict>
    <Future at 0xb488ef2c state=finished returned dict>
    <Future at 0xb488e72c state=finished returned dict>


In order to look at Future, several lines with information output are added to the script (netmiko_threads_submit_futures.py):

.. code:: python

    from concurrent.futures import ThreadPoolExecutor, as_completed
    from pprint import pprint
    from datetime import datetime
    import time
    import logging

    import yaml
    from netmiko import ConnectHandler, NetMikoAuthenticationException


    logging.getLogger("paramiko").setLevel(logging.WARNING)

    logging.basicConfig(
        format = '%(threadName)s %(name)s %(levelname)s: %(message)s',
        level=logging.INFO)


    def send_show(device_dict, command):
        start_msg = '===> {} Connection: {}'
        received_msg = '<=== {} Received: {}'
        ip = device_dict['ip']
        logging.info(start_msg.format(datetime.now().time(), ip))
        if ip == '192.168.100.1':
            time.sleep(5)

        with ConnectHandler(**device_dict) as ssh:
            ssh.enable()
            result = ssh.send_command(command)
            logging.info(received_msg.format(datetime.now().time(), ip))
        return {ip: result}


    def send_command_to_devices(devices, command):
        data = {}
        with ThreadPoolExecutor(max_workers=2) as executor:
            future_list = []
            for device in devices:
                future = executor.submit(send_show, device, command)
                future_list.append(future)
                print('Future: {} for device {}'.format(future, device['ip']))
            for f in as_completed(future_list):
                result = f.result()
                print('Future done {}'.format(f))
                data.update(result)
        return data


    if __name__ == '__main__':
        with open('devices.yaml') as f:
            devices = yaml.safe_load(f)
        pprint(send_command_to_devices(devices, 'sh clock'))




The result is:

::

    $ python netmiko_threads_submit_futures.py
    Future: <Future at 0xb5ed938c state=running> for device 192.168.100.1
    ThreadPoolExecutor-0_0 root INFO: ===> 07:14:26.298007 Connection: 192.168.100.1
    Future: <Future at 0xb5ed96cc state=running> for device 192.168.100.2
    Future: <Future at 0xb5ed986c state=pending> for device 192.168.100.3
    ThreadPoolExecutor-0_1 root INFO: ===> 07:14:26.299095 Connection: 192.168.100.2
    ThreadPoolExecutor-0_1 root INFO: <=== 07:14:32.056003 Received: 192.168.100.2
    ThreadPoolExecutor-0_1 root INFO: ===> 07:14:32.164774 Connection: 192.168.100.3
    Future done <Future at 0xb5ed96cc state=finished returned dict>
    ThreadPoolExecutor-0_0 root INFO: <=== 07:14:36.714923 Received: 192.168.100.1
    Future done <Future at 0xb5ed938c state=finished returned dict>
    ThreadPoolExecutor-0_1 root INFO: <=== 07:14:37.577327 Received: 192.168.100.3
    Future done <Future at 0xb5ed986c state=finished returned dict>
    {'192.168.100.1': '*07:14:36.546 UTC Fri Jul 26 2019',
     '192.168.100.2': '*07:14:31.865 UTC Fri Jul 26 2019',
     '192.168.100.3': '*07:14:37.413 UTC Fri Jul 26 2019'}


Since two threads are used by default, only two out of three Future shows running status. The third is in pending state and is waiting for queue to arrive.

Processing of exceptions
~~~~~~~~~~~~~~~~~~~~

If there is an exception in function execution, it will be generated when the result is obtained

For example, in device.yaml file the password for device 192.168.100.2 was changed to the wrong one:

::

    $ python netmiko_threads_submit.py
    ===> 06:29:40.871851 Connection to device: 192.168.100.1
    ===> 06:29:40.872888 Connection to device: 192.168.100.2
    ===> 06:29:43.571296 Connection to device: 192.168.100.3
    <=== 06:29:48.921702 Received result from device: 192.168.100.3
    <=== 06:29:56.269284 Received result from device: 192.168.100.1
    Traceback (most recent call last):
    ...
      File "/home/vagrant/venv/py3_convert/lib/python3.6/site-packages/netmiko/base_connection.py", line 500, in establish_connection
        raise NetMikoAuthenticationException(msg)
    netmiko.ssh_exception.NetMikoAuthenticationException: Authentication failure: unable to connect cisco_ios 192.168.100.2:22
    Authentication failed.

Since an exception occurs when result is obtained, it is easy to add exception processing (netmiko_threads_submit_exception.py file):


.. code:: python

    from concurrent.futures import ThreadPoolExecutor, as_completed
    from pprint import pprint
    from datetime import datetime
    import time
    from itertools import repeat
    import logging

    import yaml
    from netmiko import ConnectHandler
    from netmiko.ssh_exception import NetMikoAuthenticationException

    logging.getLogger("paramiko").setLevel(logging.WARNING)

    logging.basicConfig(
        format = '%(threadName)s %(name)s %(levelname)s: %(message)s',
        level=logging.INFO)

    start_msg = '===> {} Connection: {}'
    received_msg = '<=== {} Received: {}'


    def send_show(device_dict, command):
        ip = device_dict['ip']
        logging.info(start_msg.format(datetime.now().time(), ip))
        if ip == '192.168.100.1': time.sleep(5)
        with ConnectHandler(**device_dict) as ssh:
            ssh.enable()
            result = ssh.send_command(command)
            logging.info(received_msg.format(datetime.now().time(), ip))
        return {ip: result}


    def send_command_to_devices(devices, command):
        data = {}
        with ThreadPoolExecutor(max_workers=2) as executor:
            future_ssh = [
                executor.submit(send_show, device, command) for device in devices
            ]
            for f in as_completed(future_ssh):
                try:
                    result = f.result()
                except NetMikoAuthenticationException as e:
                    print(e)
                else:
                    data.update(result)
        return data


    if __name__ == '__main__':
        with open('devices.yaml') as f:
            devices = yaml.safe_load(f)
        pprint(send_command_to_devices(devices, 'sh clock'))
The result is:

::

    $ python netmiko_threads_submit_exception.py
    ThreadPoolExecutor-0_0 root INFO: ===> 07:21:21.190544 Connection: 192.168.100.1
    ThreadPoolExecutor-0_1 root INFO: ===> 07:21:21.191429 Connection: 192.168.100.2
    ThreadPoolExecutor-0_1 root INFO: ===> 07:21:23.672425 Connection: 192.168.100.3
    Authentication failure: unable to connect cisco_ios 192.168.100.2:22
    Authentication failed.
    ThreadPoolExecutor-0_1 root INFO: <=== 07:21:29.095289 Received: 192.168.100.3
    ThreadPoolExecutor-0_0 root INFO: <=== 07:21:31.607635 Received: 192.168.100.1
    {'192.168.100.1': '*07:21:31.436 UTC Fri Jul 26 2019',
     '192.168.100.3': '*07:21:28.930 UTC Fri Jul 26 2019'}


Of course, exception handling can be performed within send_show() function, but it is just an example of how you can work with exceptions when using a Future.

