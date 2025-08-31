Account Selection
========================

Description
-----------

Lets you assign multiple accounts from the account section to the page.

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
    * - min
      - string
      - The minimum number of selected accounts.
    * - max
      - string
      - The maximum number of selected accounts.

Return Value
------------

See the Account_ for available variables and functions.

Example
-------

.. code-block:: xml

    <property name="accounts" type="account_selection">
        <meta>
            <title lang="en">Accounts</title>
        </meta>
    </property>

Twig
----

.. code-block:: twig

    {% for account in content.accounts %}
        <h3>{{ account.name }}</h3>
    {% endfor %}

.. _Account: https://github.com/sulu/sulu/blob/2.x/src/Sulu/Bundle/ContactBundle/Api/Account.php
.. _jexl: https://github.com/TomFrost/jexl
