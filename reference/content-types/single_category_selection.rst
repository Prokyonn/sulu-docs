Single Category selection
=========================

Description
-----------

Allows you to assign one category. Categories can be managed in the settings section of Sulu.
The selection is saved as a single ID.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
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

    <property name="category" type="single_category_selection">
        <meta>
            <title lang="en">Single Category Selection</title>
        </meta>
    </property>

Extended Example
----------------

The following example defines a root category for the selection tree.

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
