Link
======

Description
-----------

The link content type allows you to select different types of links, including links to internal entities such as
pages and external URLs.
The content type can be limited to specific link types and configured to show ``anchor`` and
``target`` fields.

The link content type can be extended with additional link types by :doc:`implementing a custom LinkProvider service<../../cookbook/link-provider>`.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - enable_anchor
      - bool
      - Enables the ``anchor`` input field in the overlay. Default: ``false``
    * - enable_attributes
      - bool
      - Enables the ``target``, ``title``, and ``rel`` input fields in the overlay. Default: ``false``
    * - enable_query
      - bool
      - Enables the ``query`` input field in the overlay. Default: ``false``
    * - types
      - collection
      - List of available types in the dropdown menu.
        Default: All registered link types
    * - excluded_types
      - collection
      - List of types that should be excluded from the dropdown menu.
        Default: empty array

Example
-------

.. code-block:: xml

    <property name="link" type="link">
        <meta>
            <title lang="en">Link</title>
            <title lang="de">Link</title>
        </meta>
    </property>

Twig
----

.. code-block:: twig

    <a href="{{ content.link }}">Click me</a>

Complex Example
---------------

.. code-block:: xml

    <property name="link" type="link">
        <meta>
            <title lang="en">Link</title>
            <title lang="de">Link</title>
        </meta>

        <params>
            <param name="enable_anchor" value="true"/>
            <param name="enable_attributes" value="true"/>
            <param name="types" type="collection">
                <param name="page"/>
                <param name="external"/>
                <param name="media"/>
            </param>
        </params>
    </property>

Complex Twig
------------

.. code-block:: twig

    <a href="{{ content.link }}" target="{{ view.link.target }}" rel="{{ view.link.rel }}">
        {{ view.link.title }}
    </a>
