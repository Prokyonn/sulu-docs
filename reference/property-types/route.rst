Route
=====

Description
-----------

The ``route`` property type allows you to generate URLs for **custom entities**.
See :doc:`/bundles/route/index` to learn how to implement routing for your custom entity.

.. note::

    The ``route`` property type should not be used on page templates. For pages, use the :doc:`resource_locator`
    property type instead.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``mode``
      - string
      - Defines the mode of the input field. Can be "full" or "leaf". Default value is "full".
    * - ``entity_class``
      - string
      - Class used for loading the URL history of the entity.
        If not set, the :doc:`/bundles/route/index` mapping for the resource key of the form is used.
    * - ``route_schema``
      - string
      - Route schema used for generating the URL.
        If not set, the :doc:`/bundles/route/index` mapping for the resource key of the form is used.


Example
-------

.. code-block:: xml

    <property name="title" type="text_line">
        <tag name="sulu.rlp.part"/>
    </property>

    <property name="subtitle" type="text_line">
        <tag name="sulu.rlp.part"/>
    </property>

    <property name="routePath" type="route">
        <meta>
            <title lang="en">Resource locator</title>
        </meta>

        <params>
            <param name="mode" value="full"/>
            <param name="entity_class" value="App\Entity\Event"/>
            <param name="route_schema" value="/events/{implode('-', object)}"/>
        </params>
    </property>

Twig
----

You must use the :doc:`../twig-extensions/functions/sulu_content_path` Twig extension
to render the full URL.

.. code-block:: twig

    {{ sulu_content_path(content.routePath) }}
