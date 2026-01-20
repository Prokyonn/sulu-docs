How to deactivate the RequestAnalyzer?
======================================

The ``RequestAnalyzer`` has the important task of identifying which webspace and locale the current request is targeted. It also recognizes
if the current request is invalid based on established rules, such as when no 
webspace is available at the requested URL. In this case, the ``RequestAnalyzer``
throws an exception, making it easy to identify errors in your
webspace configuration.

However, this behavior might be undesirable for requests where you are 
aware that no webspace is available and you do not need one. For
these specific requests, the ``RequestAnalyzer`` can be disabled.

This is achieved using `request attributes from Symfony`_. Sulu scans these
attributes for a field called ``_requestAnalyzer`` and skips the call when this
attribute is set to false. The easiest way to achieve this is via the
routing configuration file:

.. code-block:: yaml

    sulu_example.route:
        path: /some-url
        defaults:
            _controller: SuluExampleBundle:Controller:index
            _requestAnalyzer: false

.. _request attributes from Symfony: http://symfony.com/doc/current/components/http_foundation/introduction.html#component-foundation-attributes

