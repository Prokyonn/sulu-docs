Adding localizations
====================

Sulu is built for companies with an international focus, so translating pages
into multiple languages is a very important task for a content editor
using Sulu. Sulu also considers the different variations of a language among
different countries. The combination of these two factors is called a
localization.

Configuring webspace localizations
----------------------------------

Localizations for the content are configured in the webspaces, as already
described in :doc:`webspaces`. Adding another localization is as easy as
adding another ``localization`` tag to the webspace configuration file.
Localizations can also be nested, which has no impact on the representation in
any of the dropdowns, but it will help the system find better fallbacks.

So, a good example using English and German as languages might look something
like the following fragment.

.. code-block:: xml

    <localizations>
        <localization language="en">
            <localization language="en" country="us"/>
            <localization language="en" country="gb"/>
        </localization>
        <localization language="de">
            <localization language="de" country="de"/>
            <localization language="de" country="at"/>
            <localization language="de" country="ch"/>
        </localization>
    </localizations>

With this configuration, the system will contain seven different content
localizations: ``en``, ``en-us``, ``en-gb``, ``de``, ``de-de``, ``de-at``, and
``de-ch``, whereby ``en-us`` and ``en-gb`` fall back to ``en``, and
``de-de``, ``de-at``, and ``de-ch`` fall back to ``de``.

After adding localizations in the webspace, note that you need to run:

.. code-block:: bash

    php bin/adminconsole sulu:document:initialize

This will re-initialize the PHPCR content tree, setting up the new locale to
accept new content. Afterwards, nobody will have any permissions on this
locale, so make sure that you add this permission in the permissions tab of
the contacts.

.. note::

    When adding a new localization, you have to make sure that the localization
    is also covered by one of the webspace's URLs. This can happen either by using
    the `language` and `country` attributes of the `url` tag or by specifying,
    e.g., the `{localization}` placeholder. For more information, see
    `Setup a Webspace <webspaces.html#urls>`_.

Adding custom localizations
---------------------------

There is another possibility for adding non-webspace-related localizations.
More details can be found in :doc:`../cookbook/localization-provider`.

Usage of localizations
----------------------

For the developer, the only touching points with localizations are the
configuration and the eventual use of a language switcher on the homepage.
For the language switcher, the ``localizations`` variable delivered to the Twig template
can be used, which contains an associative array with the parameters ``locale``, ``url``, and ``country``.
The currently active locale can be obtained from the underlying Symfony Request
object with ``app.request.locale``.

.. code-block:: twig

    <ul>
    {% for localization in localizations %}
        <li><a href="{{ localization.url }}">{{ localization.locale }}</a>
    {% endfor %}
    </ul>

.. important::

    The ``localizations`` variable contains the page URLs based on the URLs configured in 
    the  ``<urls>`` section of your webspace. Have a look at :doc:`../book/webspaces` for
    more information about the webspace configuration.

The template itself does not have to be adapted for usage with multiple
localizations. The Twig variables are the same for every language; only the
content is different, and this is handled by Sulu for the developer.

The nesting of the localizations is very important for the column navigation of
the content. In case there is a ghost page - which means that the page is not
translated into the current localization - this tree will be used to determine
the "closest" available language.
