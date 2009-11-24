.. _faq-index:

==========================
Frequently Asked Questions
==========================

Is it possible to connect multiple modems to a RapidSMS instance?
=================================================================

Yes. Let's suppose you want to connect to three different GSM networks.
In your local.ini file, under the [rapidsms] section, list the backends:
::

   backends=gsmnet1,gsmnet2,gsmnet3

Then in the same file, add a corresponding section for each network:
::

    [gsmnet1]
    device=/dev/ttyUSB0
    type=gsm

    [gsmnet2]
    device=/dev/ttyUSB1
    type=gsm

    [gsmnet3]
    device=/dev/ttyUSB2
    type=gsm

Note: you should use the pygsm backend for this to work.

What if RapidSMS is not installed on the same system as the GSM device?
=======================================================================

One possible solution to this use case it to use the `Kannel gateway`_ on
the machine which hosts the modem(s) and use the `Kannel backend`_ on the
RapidSMS server.

.. _`Kannel gateway`: http://www.kannel.org/
.. _`Kannel backend`: http://gist.github.com/214985

Is it possible to install RapidSMS on Windows?
==============================================

Some people reported having successfully installed RapidSMS on Windows. However
it is not as much tested as on Linux, therefore the advice is to preferably run it
on a Linux system.
