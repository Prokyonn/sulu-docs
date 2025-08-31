What Components are Packed into Sulu?
=====================================

The standard installation of Sulu comes with a set of components (called
"modules") required for the content management process:


Contacts
--------

The Contact module has two main purposes: The first (and in many cases, the most
important) is managing the users that have access to the administration
back-end of the website. The second is organizing the user data collected
through the website (e.g., newsletter registrations, etc.). In more complex
environments, this module can also be used to manage community members,
online-shop customers, or other contact-based data.


Assets
------

The Assets module lets you upload and organize any type of document, such as
pictures, text documents (PDF, Word, Excel, etc.), videos, or audio clips. Once
uploaded, an asset can be used on as many webpages as required, remaining as a
single source in the Assets module. This means that if you would like to change a
document that is used on several different webpages, you would only have to
replace it once in Assets. Pictures will be automatically transformed to
web-compatible formats and resized to the required formats of the templates,
while the original file will also be stored. All other document types
remain in their original format.


Webspaces
---------

A webspace is the place where the actual website structure and content will be
created. Within a webspace, a single content structure will be defined, but by
using, e.g., multiple languages and sub-domains, an unlimited number of websites
that share the same structure may be available. Furthermore, an unlimited number
of webspaces can be managed in one Sulu installation.

Confused? Maybe this example will help:

ACME Inc. has a website, www.acme.com, that needs to be published in English,
German, and French. The easy way to do this is to let the user choose their
desired language and stay on the same domain, displaying the required content
using sub-domains, such as, e.g., www.acme.com/de. For the user or a search
engine, this would mean one website with three languages sharing the same content
structure.

Next, let's assume that ACME Inc. wants to dedicate each language to its
corresponding market by using top-level domains. This would, of course, be more
marketing-oriented and search-engine-friendly. The English content would be
published on www.acme.com, the German content on www.acme.de, and the French
content on www.acme.fr. Let's go even further and say that each website's design
should be a little different, maybe with a different header color. The user and
the search engine would now have three separate websites, each with one language
and an individual design, but all with the same content structure.

Any of these scenarios can be implemented with Sulu using a single webspace.


Settings
--------

As the title implies, this module gives you access to all of Sulu's internal
adjustments. One very important section is "Permissions", where you can create
user roles with access rights, which can then be applied to a user in the
Contacts module. This gives you complete control over the access rights of your
website administrators. In addition, you can manage meta-information, such as
categories or tags, that are used in other modules.

Now that you know all the components of Sulu, we'll take a closer look at one
of the paradigms we have committed ourselves to: Separation of concerns.
