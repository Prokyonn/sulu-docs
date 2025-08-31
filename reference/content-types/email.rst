Email
=====

Description
-----------

Shows a text line. The inserted content will be validated against an email regex
and saved as a simple string.

Parameters
----------

No parameters available.

Example
-------

.. code-block:: xml

    <property name="email" type="email">
        <meta>
            <title lang="en">E-Mail</title>
        </meta>
    </property>

Twig
----

.. code-block:: twig

    {{ content.email }}
