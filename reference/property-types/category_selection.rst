Category selection
==================

Description
-----------

Displays a list of all available categories. The user can select which ones to assign to the page using checkboxes. Categories can be managed in the settings
section of Sulu. The selection is saved as an array.

.. note::

    This property type is rarely needed because the ``Excerpt & Taxonomies`` tab
    allows you to assign categories to pages.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - item_disabled_condition
      - string
      - Allows you to set a `jexl`_ expression that evaluates whether an item should be displayed as disabled.
        Disabled items cannot be selected.
    * - request_parameters
      - collection
      - Collection of parameters that are appended to the requests sent by the selection.
    * - resource_store_properties_to_request
      - collection
      - Collection of property names.
        The values of the respective properties are appended to the requests sent by the selection.
    * - min
      - string
      - The minimum number of selected categories.
    * - max
      - string
      - The maximum number of selected categories.

Example
-------

.. code-block:: xml

    <property name="categories" type="category_selection">
        <meta>
            <title lang="en">Category Selection</title>
        </meta>
    </property>

Extended Example
----------------

The following example defines a root category for the selection tree.

.. code-block:: xml

    <property name="categories" type="category_selection">
        <meta>
            <title lang="en">Category Selection</title>
        </meta>
        <params>
            <param name="request_parameters" type="collection">
                <param name="rootKey" value="my_category_key"/>
            </param>
        </params>
    </property>

Twig
----

.. code-block:: twig

    {% for category in content.categories %}
        <h3>{{ category.name }}</h3>
    {% endfor %}

If you want to list all categories in your template, you can use the :doc:`../twig-extensions/functions/sulu_categories`
Twig extension.

.. _jexl: https://github.com/TomFrost/jexl
