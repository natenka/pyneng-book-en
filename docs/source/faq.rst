Frequently Asked Questions (FAQ)
------------------------------

Here are some of the most frequently asked questions in reading
book.

How does this differ from the regular Python introductory course?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The main differences are three:

-  The basis is rather brief;
-  Implies a certain domain of knowledge (network-based equipment);
-  All examples are focused on network equipment, as far as possible.

I'm a network engineer. What do I need this book for?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First of all, to automate routine tasks. Automation provides
several advantages:

-  High-level thinking - it’s easier to rise above everything when you free of routine work. You’ll have time and opportunity to think of improvements;
-  Trust - you won’t be afraid to make changes that are often risky because the network is the backbone of every applications and the cost of error is high;
-  A coherent configuration - you will able to automatically create network configuration files, from users and interface descriptions to security functionality and you’ll be less worried about whether you have forgotten something.

Of course, it won’t be that after reading the book you "automate everything and happiness will come" but this is a step in this direction. I am in no way encouraging for all automation to be done via bunch of scripts. If there is some software that solves your needs, that’s great, use it. But if there isn’t or if you are just haven’t thought about it yet, try to start with a simple - Ansible, for example, allows to perfrom many tasks almost "out of the box".

Why then learn Python? The fact is that the same Ansible won’t solve everything. And you may need to add some functionality independently. In addition, apart of equipment configuration adjustment, there are daily routine tasks that can be automated by Python. Let’s just say that if you don’t want to deal with Python, but want to
automate setup and operation processes, please turn attention on Ansible. Even "out of the box" it will be very useful.
Later, if you get taste for it and you want to add something that missed in Ansible, come back :-)

And yes, this course is not only about how to use Python for network equipment configuration and connecton to it.
It’s also about how to solve tasks that are not connected to the equipment. 
For example, change something in multiple configuration files or parse log-file - Python will help you solve these tasks.

Why is this book specifically for network engineers?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are several reasons:

-  Network engineers already have experience in IT and some of concepts are familiar to them and it is likely that some programming basics will be familiar to most. This means that it will be much easier to deal with Python;
-  Working in CLI and writing scripts is unlikely to frighten them;
-  Network engineers have a familiar knowledge domain on which to build examples and tasks.

If you tell on abstract examples «about cats and bunnies», it is one thing. But when you have the ability to use ideas from subject area in examples, things get easier, you get concrete ideas about how to improve a program, a script. And when a person tries to improve it, they start to deal with something new - it’s a very powerful way to move forward.

Why Python?
~~~~~~~~~~~~~~~~~~~~~

The reasons are as follows:

-  In the context of network equipment, Python is often used now;
-  Some equipment has Python embedded or has an API that supports Python;
-  Python is simple enough to learn (of course, it is relatively, and another language may seem simpler but it is rather to be because of experience with the language than because Python is complex);
-  With Python you will not quickly reach the limits of language capabilities;
-  Python can be used not only to write scripts but also to develop applications. Of course, this is not the task of this book but at least you will spend your time on a language that will allow you to go further than simple scripts;
-  For example `GNS3 <https://github.com/GNS3/>`__ is written on Python.

And one more point - in the context of book, Python should not be seen as the only correct variant nor as the «correct» language. No, Python is just a tool like a screwdriver and we learn to use it for specific tasks. That is, there is no ideological background here, no «only Python» and no worship especially. It is strange to worship a screwdriver :-) Everything is simple - there is a good and convenient tool that will approach different tasks. He’s not the best language at all and he’s not the only language at all. Start with it and then you can choose something else if you want to - that knowledge will still be there.

Module I want does not support Python 3
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are several options:

-  Try to find an alternative module that supports Python 3 (not necessarily the latest version of language);
-  Try to find a community version of this module for Python 3. There may not be an official version but the community could translate it independently to version 3, especially if this module is popular;
-  If you use Python 2.7, nothing terrible will happen. If you’re not going to write a huge application but you’re just using Python to automate your problems, Python 2.7 will definitely work.

I don’t know if I need this.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

I, of course, think you need this :-) Otherwise I wouldn’t be writing this book. You don’t necessarily want to go into all this stuff, so you might want to start with `Ansible <https://github.com/Aidar5/nattoeng/blob/master/docs/source/book/Part_VI.md>`__. Perhaps you’ll have enough of it for a long time. Start with simple “show” commands, try to connect first to test equipment (virtual machines), then try to execute “show” command on real network, on 2-3 devices, then on more. If that’s enough for you, you can stop there. The next step is to try using Ansible to generate configuration patterns.

Why would a network engineer need programming?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In my opinion, programming is very important for a network engineer, not because everybody’s talking about it right now or because everybody’s scaring with SDN, job loss or something like that, but because network engineer is constantly facing with:

-  Routine tasks
-  Problems and solutions to be tested;
-  Large quantity of monotonous and repetitive tasks;
-  Large quantity of equipment;

At present, a large amount of equipment still offers us only the command line interface and unstructured output of commands. Software is often limited to a vendor, expensive and has reduced possibilities - we end up doing the same thing over and over again by hand. Even banal things like sending the same show command to 20 devices are not always easy to do. Suppose your SSH client supports this feature. And what if you now need to analyze the output? We are limited by the means we have been given and knowledge of programming, even the most basic, allows us to expand our means and even create new ones. I don’t think everyone should be rushing to learn programming but for an engineer that’s a very important skill. It’s for engineer, not everyone.

Now clearly there is a tendency that can be described by phrase "everybody is learning to code" and it is, in general, good. But programming is not something elementary, it’s difficult, it’s time-consuming, especially if you’ve never had relation to technology world.  It might give an impression that it’s enough to pass “these courses” and after 3 months you are great programmer with high salary. No, this book is not about that :-) We don’t talk about programming as a profession in it and we don’t set such a goal, we’re talking about programming as a tool such as knowing CLI Linux. It’s not that engineers are anything special but, in general:

-  They already have technical education;
-  Many work with command line, in one way or another;
-  They have encountered at least one programming language;
-  They have an «engineering mindset».

This does not mean that everybody else is «not allowed». It will just be easier for the engineers.

Will the book ever be charged with fee?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

No, this book will always be free. I read a paid `online course «Python for network engineers» <https://natenka.github.io/pyneng-online/>`__ (in Russian), but this will not affect this book - it will always be free.
