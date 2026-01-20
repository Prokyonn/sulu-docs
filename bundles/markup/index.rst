MarkupBundle
============

The MarkupBundle allows extending output formats using different so-called tags.
These tags are automatically parsed and replaced before the response is sent.

Example
-------

This example shows a Sulu-related tag. The tag ``sulu-link`` represents
a link to another page. This tag is replaced by a valid anchor where the
`href` attribute contains the UUID of the page.

.. code-block:: html

    <html>
        <body>
            <sulu-link href="123-123-123" title="test-title" />
        </body>
    </html>

**Result:**

.. code-block:: html

    <html>
        <body>
            <a href="http://example.org/test" title="test-title">Page Title</a>
        </body>
    </html>

Core Tags
---------

.. toctree::
    :maxdepth: 1

    link

Extending
---------

To enable replacement of your custom tags you can define a service that
implements the ``TagInterface``.

.. code-block:: php

    class LinkTag implements TagInterface
    {
        /**
         * Returns new tag with given attributes.
         *
         * @param array $attributes attributes array of each tag occurrence.
         *
         * @return array Tag array to replace all occurrences.
         */
        public function parseAll($attributesByTag): array
        {
            $result = [];
            foreach($attributesByTag as $tag => $attributes) {
                $url = ; // load url via uuid from document-manager
                $pageTitle = ...; // load page-title via uuid from document-manager

                $result[$tag] = sprintf('<a href="%s" title="%s">%s</a>', $url, $pageTitle, $attributes['content']);
            }

            return $result;
        }
    }

When registering your service, simply add the tag
``<tag name="sulu_markup.tag" tag="link"/>``.

Namespaces
----------

Namespaces are used to identify tags with specific behavior. The default
namespace is ``sulu``, but you can register your own namespace by adding a new
service and registering your ``TagInterface`` implementations with this new
namespace.

.. code-block:: xml

    <service id="app.html_extractor"
             class="Sulu\Bundle\MarkupBundle\Markup\HtmlTagExtractor">
        <argument type="string">custom-namespace</argument>

        <tag name="sulu_markup.parser.html_extractor"/>
    </service>

    <service id="app.tag" class="AppBundle\CustomTag">
        <tag name="sulu_markup.tag" namespace="custom-namespace"
             tag="custom-tag" type="html" />
    </service>

With these definitions you can use ``<custom-namespace-custom-tag/>`` in your
response.
