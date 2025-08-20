Provider for XML Sitemap
========================

`SitemapProvider`s are used to load data for the XML sitemap. They return
an array of `SitemapUrl` instances. This API has to be paginated because
Google only allows 50,000 URLs in a single sitemap. The `SitemapController`
takes care of generating a `sitemapindex` if more than one provider or
more than one page of results is available. Otherwise, it will deliver the sitemap
of the first provider.

The `SitemapUrl` consists of the following properties:

* `loc` - The URL to the page.
* `locale` - The locale of the page.
* `defaultLocale` - The default locale of the page.
* `lastmod` (optional) - The latest modification datetime.
* `changefreq` (optional) - The frequency of change (see the
  `SitemapUrl::CHANGE_FREQUENCY_*` constants).
* `priority` (optional) - The priority of the page in relation to other pages.
* `alternateLinks` (optional) - Alternate links, such as other representations
  or translations.

The Sulu core provides a single provider for pages (including the homepage).
Custom modules can provide their own providers so that these URLs will also
be published via the `sitemap.xml`.

Example
-------

This is a simple example that assumes that the logic to load entities is
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
                // If the pages are only for a specific host
                return 0;
            }

            return ceil($this->repository->countForSitemap() / self::PAGE_SIZE);
        }
    }

If you are not using autowiring, you need to tag the service with `sulu.sitemap.provider`.
