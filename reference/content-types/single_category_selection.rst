Single Category Selection
=========================

Description
-----------

Lets you assign one category. Categories can be managed in the Settings section of Sulu.
The selection will be saved as a single ID.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - item_disabled_condition
      - string
      - Allows setting a `jexl`_ expression that evaluates whether an item should be displayed as disabled.
        Disabled items cannot be selected.
    * - allow_deselect_for_disabled_items
      - bool
      - Defines if a user can deselect a disabled item. The default value is `true`.
    * - request_parameters
      - collection
      - A collection of parameters that are appended to the requests sent by the selection.
    * - resource_store_properties_to_request
      - collection
      - A collection of property names whose values are appended to the requests sent by the selection.

Example
-------

.. code-block:: xml

    <property name="category" type="single_category_selection">
        <meta>
            <title lang="en">Single Category Selection</title>
        </meta>
    </property>

Extended Example
----------------

The following example defines an entry category for the selection tree.

.. code-block:: xml

    <property name="category" type="single_category_selection">
        <meta>
            <title lang="en">Single Category Selection</title>
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

    {{ content.category.name }}

.. _jexl: https://github.com/TomFrost/jexl
