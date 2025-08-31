About the Sulu Content Architecture
===================================

We have already learned something about the :doc:`introduction/content-architecture`
in the introduction.
Now that we are starting to code, we will dig a little bit deeper.


Sulu uses `PHPCR`_ as a persistence layer and therefore follows its structure.
Additionally, Sulu adds another layer called webspaces, which has already been
explained in the section about :doc:`introduction/components`. These
webspaces contain an arbitrary number of pages, which are ordered hierarchically
in a tree. Each of these pages can contain content in many different
localizations.

This tree also represents the actual structure of the website, so no
additional navigation tree is required. Pages can be enabled in the navigation
and will then appear in the correct spot on the website's navigation.

The pages in Sulu have a specific template applied. The template defines which
properties the page will have, and each of these properties is further
specified by a content type. The content type has a direct impact on the
possible values and configuration possibilities of the property to which it is
applied. There is also a reference of all the available
:doc:`../reference/content-types/index`.

There are also some advanced features for pages in Sulu. Besides
content management using the properties and content types already described,
there is also the possibility to define internal and external links. Internal
links redirect to other pages managed by the content management section of
Sulu, and external links redirect to an arbitrary URL.

Another useful feature is the shadow page functionality. It allows for the use
of content from another localization. So if a webspace defines localizations
for American and British English, it is possible to use the content of the
American English for the British English without managing the exact same content
again. This is especially useful if there are, for example, different contact
addresses for each country, but the rest of the page should be exactly the same.

.. _PHPCR: http://phpcr.github.io/

With the content architecture in mind, we can proceed to :doc:`webspaces`.
