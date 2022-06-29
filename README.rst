Django Preferences
==================
**Django app allowing users to set app specific preferences through the admin interface.**

.. image:: https://travis-ci.org/praekelt/django-preferences.svg?branch=develop
    :target: https://travis-ci.org/praekelt/django-preferences

.. image:: https://coveralls.io/repos/github/praekelt/django-preferences/badge.svg?branch=develop
    :target: https://coveralls.io/github/praekelt/django-preferences?branch=develop

.. image:: https://badge.fury.io/py/django-preferences.svg
    :target: https://badge.fury.io/py/django-preferences

Note: This is a fork of the original django-preferences project at
`praekelt/django-preferences <https://github.com/praekelt/django-preferences>`_,
with the multi-site stuff removed due to not being needed and tbh, causing more
issues than justified it's use.

I have not uploaded to pypi, since this is not my project, however I may create
a new entry (under a new name with full credit to original authors) in the
future.

For the moment, an easy way is to just drag the entire `preferences` folder into
your own Django project and use it as if it was a Django app. Note you will need
to uninstall the original django-preferences app before you do this.

Provides singleton admin views for Preferences objects and a simple interface to
preference values. Singleton views ensure only one preference instance per site
is available for each ``Preferences`` class.

..


.. contents:: Contents
    :depth: 5

Requirements
------------

#. Python 2.7, 3.5-3.7

#. Django 1.11, 2.0, 2.1


Installation
------------

#. Install or add ``django-preferences`` to your Python path.

#. Add ``preferences`` to your ``INSTALLED APPS`` setting.


#. Optionally, add ``preferences.context_processors.preferences_cp`` to your template option settings. This will automatically add a ``preferences`` variable to your template context::

     TEMPLATES = [{
         ...
         'OPTIONS': {
             'context_processors': [
                 ...
                 'preferences.context_processors.preferences_cp',
             ],
         },
     }]

Usage
-----
To create preferences for your app create a Django ORM model as usual, with the model inheriting from ``preferences.models.Preferences``. Also specify ``preferences.models`` as your model's module::

    from django.db import models
    from preferences.models import Preferences

    class MyPreferences(Preferences):
        portal_contact_email = models.EmailField()

Admin classes are specified as per usual, except that they have to inherit from or be registered with ``preferences.admin.PreferencesAdmin``, i.e.::

    from django.contrib import admin

    from preferences.admin import PreferencesAdmin
    from <my_app>.models import MyPreferences

    admin.site.register(MyPreferences, PreferencesAdmin)

When your model is registered with admin it will show up under the *Preferences* app label in Django admin.

Preferences can be accessed in Python by importing the ``preferences`` module and traversing to your required preference in the form ``preferences.<ModelName>.<field>``, i.e.::

    from preferences import preferences

    portal_contact_email = preferences.MyPreferences.portal_contact_email


If you've specified the ``preferences.context_processors.preferences_cp`` as a `TEMPLATES <https://docs.djangoproject.com/en/1.11/topics/templates>`_ you can similarly access your preferences within templates through the ``preferences`` variable, i.e.::

    {{ preferences.MyPreferences.portal_contact_email }}
