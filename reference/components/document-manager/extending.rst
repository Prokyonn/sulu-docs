Extending the Document Manager
==============================

Where to Put Things?
--------------------

Any classes that relate to the documents or the Document Manager should, by
convention, be placed within a `Document` namespace.

Documents themselves should be placed directly under this namespace, and other
types of classes should be placed in sub-namespaces with appropriate names. For
example:

.. code-block:: bash

    src/Bundle/MyBundle/Document/FooDocument.php
    src/Bundle/MyBundle/Document/Initializer/FooInitializer.php
    src/Bundle/MyBundle/Document/Subscriber/FooSubscriber.php
