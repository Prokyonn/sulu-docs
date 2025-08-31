URL
===

Description
-----------

Shows a text line. The inserted content will be validated against a URL regex
and saved as a simple string.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - defaults
      - collection
      - Default values for the input (`scheme` and `specificPart`).
    * - schemes
      - collection
      - A list of available schemes in the dropdown and for validation.
        Defaults are `https://`, `http://`, `ftp://`, `ftps`, `mailto:`, and `tel:`.

Example
-------

.. code-block:: xml

    <property name="url" type="url">
        <meta>
            <title lang="en">URL</title>
        </meta>
    </property>

Extended Example
----------------

.. code-block:: xml

    <property name="url" type="url">
        <meta>
            <title lang="en">URL</title>
        </meta>

        <params>
            <param name="defaults" type="collection">
                <param name="scheme" value="http://"/>
                <param name="specific_part" value="www.google.at"/>
            </param>

            <param name="schemes" type="collection">
                <param name="http://"/>
                <param name="https://"/>
            </param>
        </params>
    </property>

Twig
----

The content type returns the full URL, which can be rendered directly:

.. code-block:: twig

    <a href="{{ content.url }}">
        {{ content.url }}
    </a>
