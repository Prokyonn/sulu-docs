Text area
=========

Description
-----------

Displays a simple text area. The inserted content is saved as a simple string.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``soft_max_length``
      - string
      - Soft limit for the maximum number of characters. Displays a character counter (replaces ``max_characters``).
    * - ``min_length``
      - string
      - The minimum number of characters.
    * - ``max_length``
      - string
      - The maximum number of characters.
    * - ``pattern``
      - string
      - A regex pattern that must be met by the entered data (e.g., ``"^[a-zA-Z]*$"`` allows only letters).

Example
-------

.. code-block:: xml

    <property name="description" type="text_area">
        <meta>
            <title lang="en">Description</title>
        </meta>
    </property>

Twig
----

.. code-block:: twig

    {{ content.description }}
