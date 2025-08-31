Single Contact Selection
========================

Description
-----------

Lets you assign one contact from the contacts section to the page.

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

Return Value
------------

See the ContactInterface_ for available variables and functions.

Example
-------

.. code-block:: xml

    <property name="contact" type="single_contact_selection">
        <meta>
            <title lang="en">Contact</title>
        </meta>
    </property>

Twig
----

You need to use the :doc:`../twig-extensions/functions/sulu_resolve_media` if you want to render
the contact's avatar image.

.. code-block:: twig

    {% set contact = content.contact %}
    {{ contact.fullName }}

    {% if contact.avatar %}
        {% set image = sulu_resolve_media(contact.avatar, app.request.locale) %}

        <img src="{{ image.thumbnails['80x80'] }}" alt="{{ contact.fullName }}">
    {% endif %}

.. _ContactInterface: https://github.com/sulu/sulu/blob/2.x/src/Sulu/Bundle/ContactBundle/Entity/ContactInterface.php
.. _jexl: https://github.com/TomFrost/jexl
