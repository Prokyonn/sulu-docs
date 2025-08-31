System-Collections
==================

System-Collections are special collections that are not editable, deletable, or
movable. Apart from that, they can be used like all other collections.

The System takes care of creating and upgrading them. Each bundle can request
them to save their images, like avatars or logos, in the contact section.

Because of the usage of the configuration tree, the App itself can also register
a system collection and use it.

.. code-block:: yaml

    sulu_media:
        system_collections:
            # system collection
            my_key:
                meta_title:
                    en: 'My Collection'
                    de: 'Meine Sammlung'

                # optional: you can also configure sub-collections
                collections:
                    my_child_key:
                        meta_title:
                            en: 'Child Collection'
                            de: 'Kindsammlung'

This structure will be used to create a Collection Structure like this:

.. code-block:: bash

    System
     |--> My Collection
     |     |--> Child Collection

If you want to register a system collection in a bundle, you can use the `PrependExtensionInterface`
of Symfony to prepend the respective configuration:

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
            ...
        }
    }

To use this new Collection, you can use the `sulu_media.system_collections.manager`
(`Sulu\Component\Media\SystemCollections\SystemCollectionManagerInterface`) service.
The service will create the new collection on the first access of the collection.

.. code-block:: php

    <?php

    // to get the ID of a system collection
    $systemCollectionManager->getSystemCollection('my_key');

    // to get the ID of a child system collection
    $systemCollectionManager->getSystemCollection('my_key.my_child_key');

    // to determine if an ID is a system collection (e.g., validation)
    $systemCollectionManager->isSystemCollection(1);

.. note::

    The key of a sub-collection is a combination with the parent key, so it's `parent_key.child_key`,
    e.g., `my_key.my_child_key`.
