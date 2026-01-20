How to Use the RequestAnalyzer with ESI Requests?
===============================================

The `Symfony documentation`_ describes how to use `edge side includes`_
to cache sections of pages with different lifetimes. However, if you use
the ``render_esi`` function in combination with the ``controller`` function as
shown in the following code, you may encounter issues:

.. code-block:: jinja

    {{ render_esi(controller('AppBundle:News:latest', { 'maxPerPage': 5 })) }}

While this works for most controllers, it will fail if the ``latestAction`` of
the ``NewsController`` utilizes the ``RequestAnalyzer``. This is
because the ``RequestAnalyzer`` cannot analyze the specialized URL that Symfony generates for the ``render_esi`` call.

The solution is to pass the portal and the locale (if the
rendered content should have a different locale than the rest of the page) to
the options:

.. code-block:: jinja

    {{ render_esi(controller('AppBundle:News:latest', { 'maxPerPage': 5, _portal: request.portalKey, _locale: request.locale })) }}

.. _Symfony documentation: https://symfony.com/doc/current/book/http_cache.html#using-edge-side-includes
.. _edge side includes: https://en.wikipedia.org/wiki/Edge_Side_Includes

