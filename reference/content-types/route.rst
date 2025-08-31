Route
=====

Description
-----------

The `route` content type allows generating URLs for **custom entities**.
Have a look at :doc:`/bundles/route/index` to see how to implement routing for your custom entity.

.. note::

    The `route` content type should not be used on page templates. For pages, use the :doc:`resource_locator`
    content type instead.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - mode
      - string
      - Defines the mode of the input field. Can be either "full" or "leaf". The default value is "full".
    * - entity_class
      - string
      - The class that is used for loading the history URLs of the entity.
        If not set, the :doc:`/bundles/route/index` mapping for the resource key of the form is used.
    * - route_schema
      - string
      - The route schema that is used for generating the URL.
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

You need to use the :doc:`../twig-extensions/functions/sulu_content_path` Twig extension
to render the full URL.

.. code-block:: twig

    {{ sulu_content_path(content.routePath) }}
