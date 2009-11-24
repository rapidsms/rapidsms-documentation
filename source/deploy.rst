===========================
RapidSMS Server Deployment
===========================

The internal Django Web server is not designed to be run in production. Any modern Web server can be set up to serve a Django application like RapidSMS. See also the Django deployment documentation.

http://docs.djangoproject.com/en/dev/howto/deployment/

The recommended way to configure RapidSMS in production is to use Apache with mod_wgsi.

http://docs.djangoproject.com/en/dev/howto/deployment/modwsgi/

WSGI File
=========

Here is an example wsgi file you could copy somewhere in your RapidSMS instance directory:
::

    import os
    import sys

    filedir = os.path.dirname(__file__) # this is in the rapidsms directory

    rootpath = filedir
    sys.path.append(os.path.join(rootpath))
    sys.path.append(os.path.join(rootpath,'apps'))
    sys.path.append(os.path.join(rootpath,'lib'))
    sys.path.append(os.path.join(rootpath,'contrib'))
    sys.path.append(os.path.join(rootpath,'lib','rapidsms'))
    sys.path.append(os.path.join(rootpath,'lib','rapidsms','webui'))

    os.environ['RAPIDSMS_INI'] = os.path.join(rootpath,'local.ini')
    os.environ['DJANGO_SETTINGS_MODULE'] = 'rapidsms.webui.settings'
    os.environ["RAPIDSMS_HOME"] = rootpath

    from rapidsms.webui import settings

    import django.core.handlers.wsgi
    application = django.core.handlers.wsgi.WSGIHandler()


Apache Configuration
====================

Then an Apache configuration file may look like this:::

    Alias /media/ /path/to/project/staticmedia/
    <Directory /path/to/project>
        Order deny,allow
        Allow from all
    </Directory>

    WSGIScriptAlias / /path/to/project/yourwsgifile.wsgi

The first Alias directive is used to tell Apache to directly serve static files (images, css, javascripts) without passing through the Django/RapidSMS infrastructure. This is much better on a performance point of view.
