Reference Store
===============

The reference store is a service that collects IDs of entities/documents
which are used to render a page. These IDs are used, for example, in the
caching component :doc:`../http_cache`.

Architecture
------------

Each content type registers its own service implementing the
``ReferenceStoreInterface`` or with the default implementation
``Sulu\Bundle\PageBundle\ReferenceStore\ReferenceStore``.

The ``sulu_website.reference_store_pool`` service collects services with the
tag ``sulu_website.reference_store`` and uses the ``alias`` attribute to
identify them.

To register a loaded entity, use the concrete store (e.g.
``sulu_page.reference_store.content`` or your own service) and call the
``add`` method to append the entity ID.

Example
-------

.. code-block:: xml

    <service id="app.reference_store.example"
             class="Sulu\Bundle\WebsiteBundle\ReferenceStore\ReferenceStore">
        <tag name="sulu_website.reference_store" alias="example"/>
    </service>

.. code-block:: php

    $exampleReferenceStore = $container->get('app.reference_store.example');
    $exampleReferenceStore->add(1);

    $referenceStore = $container->get('sulu_website.reference_store');
    var_dump($referenceStore->getStore('example')->getAll());

    // prints
    // array(1) {
    //   [0] =>
    //   int(1)
    // }
