Glossary
========

A glossary is a list of terms in a particular domain of knowledge with their definitions.

This page lists terminology used within Sulu as a reference and as a naming guide for the codebase.

Component
    Of a Structure—a named set of Properties.

Locale
    Represents a linguistic region, for example ``de``, ``de_at``, ``en`` or ``en_us``.

Localized
    Of a Property—the state of being localized or capable of being translated.

Metadata
    Data about data. Typically, a data structure with information such as field mappings that should be applied to a different data structure. In the context of Sulu, this applies to Structures, Properties, and Documents.

Non-localized
    Of a Property—the state of not being localized or not capable of being translated.

Path
    Always refers to the path of an object within the content repository, for example ``/cmf/sulu_io/contents/animals/dog``.

Page
    A page is a basic type of document. Pages are accessible directly via URLs and represent pages of your website.

Parameter
    In relation to Property and Structure items; a configuration parameter related to the configuration of the content type.

Prefix
    The first part of a web-facing URL defined by the portal, followed by the resource locator. The prefix may include the locale.

Property
    Refers to the items in a Structure.
    
Content Type
    The way Sulu represents different types of "content". For example, ``email``, ``text``, and ``smart_content`` are three examples of Sulu content types.

Resource locator
    The latter part of a web-facing URL belonging to a document, excluding the host and prefix segment. For example, ``/articles/foo`` is a resource locator, while ``/de/articles/foo`` and ``http://example.com/articles/foo`` are not. The resource locator never includes the locale.

Segment
    As applied to URLs and paths—a section of a path or URL, typically delimited by ``/``.

Shadow
    Of a document. A localized document can specify that it should be loaded in a different locale. The target locale is called the "shadow" locale.

Workflow Stage
    The stage of the workflow, for example, "published" and "test" are stages.

Snippet
    Snippets are like pages but are not accessible directly via URLs. Snippets are typically aggregated within pages.

Structure
    Structures represent dynamic content in Sulu. A structure is a collection of Properties.

Structure type
    The name of a given structure, e.g., ``overview``, ``hotel``, or ``article``.

Webspace
    In Sulu, a webspace encapsulates all the data of one or more domains using the same dataset.

Webspace Document
    The document at the root of the webspace tree—the homepage.
