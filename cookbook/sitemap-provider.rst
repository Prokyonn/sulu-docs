Provider for XML Sitemap
========================

``SitemapProviders`` are used to load data for the XML sitemap. They return
an array of ``SitemapUrl`` instances. This API must be paginated because
Google only allows 50,000 URLs in a single sitemap. The ``SitemapController``
automatically generates a ``sitemapindex`` if multiple providers or
multiple pages are available. Otherwise, it delivers the sitemap
from the first provider.

A ``SitemapUrl`` consists of the following properties:

* ``loc`` - URL to the page.
* ``locale`` - Locale of the page.
* ``defaultLocale`` - Default locale of the page.
* ``lastmod`` (optional) - Latest modification datetime.
* ``changefreq`` (optional) - Frequency of change (see
  ``SitemapUrl::CHANGE_FREQUENCY_*`` constants).
* ``priority`` (optional) - Priority of the page relative to other pages.
* ``alternateLinks`` (optional) - Alternate links, such as other representations
  or translations.

The Sulu core provides a single provider for pages (including the homepage).
Custom modules can provide their own providers so that their URLs are also
published via the ``sitemap.xml``.

Example
-------

This simple example assumes that the logic to load entities is
implemented in the repository.

.. code-block:: php

    <?php

    namespace AppBundle\Sitemap;

    use AppBundle\Entity\ExampleRepository;
    use Sulu\Bundle\WebsiteBundle\Sitemap\Sitemap;
    use Sulu\Bundle\WebsiteBundle\Sitemap\SitemapProviderInterface;
    use Sulu\Bundle\WebsiteBundle\Sitemap\SitemapUrl;

    class SitemapProvider implements SitemapProviderInterface
    {
        public function __construct(private ExampleRepository $repository)
        {
        }

        public function build($page, $scheme, $host)
        {
            $result = [];
            foreach ($this->repository->findAllForSitemap($page, self::PAGE_SIZE) as $item) {
                $result[] = new SitemapUrl(
                    $scheme . '://' . $host . $item->getUrl(),
                    $item->getLocale(),
                    $item->getDefaultLocale(),
                    $item->getChanged()
                );
            }

            return $result;
        }

        public function getAlias()
        {
            return 'myalias';
        }

        public function createSitemap($scheme, $host)
        {
            return new Sitemap($this->getAlias(), $this->getMaxPage($scheme, $host));
        }

        public function getMaxPage($scheme, $host)
        {
            if ($host !== 'example.org') {
                return 0;
            }

            return ceil($this->repository->countForSitemap() / self::PAGE_SIZE);
        }
    }

If you are not using autowiring, you must tag the service with ``sulu.sitemap.provider``.
