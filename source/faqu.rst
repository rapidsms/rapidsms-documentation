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
