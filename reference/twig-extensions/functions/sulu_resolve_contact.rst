``sulu_resolve_contact``
========================

Returns a contact entity.

.. code-block:: jinja

    {{ sulu_resolve_contact(contactId).fullName }}

**Arguments**:

- **id**: *int* - The ID of the requested contact.

**Returns**: `Contact` - An object with all the needed properties.
