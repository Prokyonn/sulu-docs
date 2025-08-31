Single Page Selection
=====================

Description
-----------

Shows a field to which exactly one link to another page can be assigned.

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

    <property name="link" type="single_page_selection">
        <meta>
            <title lang="en">Link</title>
        </meta>
    </property>

Twig
----

At the moment, the content type only returns the UUID of the target page. If you want to
render a link to the page, you can use the :doc:`sulu-link tag<../../bundles/markup/link>`:

.. code-block:: html

    <sulu-link href="{{ content.link }}">Link Text</sulu-link>

If you need to load additional data from the target page, you can use the
:doc:`sulu_content_load Twig extension<../twig-extensions/functions/sulu_content_load>`:

.. code-block:: twig

    {% set target = sulu_content_load(content.link, {'title': 'title', 'excerptTitle': 'excerpt.title'}) %}

.. _jexl: https://github.com/TomFrost/jexl
