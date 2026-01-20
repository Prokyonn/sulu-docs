Page Selection
==============

Description
-----------

Displays a list that allows you to add links to other pages managed in Sulu.
Additionally, it populates all fields defined in the template configuration
into the HTML template. The content is stored as an array of references.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``properties``
      - collection
      - Defines which properties of the linked page should be
        populated into the HTML template and under which key.
    * - ``item_disabled_condition``
      - string
      - Allows you to set a `jexl`_ expression that evaluates whether an item should be displayed as disabled.
        Disabled items cannot be selected.
    * - ``allow_deselect_for_disabled_items``
      - bool
      - Defines whether the user can deselect an item that is disabled. Default value is ``true``.
    * - ``sortable``
      - bool
      - Defines whether the user can sort the selected items. Default value is ``true``.
    * - ``request_parameters``
      - collection
      - Collection of parameters that are appended to the requests sent by the selection.
    * - ``resource_store_properties_to_request``
      - collection
      - Collection of property names.
        The values of the respective properties are appended to the requests sent by the selection.
    * - ``min``
      - string
      - The minimum number of selected pages.
    * - ``max``
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
