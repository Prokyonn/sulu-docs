Page Selection
==============

Description
-----------

Shows a list with the possibility to add links to other pages managed in Sulu.
Additionally, it populates all the fields defined in the template configuration
to the HTML template. The content is stored as an array of references.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - properties
      - collection
      - Defines with which key the property of the linked page should be
        populated to the HTML template.
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
      - The minimum number of selected pages.
    * - max
      - string
      - The maximum number of selected pages.

Example
-------

.. code-block:: xml

    <property name="links" type="page_selection">
        <meta>
            <title lang="en">Links</title>
        </meta>

        <params>
            <param name="properties" type="collection">
                <param name="title" value="title"/>
                <param name="article" value="article"/>
                <param name="excerptTitle" value="excerpt.title"/>
                <param name="excerptDescription" value="excerpt.description"/>
            </param>
        </params>
    </property>

.. _jexl: https://github.com/TomFrost/jexl

Twig
----

.. code-block:: twig

    {% for page in content.pages %}
        <a href="{{ sulu_content_path(page.url) }}">
            <h3>
                {{ page.excerptTitle|default(page.title) }}
            </h3>

            {{ page.excerptDescription|default(page.article) }}
        </a>
    {% endfor %}
