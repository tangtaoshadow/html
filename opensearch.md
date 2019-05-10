The [**OpenSearch description format**](http://www.opensearch.org/Specifications/OpenSearch/1.1#OpenSearch_description_document) lets a website describe a search engine for itself, so that a browser or other client application can use that search engine. OpenSearch is supported by (at least) Firefox, Edge, Internet Explorer, Safari, and Chrome. (See [Reference Material](https://developer.mozilla.org/en-US/docs/Web/OpenSearch#Reference_Material) for links to other browsers&#39; documentation.)

Firefox also supports additional features not in the OpenSearch standard, such as search suggestions and the \&lt;SearchForm\&gt; element. This article focuses on creating OpenSearch-compatible search plugins that support these additional Firefox features.

OpenSearch description files can be advertised as described in [Autodiscovery of search plugins](https://developer.mozilla.org/en-US/docs/Web/OpenSearch#Autodiscovery_of_search_plugins), and can be installed programmatically as described in [Adding search engines from web pages](https://developer.mozilla.org/en/Adding_search_engines_from_web_pages).

## **OpenSearch description file** [**Section**](https://developer.mozilla.org/en-US/docs/Web/OpenSearch#OpenSearch_description_file)

The XML file that describes a search engine follows the basic template below. Sections in _[square brackets]_ should be customized for the specific plugin you&#39;re writing.

\&lt;OpenSearchDescription xmlns=&quot;http://a9.com/-/spec/opensearch/1.1/&quot;

                       xmlns:moz=&quot;http://www.mozilla.org/2006/browser/search/&quot;\&gt;

  \&lt;ShortName\&gt;[SNK]\&lt;/ShortName\&gt;

  \&lt;Description\&gt;[Search engine full name and summary]\&lt;/Description\&gt;

  \&lt;InputEncoding\&gt;[UTF-8]\&lt;/InputEncoding\&gt;

  \&lt;Image width=&quot;16&quot;height=&quot;16&quot;type=&quot;image/x-icon&quot;\&gt;[https://example.com/favicon.ico]\&lt;/Image\&gt;

  \&lt;Url type=&quot;text/html&quot;template=&quot;[searchURL]&quot;\&gt;

    \&lt;Param name=&quot;[key name]&quot;value=&quot;{searchTerms}&quot;/\&gt;

    \&lt;!-- other Params if you need them… --\&gt;

    \&lt;Param name=&quot;[other key name]&quot;value=&quot;[parameter value]&quot;/\&gt;

  \&lt;/Url\&gt;

  \&lt;Url type=&quot;application/x-suggestions+json&quot;template=&quot;[suggestionURL]&quot;/\&gt;

  \&lt;moz:SearchForm\&gt;[https://example.com/search]\&lt;/moz:SearchForm\&gt;

\&lt;/OpenSearchDescription\&gt;

**ShortName**

A short name for the search engine. It must be  **16 or fewer character** s of plain text, with no HTML or other markup.

**Description**

A brief description of the search engine. It must be  **1024 or fewer characters**  of plain text, with no HTML or other markup.

**InputEncoding**

The [character encoding](https://developer.mozilla.org/en-US/docs/Glossary/character_encoding) to use when submitting input to the search engine.

**Image**

URI of an icon for the search engine. When possible, include a 16×16 image of type image/x-icon (such as /favicon.ico) and a 64×64 image of type image/jpeg or image/png.

The URI may also use the [data:](https://developer.mozilla.org/en-US/docs/Web/HTTP/data_URIs)[ URI scheme](https://developer.mozilla.org/en-US/docs/Web/HTTP/data_URIs). (You can generate a data: URI from an icon file at [The](http://software.hixie.ch/utilities/cgi/data/data)[data:](http://software.hixie.ch/utilities/cgi/data/data)[URI kitchen](http://software.hixie.ch/utilities/cgi/data/data).)

\&lt;Image height=&quot;16&quot;width=&quot;16&quot;type=&quot;image/x-icon&quot;\&gt;https://example.com/favicon.ico\&lt;/Image\&gt;

  \&lt;!-- or --\&gt;

\&lt;Image height=&quot;16&quot;width=&quot;16&quot;\&gt;data:image/x-icon;base64,AAABAAEAEBAAA … DAAA=\&lt;/Image\&gt;

Firefox caches the icon as a [base64](https://en.wikipedia.org/wiki/Base64) data: URI (search plug-ins are stored in the [profile](https://developer.mozilla.org/en-US/docs/Mozilla/Profile_Manager)&#39;s searchplugins/ folder). http: and https: URLs are converted to data: URIs when this is done.

**Note:**  For icons loaded remotely (that is, from https:// URIs as opposed to data:URIs), Firefox will reject icons larger than  **10 kilobytes**.

 ![](data:image/*;base64,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)

**Url**

Describes the URL or URLs to use for the search. The template attribute indicates the base URL for the search query.

Firefox supports three URL types:

- type=&quot;text/html&quot; specifies the URL for the actual search query.
- type=&quot;application/x-suggestions+json&quot; specifies the URL for fetching search suggestions. In Firefox 63 onwards, type=&quot;application/json&quot; is accepted as an alias of this.
- type=&quot;application/x-moz-keywordsearch&quot; specifies the URL used when a keyword search is entered in the location bar. This is supported only in Firefox.

For these URL types, you can use {searchTerms} to substitute the search terms entered by the user in the search bar or location bar. Other supported dynamic search parameters are described in [OpenSearch 1.1 parameters](http://www.opensearch.org/Specifications/OpenSearch/1.1/Draft_3#OpenSearch_1.1_parameters).

For search suggestions, the application/x-suggestions+json URL template is used to fetch a suggestion list in [JSON](https://developer.mozilla.org/en-US/docs/Glossary/JSON) format. For details on how to implement search suggestion support on a server, see [Supporting search suggestions in search plugins](https://developer.mozilla.org/en/Supporting_search_suggestions_in_search_plugins).

**Param**

The parameters that must be passed in along with the search query as key/value pairs. When specifying values, you can use {searchTerms} to insert the search terms entered by the user in the search bar.

**moz:SearchForm**

The URL for the site&#39;s search page for which the plugin. This lets Firefox users visit the web site directly.

**Note:**  Since this element is Firefox-specific, and not part of the OpenSearch specification, we use the moz: XML namespace prefix in the example above to ensure that other user agents that don&#39;t support this element can safely ignore it.

## **Autodiscovery of search plugins** [**Section**](https://developer.mozilla.org/en-US/docs/Web/OpenSearch#Autodiscovery_of_search_plugins)

Web sites with search plugins can advertise them so Firefox users can easily install the plugins.

To support autodiscovery, add a \&lt;link\&gt; element for each plugin to the \&lt;head\&gt; of your web page:

\&lt;link rel=&quot;search&quot;

      type=&quot;application/opensearchdescription+xml&quot;

      title=&quot;searchTitle&quot;

      href=&quot;pluginURL&quot;\&gt;

Replace the bolded items as explained below:

**searchTitle**

The name of the search to perform, such as &quot;Search MDC&quot; or &quot;Yahoo! Search&quot;. This must match your plugin file&#39;s \&lt;ShortName\&gt;.

**pluginURL**

The URL to the XML search plugin, so the browser can download it.

If your site offers multiple search plugins, you can support autodiscovery for them all. For example:

\&lt;link rel=&quot;search&quot;type=&quot;application/opensearchdescription+xml&quot;

      title=&quot;MySite: By Author&quot;href=&quot;http://example.com/mysiteauthor.xml&quot;\&gt;

\&lt;link rel=&quot;search&quot;type=&quot;application/opensearchdescription+xml&quot;

      title=&quot;MySite: By Title&quot;href=&quot;http://example.com/mysitetitle.xml&quot;\&gt;

This way, your site can offer plugins to search by author, or by title.

## **Supporting automatic updates for OpenSearch plugins** [**Section**](https://developer.mozilla.org/en-US/docs/Web/OpenSearch#Supporting_automatic_updates_for_OpenSearch_plugins)

OpenSearch plugins can automatically update. To support this, include an extra Url element with type=&quot;application/opensearchdescription+xml&quot; and rel=&quot;self&quot;. The templateattribute should be the URL of the OpenSearch document to automatically update to.

For example:

\&lt;Url type=&quot;application/opensearchdescription+xml&quot;

     rel=&quot;self&quot;

     template=&quot;https://example.com/mysearchdescription.xml&quot;/\&gt;

**Note:**  At this time, [addons.mozilla.org](http://addons.mozilla.org/) (AMO) doesn&#39;t support automatic updating of OpenSearch plugins. If you want to put your search plugin on AMO, remove the auto-updating feature before submitting it.

## **Troubleshooting Tips** [**Section**](https://developer.mozilla.org/en-US/docs/Web/OpenSearch#Troubleshooting_Tips)

If there is a mistake in your Search Plugin XML, you could run into errors when adding a discovered plugin. If the error message isn&#39;t be helpful, the following tips could help you find the problem.

- Your server should serve OpenSearch plugins using Content-Type: application/opensearchdescription+xml.
- Be sure that your Search Plugin XML is well formed. You can check by loading the file directly into Firefox. Ampersands (&amp;) in the template URL must be escaped as &amp;amp;, and tags must be closed with a trailing slash or matching end tag.
- The xmlns attribute is important — without it you could get the error message &quot;Firefox could not download the search plugin&quot;.
- You  **must**  include a text/html URL — search plugins including only Atom or [RSS](https://developer.mozilla.org/en/RSS) URL types (which is valid, but Firefox doesn&#39;t support) will also generate the &quot;could not download the search plugin&quot; error.
- Remotely fetched favicons must not be larger than 10KB (see [bug 361923](https://bugzilla.mozilla.org/show_bug.cgi?id=361923)).

In addition, the search plugin service provides a logging mechanism that may be useful to plugin developers. Use about:config to set the pref &#39;browser.search.log&#39; to true. Then, logging information will appear in Firefox&#39;s [Error Console](https://developer.mozilla.org/en/Error_Console) (Tools 〉 Error Console) when search plugins are added.
