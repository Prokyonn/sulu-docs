Single Account Selection
========================

Description
-----------

Lets you assign one account from the account section to the page.

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

See the Account_ for available variables and functions.

Example
-------

.. code-block:: xml

    <property name="account" type="single_account_selection">
        <meta>
            <title lang="en">Account</title>
        </meta>
    </property>

Twig
----

You need to use the :doc:`../twig-extensions/functions/sulu_resolve_media` if you want to render
the account's logo.

.. code-block:: twig

    {% set account = content.account %}
    {{ account.name }}

    {% if account.logo %}
        {% set image = sulu_resolve_media(account.logo, app.request.locale) %}

        <img src="{{ image.thumbnails['80x80'] }}" alt="{{ account.name }}">
    {% endif %}

.. _Account: https://github.com/sulu/sulu/blob/2.x/src/Sulu/Bundle/ContactBundle/Api/Account.php
.. _jexl: https://github.com/TomFrost/jexl
