# The Open Graph Protocol

The Open [Graph protocol](https://ogp.me/) enables any web page to become a rich object in a social graph. For instance, this is used on Facebook to allow any web page to have the same functionality as any other object on Facebook.

**Basic data**

To turn your web pages into graph objects, you need to add basic metadata to your page. We've based the initial version of the protocol on [RDFa](https://en.wikipedia.org/wiki/RDFa) which means that you'll place additional `<meta>` tags in the `<head>` of your web page. The five required properties for every page are:


- og:title - The title of your object as it should appear within the graph, e.g, "Ttattà Go"
- og:description - A one to two sentece description of your object, e.g, "Virtual assistant for tourism"
- og:type - The type of your object, e.g, "website". Depending on the type you specify, other properties may also be required.
- og:url - The canonical URL of your object that will be used as pemanent ID in the graph, e.g., "https://www.ttattago.com"
- og:image - An image URL which should represent your object within the graph.



**Example**

```html
...
<head>
  <meta property="og:title" content="Ttattà Go"/>
  <meta property="og:type"  content="website"/>
  <meta property="og:description" content="Virtual assistant for tourism" />
  <meta property="og:url"   content="https//www.ttattago.com" />
  <meta property="og:image" content="ttattago.png" />
</head>
...

```



