Single Snippet Selection
========================

Description
-----------

Allows you to select a single snippet. Snippets are reusable pieces of content that can be included on multiple pages.

Currently, this content type does not support multiple areas and types.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``types``
      - string
      - If set, only snippets of this type can be selected.
    * - ``default``
      - string
      - If set, the default snippet of the given area will be used as a fallback if no snippet is selected.
    * - ``loadExcerpt``
      - boolean
      - If set to ``true``, the taxonomy information of the snippet is loaded into a ``taxonomies`` property.
    * - ``item_disabled_condition``
      - string
      - Allows you to set a `jexl`_ expression that evaluates whether an item should be displayed as disabled.
        Disabled items cannot be selected.
    * - ``allow_deselect_for_disabled_items``
      - bool
      - Defines whether the user can deselect an item that is disabled. Default value is ``true``.
    * - ``request_parameters``
      - collection
      - Collection of parameters that are appended to the requests sent by the selection.
    * - ``resource_store_properties_to_request``
      - collection
      - Collection of property names.
        The values of the respective properties are appended to the requests sent by the selection.

Example
-------

.. code-block:: xml

    <property name="snippet" type="single_snippet_selection">
        <meta>
            <title lang="en">Snippet</title>
        </meta>

        <params>
            <param name="types" value="default"/>
            <param name="default" value="footer_social_media_links"/>
            <param name="loadExcerpt" value="true"/>
        </params>
    </property>

Twig
----

.. code-block:: twig

    {{ content.snippet.title }}

.. _jexl: https://github.com/TomFrost/jexl
