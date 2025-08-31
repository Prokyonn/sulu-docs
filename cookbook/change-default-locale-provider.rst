How to Change the Default Locale Provider
=========================================

Perhaps we should first explain the purpose of a ``DefaultLocaleProvider``. A ``DefaultLocaleProvider`` is used
to determine the locale of a request if the request does not already contain the information. E.g., if you open
http://sulu.io, Sulu doesn't know which localization should be displayed because an English and a German version
of the homepage are available. In this case, the ``DefaultLocaleProvider`` is called to provide a default locale,
which is used to redirect to http://sulu.io/en.

Currently, two providers are available. One makes use of the portal's default localization configuration. The other
tries to find the best-matching locale based on the preferred language of the HTTP request.

You can provide your own ``DefaultLocaleProvider``, which has to implement the ``DefaultLocaleProviderInterface``.

Available default locale providers:

+---------------------------------------------------+-------------------------------------------------------+
| Service ID                                        | Description                                           |
+===================================================+=======================================================+
| `sulu_website.default_locale.portal_provider`     | Use the portal's default localization configuration   |
+---------------------------------------------------+-------------------------------------------------------+
| `sulu_website.default_locale.request_provider`    | Use the preferred language of the HTTP request        |
+---------------------------------------------------+-------------------------------------------------------+

Configuration
-------------

Change the default locale provider service ID to the provider that fulfills your needs.

.. code-block:: yaml

    sulu_website:
        default_locale:
            provider_service_id: sulu_website.default_locale.request_provider
