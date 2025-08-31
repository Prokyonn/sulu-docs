Collection Selection
====================

Description
-----------

Lets you assign multiple collections from the media section.

.. note::

    This content type passes an array of Collection_ entities to the Twig template. It does not provide the media
    entities inside the selected collections.
    If you want to access the media entities of a collection, you should use a :doc:`smart_content property <smart_content>`
    with the ``media`` data provider, load the matching media entities in a  :doc:`custom controller <../../cookbook/custom-controller>`,
    or in a `custom twig extension`_.

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
    * - sortable
      - bool
      - Defines if a user can sort the selected items. The default value is `true`.
    * - request_parameters
      - collection
      - A collection of parameters that are appended to the requests sent by the selection.
    * - resource_store_properties_to_request
      - collection
      - A collection of property names whose values are appended to the requests sent by the selection.

Return Value
------------

See the Collection_ class for available variables and functions.

Example
-------

.. code-block:: xml

    <property name="collections" type="collection_selection">
        <meta>
            <title lang="en">Collections</title>
        </meta>
    </property>

Twig
----

.. code-block:: twig

    {% for collection in content.collections %}
        <h3>{{ collection.title }}</h3>
    {% endfor %}

.. _Collection: https://github.com/sulu/sulu/blob/2.x/src/Sulu/Bundle/MediaBundle/Api/Collection.php
.. _custom twig extension: https://symfony.com/doc/current/templating/twig_extension.html
.. _jexl: https://github.com/TomFrost/jexl
