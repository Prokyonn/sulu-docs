How to change the default locale provider?
==========================================

The ``DefaultLocaleProvider`` is used to determine the locale of a request if the request does not already contain that information. For example, if you open
http://sulu.io, Sulu doesn't know which localization should be displayed because English and German versions
of the homepage are available. In this case, the ``DefaultLocaleProvider`` is called to provide a default locale,
which is used to redirect to http://sulu.io/en.

Currently, two providers are available. One utilizes the portal's default localization configuration. The other
attempts to find the best matching locale based on the preferred language of the HTTP request.

You can provide your own ``DefaultLocaleProvider``, which must implement the ``DefaultLocaleProviderInterface``.

Available default locale providers:

+---------------------------------------------------+-------------------------------------------------------+
| Service ID                                        | Description                                           |
+===================================================+=======================================================+
| `sulu_website.default_locale.portal_provider`     | Use portal default localization configuration         |
+---------------------------------------------------+-------------------------------------------------------+
| `sulu_website.default_locale.request_provider`    | Use preferred language of the HTTP request            |
+---------------------------------------------------+-------------------------------------------------------+

Configuration
-------------

Set the default locale provider service ID to the provider that meets your needs.

.. code-block:: yaml

    sulu_website:
        default_locale:
            provider_service_id: sulu_website.default_locale.request_provider
