Backwards Compatibility Promise
===============================

Sulu is stable software used in production. However, it is still under heavy
development, and therefore full backwards compatibility cannot be guaranteed
at this stage.

We do our best to maintain backwards compatibility for the most commonly used extension
points and services in Sulu. These are listed in this document.

The promises given in this document are only valid within a single major
release. When a new major version is released, these promises may be broken.

PHP
---

Twig
~~~~

The most important extension point is Twig, as its templates are used in every
Sulu project. We guarantee that the variables
passed to Twig templates, as described in
:doc:`../book/twig`, will maintain their
structure and that all Twig extensions described in
:doc:`../reference/twig-extensions/index` will continue to function using the same
calls.

Configuration
~~~~~~~~~~~~~

Several configuration files define Sulu's behavior, and we promise backwards
compatibility for the following:

* Webspace (see :doc:`../book/webspaces`)
* Template (see :doc:`../book/templates`)
* Image formats (see :doc:`../book/image-formats`)
* MassiveSearch (see the `MassiveSearchBundle Mapping`_)
* Bundle configurations

Events
~~~~~~

Using events to extend Sulu is common, and we maintain backwards
compatibility here. While new data may be added to events, existing
data will not be removed. Event names are also guaranteed not to change.

Sulu Admin
~~~~~~~~~~

Using the ``Admin`` class along with its navigation is guaranteed not to
break.

Property Types
~~~~~~~~~~~~~~

We ensure that the property types included with Sulu save
content in a consistent way to prevent regressions during upgrades.

If the content structure must be changed to fix bugs, we will provide
migrations.

Sulu Classes and Interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following classes and interfaces are guaranteed to maintain backwards
compatibility:

* ``DocumentManagerInterface``
* ``WebsiteController``
* ``RequestAnalyzerInterface``
* ``SecurityCheckerInterface``
* ``User``
* ``Contact``
* ``Category``
* ``Tag``

.. _MassiveSearchBundle Mapping: http://massivesearchbundle.readthedocs.org/en/latest/mapping.html
