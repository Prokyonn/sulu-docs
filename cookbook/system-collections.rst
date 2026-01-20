System Collections
==================

System collections are special collections that are not editable, deletable, or
movable. Otherwise, they can be used like all other collections.

The system manages the creation and upgrading of these collections. Bundles can request
them to save images such as avatars or logos in the contact section.

Because of the configuration tree, the application itself can also register
system collections and use them.

.. code-block:: yaml

    sulu_media:
        system_collections:
            # system collection
            my_key:
                meta_title:
                    en: 'My Collection'
                    de: 'Meine Sammlung'

                # optional you can also configure sub collections
                collections:
                    my_child_key:
                        meta_title:
                            en: 'Child Collection'
                            de: 'Kindsammlung'

This structure creates a collection structure like this:

.. code-block:: bash

    System
     |--> My Collection
     |     |--> Child Collection

To register a system collection in a bundle, use the ``PrependExtensionInterface``
of Symfony to prepend the corresponding configuration:

.. code-block:: php

    <?php

    class ClientWebsiteExtension extends Extension implements PrependExtensionInterface
    {
        /**
         * {@inheritdoc}
         */
        public function prepend(ContainerBuilder $container)
        {
            if ($container->hasExtension('sulu_media')) {
                $container->prependExtensionConfig(
                    'sulu_media',
                    [
                        'system_collections' => [
                            'my_key' => [
                                'meta_title' => [
                                    'en' => 'Bundle Collection',
                                    'de' => 'Bundle Sammlung',
                                ],
                                'collections' => [
                                    'my_child' => [
                                        'meta_title' => [
                                            'en' => 'Child Collection',
                                            'de' => 'Kindsammlung',
                                        ],
                                    ],
                                ],
                            ],
                        ],
                    ]
                );
            }
        }

        /**
         * {@inheritdoc}
         */
        public function load(array $configs, ContainerBuilder $container)
        {
            // ...
        }
    }

To use the new collection, use the ``sulu_media.system_collections.manager``
(``Sulu\Component\Media\SystemCollections\SystemCollectionManagerInterface``) service.
The service creates the new collection upon the first access.

.. code-block:: php

    <?php

    // to get id of system collection
    $systemCollectionManager->getSystemCollection('my_key');

    // to get id of a child system collection
    $systemCollectionManager->getSystemCollection('my_key.my_child_key');

    // to determine if id is a system collection (e.g. validation)
    $systemCollectionManager->isSystemCollection(1);

.. note::

    The key for a sub-collection is a combination of the parent key and the child key, such as ``parent_key.child_key``
    (e.g., ``my_key.my_child_key``).
