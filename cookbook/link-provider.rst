Provider for a Custom Link Type
=============================

``LinkProvider`` services resolve data for different types of internal links.
These services are used in various parts of the system, including the ``Link`` property type
(see :doc:`../reference/property-types/link`), the internal link plugin for CKEditor, and
the ``<sulu-link>`` tag inside Twig templates (see :doc:`../bundles/markup/link`).

The ``LinkProvider::preload`` method resolves an array of ``LinkItem``
instances for given arguments. Each ``LinkItem`` contains the following properties:

* ``id``
* ``title``
* ``url``
* ``published``

Example
-------

To register a ``LinkProvider`` service for a custom link type, create a service implementing
the ``LinkProviderInterface`` and tag it with the respective resource key:
``<tag name="sulu.link.provider" alias="{resourceKey}"/>``

If entities of the new link type should be selectable via a list in the administration interface,
the ``LinkProvider::getConfiguration`` method must return the list configuration.

.. code-block:: php

    <?php

    namespace AppBundle\Link;

    use Sulu\Bundle\MarkupBundle\Markup\Link\LinkConfigurationBuilder;
    use Sulu\Bundle\MarkupBundle\Markup\Link\LinkItem;
    use Sulu\Bundle\MarkupBundle\Markup\Link\LinkProviderInterface;

    class LinkProvider implements LinkProviderInterface
    {
        /**
         * {@inheritdoc}
         */
        public function getConfiguration()
        {
            return LinkConfigurationBuilder::create()
                ->setTitle($this->translator->trans('sulu_page.pages', [], 'admin'))
                ->setResourceKey('...') // the resourceKey of the entity that should be loaded
                ->setListAdapter('column_list')
                ->setDisplayProperties(['title'])
                ->setOverlayTitle($this->translator->trans('sulu_page.single_selection_overlay_title', [], 'admin'))
                ->setEmptyText($this->translator->trans('sulu_page.no_page_selected', [], 'admin'))
                ->setIcon('su-document')
                ->getLinkConfiguration();
        }

        /**
         * {@inheritdoc}
         */
        public function preload(array $hrefs, $locale, $published = true)
        {
            if (0 === count($hrefs)) {
                return [];
            }

            $items = ...; // load items by ID
            foreach ($items as $item) {
                $result[] = new LinkItem(...); // create LinkItem for each item
            }

            return $result;
        }
    }

If entities of the new link type cannot be selected via a list, the ``LinkProvider::getConfiguration``
method of your service must return ``null``, and you need to register a custom overlay via
the ``linkTypeRegistry`` JavaScript service:

.. code-block:: javascript

    import linkTypeRegistry from 'sulu-admin-bundle/containers/Link/registries/linkTypeRegistry';

    linkTypeRegistry.add('custom_resource_key', CustomLinkTypeOverlay, translate('app.custom_translation_key'));
