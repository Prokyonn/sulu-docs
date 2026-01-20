Text line
=========

Description
-----------

Displays a simple text line. The inserted content is saved as a simple string.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``headline``
      - boolean
      - If ``true``, the height and font size of the text line are increased.
    * - ``soft_max_length``
      - string
      - Soft limit for the maximum number of characters. Displays a character counter (replaces ``max_characters``).
    * - ``max_segments``
      - string
      - Soft limit for the maximum number of segments. Displays a segment counter.
    * - ``segment_delimiter``
      - string
      - The delimiter used to split the value into segments (required to use ``max_segments``).
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

    <property name="title" type="text_line">
        <meta>
            <title lang="en">Title</title>
        </meta>
        <params>
            <param name="headline" value="true"/>
        </params>
    </property>

Twig
----

.. code-block:: twig

    {{ content.title }}
