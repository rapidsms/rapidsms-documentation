.. _tutorial:

===============================
Writing your first RapidSMS app
===============================

One of the best ways to learn anything new is by learning by example.

In this tutorial, we'll be building a RapidSMS app to track vehicle mileage.

First, let's consider the problem: We are interested in building an application
capable of allowing a motorist to track his vehicle mileage.

The idea is that before starting a trip, the driver sends the mileage reading 
on his odometer to RapidSMS and on completing the trip, sends the mileage 
reading on the odometer again to RapidSMS.

RapidSMS in responding to the second message will report cool things like 
the total distance travelled and the average speed.

Here are certain things we will be keeping track of:

    1) The time the messages were sent
    2) The mileage readings
    3) The driver (we would need some way to identify the driver)

Getting RapidSMS
----------------

The best way to get RapidSMS is to clone it from its repository. In order to 
do this, you'll need to have Git installed on your machine. Please visit 
http://git-scm.com/ for more information on getting Git installed.

Assuming you've installed ``git``, the following steps will get RapidSMS 
setup on your machine::

    ~$ git clone git://github.com/rapidsms/rapidsms.git
    ~$ cd rapidsms
    ~rapidsms$


Creating the application stub
-----------------------------

RapidSMS comes with tools to make creating applications as easy as possible 
and one of such is to create the skeleton for your application so you can just 
write the logic where you need the processing to be done.

To create the application skeleton type the following::

    ~rapidsms$ python rapidsms startapp mileage
    Don't forget to add 'mileage' to your rapidsms.ini apps.
    ~rapidsms$

As you can see in the message, in order for the application to be of any use 
it has to be inserted in the list of applications. Applications are not 
enabled in RapidSMS unless they are included in the list of apps.

Make a copy of the rapidsms configuration file ``rapidsms.ini``::

    ~rapidsms$ cp rapidsms.ini local.ini

Next, use your favorite text editor and open the ``local.ini`` file.
Under the ``[rapidsms]`` section, add ``mileage`` at the end of ``apps`` 
directive.

This step has created a directory named ``mileage`` in your RapidSMS apps 
directory which is located at ``rapidsms/apps``. We will change directories 
into this so we can work with it.::

    ~rapidsms$ cd apps/mileage
    ~rapidsms/apps/mileage$

In this directory, are four files of importance:

    - ``app.py``
        This file is where your application logic is stored. We'll be 
        working more with this file.

    - ``models.py``
        Your Django models are defined in this file. We'll be using this 
        file to define the model structure for persisting data in the database

    - ``tests.py``
        One of the ideologies used in developing RapidSMS is the test-driven 
        design ideology. You store unit tests for your app in this file.

    - ``views.py``
        Django is at the core of RapidSMS and for apps that require a web 
        interface, the views logic is defined here.

Defining the database model
---------------------------

Syncing the database
--------------------

Writing the application logic
-----------------------------

Testing your application
------------------------
