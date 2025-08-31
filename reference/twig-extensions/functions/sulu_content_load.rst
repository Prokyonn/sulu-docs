``sulu_content_load``
=====================

Returns a `Structure` for the given UUID.

.. code-block:: jinja

    {% set page = sulu_content_load('1234-1234-1234-1234', {
        'title': 'title',
        'excerptTitle': 'excerpt.title',
        'url': 'url',
    }) %}

**Arguments**:

- **uuid**: *string* - The UUID of the structure.
- **properties**: *array* - An array of properties of the structure that should be loaded.

**Returns**:

.. include:: _page_structure.inc

.. note::

    Calling the `sulu_content_load` Twig extension without the `properties` argument
    loads and resolves all properties of the target. This is an expensive operation that has
    a negative impact on performance and is therefore deprecated.
