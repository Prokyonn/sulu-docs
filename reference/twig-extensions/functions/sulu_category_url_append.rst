.. _sulu_category_url_append:

``sulu_category_url_append``
============================

Returns the current URL and appends the given category to the GET parameter.

**Arguments**:

- **category**: *array* - A serialized `Category` instance to determine the value.
- **categoryParameter**: *string* - The optional `category` parameter name.

**Returns**: *string* - The current URL with the given category in the `categories` parameter.

**See also**:

- :ref:`sulu_category_url`
- :ref:`sulu_category_url_remove`
- :ref:`sulu_category_url_toggle`
- :ref:`sulu_category_url_clear`
