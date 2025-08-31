``sulu_snippet_load``
=====================

Returns a content array for a given snippet UUID.

.. code-block:: jinja

    {% set snippet = sulu_snippet_load('1234-1234-1234-1234-1234') %}
    {{ snippet.content.title }}

**Arguments**:

- **uuid**: *string* - The UUID of the requested content.
- **locale**: *string* - An optional locale to load the snippet.

**Returns**:

.. include:: _snippet_structure.inc
