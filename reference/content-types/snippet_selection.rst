Snippet Selection
=================

Description
-----------

Allows selecting an arbitrary number of snippets. Snippets are reusable pieces of content that can be included on
multiple pages. The assigned snippets will be saved as an array of references.

Currently, this content type does not support multiple areas and types.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - types
      - string
      - If set, only snippets of this type can be selected.
    * - default
      - string
      - If set, the default snippet of the given area will be used as a fallback value if no snippet is selected.
    * - loadExcerpt
      - boolean
      - If set to `true`, the taxonomies information of the snippet is loaded into a "taxonomies" property.
    * - item_disabled_condition
      - string
      - Allows setting a `jexl`_ expression that evaluates whether an item should be displayed as disabled.
        Disabled items cannot be selected.
    * - allow_deselect_for_disabled_items
      - bool
      - Defines if a user can deselect a disabled item. The default value is `true`.
    * - sortable
      - bool
      - Defines if a user can sort the selected items. The default value is `true`.
    * - request_parameters
      - collection
      - A collection of parameters that are appended to the requests sent by the selection.
    * - resource_store_properties_to_request
      - collection
      - A collection of property names whose values are appended to the requests sent by the selection.
    * - min
      - string
      - The minimum number of selected snippets.
    * - max
      - string
      - The maximum number of selected snippets.

Example
-------

.. code-block:: xml

    <property name="snippets" type="snippet_selection">
        <meta>
            <title lang="en">Snippets</title>
        </meta>

        <params>
            <param name="types" value="sidebar"/>
            <param name="default" value="footer_social_media_links"/>
            <param name="loadExcerpt" value="true"/>
        </params>
    </property>

Twig
----

.. code-block:: twig

    {% for snippet in content.snippets %}
        {{ snippet.title }}
    {% endfor %}

.. _jexl: https://github.com/TomFrost/jexl
