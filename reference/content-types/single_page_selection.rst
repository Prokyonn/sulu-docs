Single page selection
=====================

Description
-----------

Displays a field where exactly one link to another page can be assigned.

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

    <property name="link" type="single_page_selection">
        <meta>
            <title lang="en">Link</title>
        </meta>
    </property>

Twig
----

The content type returns only the UUID of the target page. To
render a link to the page, use the :doc:`sulu-link tag<../../bundles/markup/link>`:

.. code-block:: html

    <sulu-link href="{{ content.link }}">Link Text</sulu-link>

If you need to load additional data from the target page, use the
:doc:`sulu_content_load Twig extension<../twig-extensions/functions/sulu_content_load>`:

.. code-block:: twig

    {% set target = sulu_content_load(content.link, {'title': 'title', 'excerptTitle': 'excerpt.title'}) %}

.. _jexl: https://github.com/TomFrost/jexl
