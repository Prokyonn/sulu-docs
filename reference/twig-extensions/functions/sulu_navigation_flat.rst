``sulu_navigation_flat``
========================

Returns the navigation from the given page in a flat list data structure.

**Arguments**:

- **uuid**: *string* - The UUID for which the navigation should be loaded.
- **context**: *string* - An optional context to filter the navigation.
- **depth**: *integer* - An optional depth to load (1 - one level deep, 2 - two levels deep, etc.).
- **loadExcerpt**: *boolean* - Optionally load data from the excerpt tab.

**Returns**:

.. include:: _navigation_structure.inc
