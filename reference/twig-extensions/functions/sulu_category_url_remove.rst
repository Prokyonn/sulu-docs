.. _sulu_category_url_remove:

``sulu_category_url_remove``
============================

Returns the current URL and removes the given category from the GET parameters.

**Arguments**:

- **category**: *array* - A serialized `Category` instance to determine the value.
- **categoryParameter**: *string* - The optional `category` parameter name.

**Returns**: *string* - The current URL without the given category in the `categories` parameter.

**See also**:

- :ref:`sulu_category_url`
- :ref:`sulu_category_url_append`
- :ref:`sulu_category_url_toggle`
- :ref:`sulu_category_url_clear`
