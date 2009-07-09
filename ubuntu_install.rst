.. _PyGSM: http://github.com/rapidsms/pygsm/tree/master
.. _synaptic: https://help.ubuntu.com/community/SynapticHowto
.. _apt: http://www.debian.org/doc/manuals/apt-howto/ch-apt-get.en.html
.. _RapidSMS: http://www.rapidsms.org
.. _Get Ubuntu: http://www.ubuntu.com/getubuntu 
.. _Ubuntu: http://www.ubuntu.com
.. _GitHub: http://github.com
.. _RapidSMS Forks: http://github.com/unicefinnovation/rapidsms/network/members
_rapidsms_forks `RapidSMS Forks`_

Installing RapidSMS on Ubuntu 9.10 Jaunty
==========================================

1 Intro
-------

These instructions explain how to install RapidSMS_ with a PyGSM_ backend for communicating with GSM handsets and modems.

There are many options in how to install the software so a few comments on why to do it as described below:

 * **Stay as close to standard Ubuntu Software install processes**: 
      In Ubuntu software is installed from Ubuntu central repositories
      using a package manager like synaptic_ or the command line apt_ 
      commands. In setting up a RapidSMS server using *standard* Ubuntu 
      packages makes set-up and maintenance easier. 

	.. NOTE:: I
	   The command-line is really useful for managing remote servers, 
	   so this documentation will use apt_ . 
	   
	   If you aren't comfortable with the command-line feel free to 
	   substitute synaptic_. 

 * **Install Paths**: On Ubuntu, ``/usr/local`` is a good place to put
    custom software packages. If you are installing RapidSMS to deploy
    or develop an application, ``/usr/local`` is a good spot. 
    
    If you are installing to develop the platform itself, 
    ``~/home/dev`` is probably a better spot. 

    .. NOTE::
       If you choose to install any of the non-mainline forks,
       it's a good idea to name your install with the fork name
       so that you can easily remember where it came from.
       E.g.: For example, if you use the 'ewheeler' fork, 
       put RapidSMS_ in /usr/local/ewheeler-rapidsms. 

2 Install Ubuntu Jaunty
-----------------------
Install *either* Jaunty Desktop or Jaunty Server. 

The advantage of Desktop is that you get a full graphical desktop to work with. 

The advantage of Server is that you *don't* get a full desktop, which can discourage people from mucking with your server. Server is a little quicker to install as it has fewer packages.

If you don't know how to get and install Ubuntu_, visit the `Get Ubuntu`_ website for instructions on downloading or ordering a CD.

3 Update Jaunty
---------------
The install images and CDs for Ubuntu are always a little out of date. Before installing, it's a good idea to update the system. This requires an Internet connection and can take some time if there are a lot of changes::

    > sudo apt-get update
    > sudo apt-get upgrade


4 Install required and useful packages
--------------------------------------
The following packages are required to complete the install:

* **git-core**: the 'git' command line for retrieving code from GitHub_
* **python-django**: the Django application server (the basis of RapidSMS_)
* **python-serial**: Python serial libraries needed to run PyGSM_
* **python-tz**: Python timezone libraries

You will also need at least one of the following database systems:

* **mysql-server**: The MySQL server
* **python-mysqldb**: Python library access to MySQL

OR

* **python-pysqlite2**: Support for Sqlite3 in Python 
    (I know, the numbering is confusing, but pysqlite2 *is* for Sqlite3) 
    .. NOTE::
       Currently SQLite should be used for development and
       testing only. The multi-process nature of RapidSMS_
       does not interact stably enough with SQLite for use
       in production.
       
The following packages are OPTIONAL but useful to have, though you can leave them out if you want to create a minimal system and avoid downloading any more packages than you absolutely need:

* **picocom**: a program for talking to modems over serial ports. 
    *Very* useful for debugging modem/handset connection problems.
* **sqlite3**: a command line program for accessing sqlite3 databases. 
    If you use Sqlite3 as the datastore for RapidSMS, 
    you will want this for debugging.
* **sqlite3-doc**: documentation for sqlite3 tool.
* **emacs22-nox**: the Emacs text editor. 
    .. NOTE:: 
    This is *big*. If you are happy with Vi or Nano (installed by default), skip this.

This apt command will install *all* the packages listed above::

    > sudo apt-get install git-core python-pysqlite2 mysql-server python-mysqldb python-django picocom sqlite3 sqlite3-doc emacs22-nox

6 Retrieve RapidSMS from GitHub_
--------------------------------
The source code for RapidSMS_ is stored at GitHub_. You use the 'git' command to retrieve it.

Choosing the correct Fork
+++++++++++++++++++++++++
The most confusing part of downloading RapidSMS is decide *which version* to download! 
With all the development happening right now there are more than **10** versions of RapidSMS. In GitHub_ terminology, each version is called a *fork*

You can view all the `RapidSMS Forks here`_rapidsms_forks

The ''main'' fork is '''unicefinnovation / rapidsms''', but this fork is often not the newest.

Currently I am using the '''ewheeler / rapidsms''' fork.

'''IMPORTANT''': If you don't know which fork to use, please ask for help on the [http://groups.google.com/group/rapidsms  RapidSMS email group]

Once you have picked your fork, you can download the software with a command in the form:
{{{
> sudo git clone git://github.com/<fork name>/rapidsms.git <local folder name>
}}}

Where you ''replace'' <fork name> with your fork and <local folder name> with a name for the folder that the content will go into. To download the ewheeler fork, I do the following:
{{{
> cd /usr/local
> sudo git clone git://github.com/ewheeler/rapidsms.git ewheeler-rapidsms
}}}

== 6. Compile and install RapidSMS ==

'''NOTE''': If you named your rapidsms directory differently than I did (maybe you used a different fork) you need to change my example command below to 'cd' into the folder that holds the RapidSMS code that you retrieved in step 6 above.
 
{{{
> cd /usr/local/ewheeler-rapidsms
> sudo python setup.py install
}}}

== 7. Test your install ==

=== Test Spomsky ===
Try running Spomsky with the following command:
{{{
> sudo spomskyd
}}}

If it is working, you should see output like:
{{{
init Started HTTP Offline Backend
     URI: http://localhost:1270/
init Started SPOMSKYd Application
     URI: http://localhost:8100/
}}}

=== Test RapidSMS ===
The following commands create a test project (remember to replace ewheeler-rapidsms with the folder that has your RapidSMS source code in it from step 5 above):

{{{
> mdkir ~/rapidsms-projects
> cd ~/rapidsms-projects
> rapidsms startproject test-project
> cd ~/rapidsms-projects/test-project
> cp -a /usr/local/ewheeler-rapidsms/apps/* apps/
> chmod a+x ./manage.py
> ./manage.py syncdb
> ./manage.py route &
> ./manage.py runserver &
}}}

Now open a browser and connect to:
{{{
http://localhost:8000
}}}

You should see a RapidSMS dashboard.

'''NOTE''': If you do ''not'' have 'manage.py' in your test-project folder after running 'rapidsms startproject test-project', this means your rapidsms fork has a ''bug'' in it!. To fix this bug run the following commands, then erase your 'test-project' directory, and recreate it with the commands above. Remember to change 'ewheeler-rapidsms' to whatever folder has your RapidSMS source in it from step 5.

{{{
> sudo cp /usr/local/ewheeler-rapidsms/lib/rapidsms/skeleton/project/manage.py /usr/local/lib/python2.6/dist-packages/rapidsms/skeleton/project/
}}}