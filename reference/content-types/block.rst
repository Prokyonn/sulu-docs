Block
=====

Description
-----------

The block content type allows you to group an arbitrary amount of other content
types. Each block can define multiple types with different content types
included. These blocks can then be repeated and ordered by the content manager
in the Sulu admin.

A common use case is to combine a text editor with a media selection.
This way, text can be directly linked to an image via the assignment to the
same block. This approach has its biggest benefit compared to putting images into the
text editor when used in combination with responsive design. When using
multiple content types in a block, the template developer has the freedom to
place the image where and in which format it makes sense. In contrast, adding
images to the text editor would make it difficult to adapt the format and
placement in the Twig template.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``add_button_text``
      -
      - Allows you to adjust the text of the add button displayed in the administration interface.
    * - ``paste_button_text``
      -
      - Allows you to adjust the text of the paste button displayed in the administration interface.
    * - ``collapsable``
      - boolean
      - If set to ``true``, blocks can be collapsed; otherwise, they are always expanded. Defaults to ``true``.
    * - ``movable``
      - boolean
      - If set to ``true``, the position of blocks can be changed using drag-and-drop; otherwise, they have a fixed position. Defaults to ``true``.
    * - ``settings_form_key``
      - string
      - Key of the form that should be opened in an overlay when the settings icon is clicked. Defaults to ``page_block_settings`` for pages.

Example
-------

Please note that the configuration of the block content type differs from
other content types.

Instead of a ``property`` tag, a ``block`` tag is used. The
``default-type`` attribute is mandatory and describes which of the types are
used by default.

The other essential attribute is the ``types`` tag, which contains multiple
``type`` tags. A type defines titles and its containing ``properties``,
whereby all available :doc:`index` can be used. These types are offered
to the content manager via a dropdown menu.

When collapsed, the system shows the content of three properties in the block
by default to give the content manager an idea of which block they are
viewing. The ``sulu.block_preview`` tag can be used to manually choose which
properties should be shown as a preview in collapsed blocks. These tags
additionally take a ``priority`` attribute, which can alter the order of the
property previews.

The example below shows a single type, combining a single media selection with
a text line and text editor. This is a common block type in Sulu for 
placing images between different text passages.

.. code-block:: xml

    <!-- A maxOccurs is also possible to limit the blocks for very special cases  -->
    <block name="blocks" default-type="text_image" minOccurs="0">
        <meta>
            <title lang="de">Inhalte</title>
            <title lang="en">Content</title>
        </meta>

        <!-- Params are optional, but specially for nested blocks `add_button_text` is recommended to be set -->
        <params>
            <param name="add_button_text">
                <meta>
                    <title lang="en">Add block</title>
                    <title lang="de">Block hinzufügen</title>
                </meta>
            </param>
        </params>

        <types>
            <type name="text_image">
                <meta>
                    <title lang="de">Editor mit Bild</title>
                    <title lang="en">Editor with image</title>
                </meta>

                <properties>
                    <property name="title" type="text_line">
                        <meta>
                            <title lang="de">Titel</title>
                            <title lang="en">Title</title>
                        </meta>

                        <!-- optional: you can control which fields the collapsed view prefer to show -->
                        <tag name="sulu.block_preview" priority="128"/>
                    </property>

                    <property name="description" type="text_editor">
                        <meta>
                            <title lang="de">Artikel</title>
                            <title lang="en">Article</title>
                        </meta>

                        <!-- optional: you can control which fields the collapsed view prefer to show -->
                        <tag name="sulu.block_preview" priority="64"/>
                    </property>

                    <property name="image" type="single_media_selection">
                        <meta>
                            <title lang="de">Bilder</title>
                            <title lang="en">Images</title>
                        </meta>

                        <params>
                            <param name="types" value="image"/>
                            <param name="defaultDisplayOption" value="bottom"/>
                            <param name="displayOptions" type="collection">
                                <param name="top" value="true"/>
                                <param name="left" value="true"/>
                                <param name="right" value="true"/>
                                <param name="bottom" value="true"/>
                            </param>
                        </params>

                        <!-- optional: you can control which fields the collapsed view prefer to show -->
                        <tag name="sulu.block_preview" priority="1024"/>
                    </property>
                </properties>
            </type>
        </types>
    </block>

.. note::

    If you want to use a block type in multiple templates, you can use global blocks. See
    :ref:`template properties <templates-global-blocks>` for more information.

Twig
----

A reusable way to render blocks is to have a separate template file per type:

.. code-block:: twig

    {% for block in content.blocks %}
        {% include 'includes/blocks/' ~ block.type ~ '.html.twig' with {
            content: block,
            view: view.blocks[loop.index0],
        } %}
    {% endfor %}

This allows you to access the ``properties`` of the block type via
the ``content`` and ``view`` variables in the rendered block template.

Extending Default Block Settings for Pages
------------------------------------------

If you want to add a custom field to the block settings for pages, you can extend
the ``page_block_settings`` form by creating ``config/forms/page_block_settings.xml``:

.. code-block:: xml

    <?xml version="1.0" ?>
    <form xmlns="http://schemas.sulu.io/template/template"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://schemas.sulu.io/template/template http://schemas.sulu.io/template/form-1.0.xsd"
    >
        <key>page_block_settings</key>

        <properties>
            <section name="custom">
                <properties>
                    <property name="theme" type="single_select">
                        <meta>
                            <title lang="en">Block Theme</title>
                            <title lang="de">Block Theme</title>
                        </meta>

                        <params>
                            <param name="default_value" value=""/>

                            <param name="values" type="collection">
                                <param name="">
                                    <meta>
                                        <title lang="en">Default</title>
                                        <title lang="de">Standard</title>
                                    </meta>
                                </param>

                                <param name="highlight">
                                    <meta>
                                        <title lang="en">Highlight</title>
                                        <title lang="de">Highlight</title>
                                    </meta>
                                </param>
                            </param>
                        </params>
                    </property>
                </properties>
            </section>
        </properties>
    </form>

The value of your field can be accessed in Twig via the ``settings`` variable:

.. code-block:: twig

    {% for block in content.blocks %}
        <div class="blocks__item{% if block.settings.theme|default %} block__item--{{ block.settings.theme }}{% endif %}">
            {# ... #}
        </div>
    {% endfor %}
