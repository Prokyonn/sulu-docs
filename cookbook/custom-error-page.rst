Custom Error Page
=================

With Sulu, it is very easy to customize the error pages for your website's users.
You can define a template for each HTTP status code.

Configuration
-------------

The following code block from the webspace configuration file shows a default
configuration for the error templates. If you want to add your own template
for a 404 error, for example, you can simply add it to the list. You can specify this for
each theme.

.. code-block:: xml

    <templates>
        <template type="error">error/error</template>
        <template type="error-404">error/error-404</template>
    </templates>

The `ErrorController` uses the status code of the response to determine
which template is responsible for displaying the error. If no specific template is
defined, it uses the template without an error code.

Twig Template
-------------

In the Twig template, you can use your website's base template to reuse your
styles.

.. code-block:: html

	{% extends "base.html.twig" %}

	{% block title %}Error {{ status_code }}{% endblock %}

	{% block content %}
	    <h1>Error {{ status_code }}</h1>
	    <p>{{ status_text }}</p>

	    <p>{{ exception.message }}</p>
	{% endblock %}

.. warning::

    Be careful which variables you use in your `base.html.twig`. If you use variables
    that are not defined in the error template, the error page cannot be rendered.

The following variables are available inside the error template.

+---------------------------------+------------------------------------------------------------------+
| Name                            | Description                                                      |
+=================================+==================================================================+
| `status_code`                   | The HTTP status code.                                            |
+---------------------------------+------------------------------------------------------------------+
| `status_text`                   | The HTTP status text.                                            |
+---------------------------------+------------------------------------------------------------------+
| `exception`                     | The complete exception object.                                   |
+---------------------------------+------------------------------------------------------------------+
| `urls`                          | Localized URLs to the start page (e.g., for a language switcher).|
+---------------------------------+------------------------------------------------------------------+
| `request.webspaceKey`           | The key of the current webspace.                                 |
+---------------------------------+------------------------------------------------------------------+
| `request.defaultLocale`         | The default locale of the current portal.                        |
+---------------------------------+------------------------------------------------------------------+
| `request.locale`                | The current locale.                                              |
+---------------------------------+------------------------------------------------------------------+
| `request.portalUrl`             | The URL of the current portal.                                   |
+---------------------------------+------------------------------------------------------------------+
| `request.resourceLocatorPrefix` | The prefix for resource locators of the current portal.          |
+---------------------------------+------------------------------------------------------------------+
| `request.resourcelocator`       | The current resource locator.                                    |
+---------------------------------+------------------------------------------------------------------+
| `request.get`                   | An array of GET parameters.                                      |
+---------------------------------+------------------------------------------------------------------+
| `request.post`                  | An array of POST parameters.                                     |
+---------------------------------+------------------------------------------------------------------+
| `request.analyticsKey`          | The analytics key of the current webspace.                       |
+---------------------------------+------------------------------------------------------------------+

Testing
-------

To test your error pages, you can use the following URL pattern:

.. code-block:: bash

    {portal-prefix}/_error/{statusCode}

.. note::

    If you are not sure about your portal configuration, you can get the routes with the following command:
    `bin/websiteconsole debug:router | grep _error`

Examples:

.. code-block:: bash

    example.org/ch._twig_error_test       ANY    ANY    example.org /ch/_error/{code}.{_format}
    example.org/en._twig_error_test       ANY    ANY    example.org /en/_error/{code}.{_format}
    example.org/fr._twig_error_test       ANY    ANY    example.org /fr/_error/{code}.{_format}
    example.org/de._twig_error_test       ANY    ANY    example.org /de/_error/{code}.{_format}
    example.org._twig_error_test          ANY    ANY    example.org /_error/{code}.{_format}
