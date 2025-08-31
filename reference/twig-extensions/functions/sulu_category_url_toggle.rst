.. _sulu_category_url_toggle:

``sulu_category_url_toggle``
============================

Returns the current URL and adds the given category as a GET parameter if it's not already there,
or removes the given category from the GET parameters if it is.

**Arguments**:

- **category**: *array* - A serialized `Category` instance to determine the value.
- **categoryParameter**: *string* - The optional `category` parameter name.

**Returns**: *string* - The current URL with or without the given category in the `categories` parameter.

**See also**:

- :ref:`sulu_category_url`
- :ref:`sulu_category_url_append`
- :ref:`sulu_category_url_remove`
- :ref:`sulu_category_url_clear`
