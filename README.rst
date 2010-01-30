====================================================
Authentication agains Google Apps Domains for Django
====================================================

*googleappsauth* allows you to authenticate your `Django <http://www.djangoproject.com/>`_  users against an Google `Apps <http://www.google.com/apps/>`_ domain.
This means you basically get a single sign-on solution, provided that all users of your django application
also have Accounts in Google Apps for your Domain.


Usage
=====

To use googleappsauth, configuration in `settings.py` should look like this::

    GOOGLE_APPS_DOMAIN = 'example.com'
    GOOGLE_APPS_CONSUMER_KEY = 'example.com'
    GOOGLE_APPS_CONSUMER_SECRET = '*sekret*'
    GOOGLE_OPENID_ENDPOINT = 'https://www.google.com/a/%s/o8/ud?be=o8' % GOOGLE_APPS_DOMAIN
    GOOGLE_API_SCOPE = 'http://www.google.com/m8/feeds/+http://docs.google.com/feeds/+http://spreadsheets.google.com/feeds/'
    # domain where your application is running
    GOOGLE_OPENID_REALM = 'http://*.hudora.biz/'

You also have to tell googleappsauth where various views life::

    LOGIN_URL = '/login'
    LOGIN_REDIRECT_URL = '/admin'
    LOGOUT_URL = '/logout'

To activate googleappsauth, set the appropriate Authentication backend and include a callback view.
::

    settings.py:
    AUTHENTICATION_BACKENDS = ('googleappsauth.backends.GoogleAuthBackend',)
    
    urls.py:
    (r'^callback_googleappsauth/', 'googleappsauth.views.callback'),


Using a special middleware which is included int he package, you can block access to a compete site.
::

    MIDDLEWARE_CLASSES = (
        'django.middleware.common.CommonMiddleware',
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'googleappsauth.middleware.GoogleAuthMiddleware',
    )

In addition you can set `AUTH_PROTECTED_AREAS` to authenticate only access to certain parts of a site, e.g.
::

    AUTH_PROTECTED_AREAS = '/admin'