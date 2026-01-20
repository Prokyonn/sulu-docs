SnippetBundle
=============

The SnippetBundle implements snippets in Sulu.

What is a Snippet
-----------------

As the name suggests, a snippet is a small page fragment.
However, unlike blocks, for example, which would also fit this description, snippets are designed for reusability.
A snippet is a website section that is maintained centrally and reused anywhere.
A social media section is a good example.

This section might contain logos of social services like Facebook and a link to the profile on the service.
You could build this section in a page template, but you would have to maintain it on every page.

This is where snippets come into play.
A snippet could be configured to cover exactly this use case and you would only have to maintain the profiles once and reuse them anywhere.

Creating a Snippet Template
---------------------------

In this example, we will create a "Social Media" snippet.

.. figure:: ../img/snippet-social-media.png

Creating a snippet template is similar to creating a page template (see :doc:`../book/templates`).
Create an XML file in your `config/template/snippets/` folder, as shown in the following example:

.. note::

    The ``<key>`` and the XML filename must match.

.. code-block:: xml

    <?xml version="1.0" ?>
    <template xmlns="http://schemas.sulu.io/template/template"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://schemas.sulu.io/template/template http://schemas.sulu.io/template/template-1.0.xsd">

        <key>social_media</key>

        <meta>
            <title lang="en">Social Media</title>
            <title lang="de">Social Media</title>
        </meta>

        <properties>
            <property name="title" type="text_line" mandatory="true">
                <meta>
                    <title lang="en">Title</title>
                    <title lang="de">Titel</title>
                </meta>
                <tag name="sulu.node.name"/>
            </property>

            <property name="facebookImage" colspan="3" type="single_media_selection">
                <meta>
                    <title lang="en">Facebook Icon</title>
                    <title lang="de">Facebook Icon</title>
                </meta>
            </property>

            <property name="facebookLink" colspan="9" type="url">
                <meta>
                    <title lang="en">Facebook Link</title>
                    <title lang="de">Facebook Link</title>
                </meta>
                <params>
                    <param name="schemes" type="collection">
                        <param name="http://"/>
                        <param name="https://"/>
                    </param>
                </params>
            </property>

            <property name="twitterImage" colspan="3" type="single_media_selection">
                <meta>
                    <title lang="en">Twitter Icon</title>
                    <title lang="de">Twitter Icon</title>
                </meta>
            </property>

            <property name="twitterLink" colspan="9" type="url">
                <meta>
                    <title lang="en">Twitter Link</title>
                    <title lang="de">Twitter Link</title>
                </meta>
                <params>
                    <param name="schemes" type="collection">
                        <param name="http://"/>
                        <param name="https://"/>
                    </param>
                </params>
            </property>
        </properties>
    </template>

Properties
----------

Properties are the same as for Pages (see :doc:`../book/templates`).


Implement a Snippet in your Template
------------------------------------

Snippets are stored separately and are not accessible via a direct URL.

To use a snippet on a page, add the content type ":doc:`../reference/content-types/single_snippet_selection`" to link one snippet, or ":doc:`../reference/content-types/snippet_selection`" for multiple snippets.

.. figure:: ../img/social-media-snippet-selection.png

.. code-block:: xml

        <property name="footer_social_media" type="snippet_selection">
            <meta>
                <title lang="en">Footer Social Media</title>
            </meta>
            <params>
                <param name="default" value="social_media"/>
            </params>
        </property>


Load Snippets from a Subfolder
------------------------------
You can load snippet templates from custom folders by configuring `config/packages/sulu_admin.yaml` as follows:

.. code-block:: yaml

    sulu_core:
        content:
            structure:
                paths:
                    event_snippets:
                        path: "%kernel.project_dir%/config/template/events/snippets/"
                        type: "snippet"

In this example, a new Events folder is specified. The configuration key must be unique.


Learn more
----------

* :doc:`../cookbook/default-snippets`
* Content type reference for :doc:`../reference/content-types/single_snippet_selection`
* Content type reference for :doc:`../reference/content-types/snippet_selection`
