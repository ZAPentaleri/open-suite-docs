# N/xml
<!-- id: N/xml -->

`N/xml` gives scripts a DOM-style toolkit for XML: turn a string into a document tree, walk and
query it (directly or with XPath), change it, write it back out as a string, check it against an
XSD schema, and escape text so it is safe to drop into markup.

- **Availability:** client and server scripts, except [`xml.validate`](#xmlvalidateoptions), which
  runs only in server scripts.
- **Load it as:** `define(['N/xml'], (xml) => { ... })`
- **Since:** 2015.2 for every member.
- [`Document`](#document-object) and [`Element`](#element-object) both inherit the members of
  [`Node`](#node-object), so tree navigation (`firstChild`, `childNodes`, `appendChild` and so on)
  works on either.
- Only element nodes carry attributes. On any other node type, `Node.attributes` is `null`.
- Every method on this page is free: 0 governance units.

> [!SOURCE-ERROR] The source appears to have the return types of `Element.getAttributeNS` and
> `Element.getAttributeNodeNS` the wrong way round. The spec records `string` for both; by their
> purpose, the "node" variant should hand back an attribute node and the plain variant a value
> string. Check what you actually receive before relying on either type.
<!-- source-error: N/xml | - | getattributens_and_getattributenodens_return_types_swapped -->

## Members

| Member | Kind | Summary |
|---|---|---|
| [`xml.validate(options)`](#xmlvalidateoptions) | function | Check a document against an XSD from the File Cabinet (server only) |
| [`xml.escape(options)`](#xmlescapeoptions) | function | Escape `<`, `>`, quotes and `&` in a string |
| [`Parser`](#parser-object) | object | Parse a string into a document, and serialize a document to a string |
| [`XPath`](#xpath-object) | object | Run an XPath expression against a node |
| [`Node`](#node-object) | object | Base node of the DOM; shared by `Document` and `Element` |
| [`Document`](#document-object) | object | The whole parsed document; also a factory for new nodes |
| [`Element`](#element-object) | object | One element, with attribute methods |
| [`Attr`](#attr-object) | object | An attribute node belonging to an element |
| [`xml.NodeType`](#xmlnodetype) | enum | Kinds of DOM node |

## Functions

### xml.validate(options)
<!-- id: N/xml.validate -->

Checks an in-memory XML document against an XSD schema stored in the File Cabinet. XSD is the only
schema language it understands. If the document passes, the call simply returns; if it does not,
it throws. You must already hold the document as an `xml.Document` (for example from
[`Parser.fromString`](#parserfromstringoptions)), and the XSD must be a File Cabinet file.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.importFolderPathOrId` | number \| string | No | |
| `options.xml` | xml.Document | Yes | |
| `options.xsdFilePathOrId` | number \| string | Yes | |

- `options.importFolderPathOrId`: the folder (path or id) holding any XSD files that the main
  schema imports. Since 2015.2.
- `options.xml`: the parsed document to check. Since 2015.2.
- `options.xsdFilePathOrId`: the schema file, by File Cabinet path or internal id. Since 2015.2.

**Returns:** `void`. Success is signalled by the absence of an error.

**Errors:**

- `SSS_XML_DOES_NOT_CONFORM_TO_SCHEMA`: the document is not valid according to the schema.
- `SSS_INVALID_XML_SCHEMA_OR_DEPENDENCY`: the schema, or something it depends on, could not be
  found.
- `ILLEGAL_REQUEST_FOR_A_FILE_THAT_ISNT_DOWNLOADABLE`: the running script lacks permission to read
  the XSD file named in `options.xsdFilePathOrId`.

**Governance:** 0 units.

**Availability:** server scripts only.

**Since:** 2015.2

### xml.escape(options)
<!-- id: N/xml.escape -->

Makes a piece of text safe to embed in XML by replacing angle brackets, quote characters and
ampersands with their escaped forms. Use it whenever you build markup by string concatenation from
data you do not control.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.xmlText` | string | Yes | |

- `options.xmlText`: the text to escape. Since 2015.2.

**Returns:** `string`, the escaped text.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

## Objects

### Parser (object)
<!-- id: N/xml.Parser -->

Converts between XML text and `xml.Document` trees in both directions. You call its methods
directly as `xml.Parser.fromString(...)` and `xml.Parser.toString(...)`. Client and server; since
2015.2.

#### Parser.fromString(options)
<!-- id: N/xml.Parser#fromString -->

Parses XML text into a W3C-style document object, which you can then walk with DOM members or
query with XPath. Because malformed text throws, it doubles as a well-formedness check.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.text` | string | Yes | |

**Returns:** `xml.Document`, the parsed document.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the text is not well-formed XML.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Parser.toString(options)
<!-- id: N/xml.Parser#toString -->

Serializes a document back into XML text. A typical use is saving a built or modified document into
a custom field.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.document` | xml.Document | Yes | |

**Returns:** `string`, the serialized document.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### XPath (object)
<!-- id: N/xml.XPath -->

Evaluates XPath expressions. Call it as `xml.XPath.select(...)`. Client and server; since 2015.2.

#### XPath.select(options)
<!-- id: N/xml.XPath#select -->

Runs an XPath expression, starting from the node you pass, and collects the nodes it matches.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.node` | xml.Node | Yes | |
| `options.xpath` | string | Yes | |

- `options.node`: the context node the expression is evaluated from (often the document itself).
- `options.xpath`: the expression.

**Returns:** `xml.Node[]`, the matching nodes.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

### Node (object)
<!-- id: N/xml.Node -->

The base building block of the DOM. [`Document`](#document-object) and
[`Element`](#element-object) inherit everything listed here. Client and server; since 2015.2.

Several members below are flagged as unsupported in Internet Explorer when used from client
scripts; one (`isSameNode`) is also unsupported in Firefox there.

#### Node.insertBefore(options)
<!-- id: N/xml.Node#insertBefore -->

Inserts `newChild` immediately in front of the existing child `refChild`. If `newChild` is already
somewhere in the tree, it is moved rather than copied.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.refChild` | xml.Node | Yes | |
| `options.newChild` | xml.Node | Yes | |

- `options.refChild`: passing `null` places `newChild` at the end of the child list. The option is
  listed as required, so pass `null` explicitly rather than leaving it out.

> [!SOURCE-ERROR] The source's sentence describing the `refChild` condition is missing the value
> it is talking about. The spec reads it as "when `refChild` is null, the node is appended at the
> end"; confirm that behaviour before depending on it, especially since the parameter is also
> listed as required.
<!-- source-error: N/xml.Node#insertBefore | options.refChild | refchild_condition_value_missing_from_text -->

**Returns:** `xml.Node`, the inserted node.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the input is malformed, or this node type is not allowed to hold the new
  child at that position (a hierarchy violation).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.isEqualNode(options)
<!-- id: N/xml.Node#isEqualNode -->

Tests structural equality: two nodes are equal when they are of the same type, carry the same
attributes (order does not matter), and have equal child lists in the same order. They need not be
the same object; for identity use [`isSameNode`](#nodeissamenodeoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.other` | xml.Node | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.hasChildNodes()
<!-- id: N/xml.Node#hasChildNodes -->

Tells you whether the node has any children.

**Returns:** `boolean`, `true` when at least one child node exists.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.isSameNode(options)
<!-- id: N/xml.Node#isSameNode -->

Tests identity: `true` only when `other` is this very node object. Two nodes that are the same
object are necessarily also equal under [`isEqualNode`](#nodeisequalnodeoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.other` | xml.Node | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer
or Firefox.

**Since:** 2015.2

#### Node.removeChild(options)
<!-- id: N/xml.Node#removeChild -->

Detaches a child from this node.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.oldChild` | xml.Node | Yes | |

**Returns:** `xml.Node`, the node that was removed.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails, including when `oldChild` is not actually a child
  of this node (not-found case).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.lookupPrefix(options)
<!-- id: N/xml.Node#lookupPrefix -->

Finds the prefix bound to a namespace URI, as seen from this node. If more than one prefix is bound
to the URI, which one you get is up to the implementation.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |

**Returns:** `string`, the prefix, or `null` if none is bound.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Node.replaceChild(options)
<!-- id: N/xml.Node#replaceChild -->

Swaps one child (`oldChild`) for another (`newChild`). If `newChild` is already in the tree it is
first taken out of its current position.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.newChild` | xml.Node | Yes | |
| `options.oldChild` | xml.Node | Yes | |

**Returns:** `xml.Node`. The source gives only the type; it does not state whether the node handed
back is `oldChild` or `newChild`, so do not depend on either without testing.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: `oldChild` is not a child of this node (not found), or this node is not
  allowed to hold `newChild` (hierarchy violation).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.normalize()
<!-- id: N/xml.Node#normalize -->

Tidies the text beneath this node: adjacent text nodes are merged into one and empty text nodes are
dropped. Call it when you need the in-memory tree to look the same as it will after the document is
saved and parsed again.

**Returns:** `void`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.appendChild(options)
<!-- id: N/xml.Node#appendChild -->

Adds a node as the new last child of this node.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.newChild` | xml.Node | Yes | |

**Returns:** `xml.Node`, the node that was appended.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the input is malformed, or this node type is not allowed to contain the
  new child (hierarchy violation).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.hasAttributes()
<!-- id: N/xml.Node#hasAttributes -->

Tells you whether the node has any attributes defined.

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Node.compareDocumentPosition(options)
<!-- id: N/xml.Node#compareDocumentPosition -->

Reports where `other` sits relative to this node, as a bit mask. The bits are:

| Bit | Meaning |
|---|---|
| 1 | The two nodes are in different documents |
| 2 | `other` comes before this node |
| 4 | `other` comes after this node |
| 8 | `other` contains this node |
| 16 | This node contains `other` |
| 32 | No common container, or the nodes are attributes that cannot be ordered against each other |

Bits combine. For example, 20 (16 + 4) means `other` is inside this node and therefore also
follows it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.other` | xml.Node | Yes | |

**Returns:** `number`, the combined flags.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: `options.other` is of the wrong type, that is, not an `xml.Node`.

> [!SOURCE-ERROR] The source words the error condition as if it fires when `other` *is* an XML
> node, which inverts its meaning. The spec takes it to fire when `other` is not a node; pass a
> genuine `xml.Node` and treat the wording as unreliable.
<!-- source-error: N/xml.Node#compareDocumentPosition | - | error_condition_says_other_is_xml_node -->

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Node.isDefaultNamespace(options)
<!-- id: N/xml.Node#isDefaultNamespace -->

Checks whether a namespace URI is the default namespace in scope at this node.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Node.lookupNamespaceURI(options)
<!-- id: N/xml.Node#lookupNamespaceURI -->

The reverse of [`lookupPrefix`](#nodelookupprefixoptions): finds the namespace URI bound to a
prefix, as seen from this node.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.prefix` | string | Yes | |

**Returns:** `string`, the URI, or `null` when the prefix is unbound.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Node.cloneNode(options)
<!-- id: N/xml.Node#cloneNode -->

Makes a copy of this node.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | No | |
| `options.deep` | boolean | No | |

- `options.deep`: `true` copies the node together with its whole subtree; `false` copies only the
  node itself and its attributes. The source does not state which of the two happens when you omit
  it, so pass the value you want explicitly.

**Returns:** `xml.Node`, the copy.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Node.nodeValue
<!-- id: N/xml.Node#nodeValue -->

`string`. The node's value; what that means depends on the node type. If the value is defined as
`null` for this node type, assigning to it does nothing.

#### Node.nextSibling
<!-- id: N/xml.Node#nextSibling -->

`xml.Node`, read-only. The node right after this one under the same parent, or `null`.

#### Node.nodeType
<!-- id: N/xml.Node#nodeType -->

`xml.NodeType`. Which kind of node this is; compare against [`xml.NodeType`](#xmlnodetype).

#### Node.namespaceURI
<!-- id: N/xml.Node#namespaceURI -->

`string`, read-only. The node's namespace URI, or `null` if it has none.

#### Node.lastChild
<!-- id: N/xml.Node#lastChild -->

`xml.Node`. The last child, or `null` when there are no children.

#### Node.textContent
<!-- id: N/xml.Node#textContent -->

`string`. The text of this node and everything below it. Assigning a string throws away all
existing children and puts a single text node holding that string in their place.

#### Node.nodeName
<!-- id: N/xml.Node#nodeName -->

`string`, read-only. The node's name; its form depends on the node type. In Chrome, the name
includes the namespace prefix.

#### Node.ownerDocument
<!-- id: N/xml.Node#ownerDocument -->

`xml.Document`. The document this node belongs to. Handy when you hold only a node and need to
create new nodes for the same tree, through
[`createElement`](#documentcreateelementoptions) or
[`createElementNS`](#documentcreateelementnsoptions).

#### Node.baseURI
<!-- id: N/xml.Node#baseURI -->

`string`, read-only. The node's absolute base URI, or `null`. In client scripts it is always
`null`, and where a value is given its format varies by browser.

#### Node.childNodes
<!-- id: N/xml.Node#childNodes -->

`xml.Node[]`. All direct children; an empty array when there are none.

#### Node.prefix
<!-- id: N/xml.Node#prefix -->

`string`. The namespace prefix, or `null`. Where the prefix is defined as `null` for the node type,
assigning it has no effect. Setting an invalid prefix throws `SSS_XML_DOM_EXCEPTION` (a namespace
error).

#### Node.firstChild
<!-- id: N/xml.Node#firstChild -->

`xml.Node`. The first child, or `null` when there are no children.

#### Node.localName
<!-- id: N/xml.Node#localName -->

`string`, read-only. The local part of the node's qualified name (the part after any prefix).

#### Node.attributes
<!-- id: N/xml.Node#attributes -->

Read-only, typed `string`. The attributes of an element. For any node that is not an element this
is `null`.

> [!SOURCE-ERROR] The source types this property as `string`, yet describes it as a key/value
> collection of an element's attributes. A plain string cannot be that; inspect the value at run
> time, or use `getAttribute` / `getAttributeNode` on the element instead.
<!-- source-error: N/xml.Node#attributes | - | type_string_for_key_value_attribute_collection -->

#### Node.previousSibling
<!-- id: N/xml.Node#previousSibling -->

`xml.Node`. The node right before this one under the same parent, or `null`.

#### Node.parentNode
<!-- id: N/xml.Node#parentNode -->

`xml.Node`. The parent of this node. It does not apply to attribute, document, document fragment,
entity or notation nodes, which have no parent in the tree.

### Document (object)
<!-- id: N/xml.Document -->

Represents an entire XML document as a tree of nodes; its own node type is `DOCUMENT_NODE`. You get
one from [`Parser.fromString`](#parserfromstringoptions). Besides the inherited
[`Node`](#node-object) members, it is the factory for new nodes that belong to the document.
Client and server; since 2015.2.

#### Document.createAttributeNS(options)
<!-- id: N/xml.Document#createAttributeNS -->

Creates a new attribute node in a namespace.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.value` | string | No | |
| `options.qualifiedName` | string | Yes | |

- `options.namespaceURI`: may be `null`.
- `options.value`: the attribute value; an empty string when omitted.

The source says the new node's `localName`, `prefix` and `namespaceURI` are all `null`.

> [!SOURCE-ERROR] Saying that a node created *with* a namespace URI and qualified name has null
> namespace properties contradicts the purpose of this method; the text seems copied from the
> non-namespace variant. Read the properties of the created node rather than assuming they are null.
<!-- source-error: N/xml.Document#createAttributeNS | - | namespace_properties_said_to_be_null -->

**Returns:** `xml.Attr`, the new attribute node.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails, for example when the name contains a character
  that is illegal in XML.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Document.createElement(options)
<!-- id: N/xml.Document#createElement -->

Creates a new element with the given tag name. The new node's `localName`, `prefix` and
`namespaceURI` are `null`. It is not placed in the tree until you append or insert it.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tagName` | string | Yes | |

**Returns:** `xml.Element`, the new element.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails, for example when the tag name contains a character
  that is illegal in XML.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.adoptNode(options)
<!-- id: N/xml.Document#adoptNode -->

Moves a node (with its whole subtree) into this document: the owner document of the node and all
its descendants changes to this one, and the node is detached from its previous parent.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.source` | xml.Node | Yes | |

**Returns:** `xml.Node`, the adopted node.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the input is malformed, or the node cannot be adopted.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.createProcessingInstruction(options)
<!-- id: N/xml.Document#createProcessingInstruction -->

Creates a processing-instruction node, used to carry information meant for a specific processing
application.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.target` | string | Yes | |
| `options.data` | string | Yes | |

**Returns:** `xml.Node`, the new processing-instruction node.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.createAttribute(options)
<!-- id: N/xml.Document#createAttribute -->

Creates a new attribute node with no namespace (its `localName`, `prefix` and `namespaceURI` are
`null`). Attach it to an element with
[`Element.setAttributeNode`](#elementsetattributenodeoptions).

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.value` | string | No | |
| `options.name` | string | Yes | |

- `options.value`: an empty string when omitted.

**Returns:** `xml.Attr`, the new attribute.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails, for example when the name contains a character
  that is illegal in XML.

> [!SOURCE-ERROR] The source's text for this error condition contains a misspelling
> (`spcified` [sic]), and the spec marks the passage for human review. The error code itself is not
> in question; just do not rely on the exact wording of the condition.
<!-- source-error: N/xml.Document#createAttribute | - | error_condition_misspelled_spcified -->

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.importNode(options)
<!-- id: N/xml.Document#importNode -->

Copies a node from another document into this one. Unlike
[`adoptNode`](#documentadoptnodeoptions), the original stays where it was.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.deep` | boolean | Yes | |
| `options.importedNode` | xml.Node | Yes | |

- `options.deep`: `true` brings the children along too; `false` imports only the node and its
  attributes. In client scripts this option is not supported in Internet Explorer.

**Returns:** `xml.Node`, the imported copy.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the input is malformed, or the node type cannot be imported.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.createDocumentFragment()
<!-- id: N/xml.Document#createDocumentFragment -->

Creates an empty document-fragment node, a lightweight container for building a group of nodes
before inserting them.

**Returns:** `xml.Node`, the new fragment.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.getElementsByTagName(options)
<!-- id: N/xml.Document#getElementsByTagName -->

Finds every element in the document with a given tag name, in document order.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tagName` | string | Yes | |

- `options.tagName`: case-sensitive. Pass `*` to match every element.

**Returns:** `xml.Element[]`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.createTextNode(options)
<!-- id: N/xml.Document#createTextNode -->

Creates a text node holding the given string.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.data` | string | Yes | |

**Returns:** `xml.Node`, the new text node.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.createElementNS(options)
<!-- id: N/xml.Document#createElementNS -->

Creates a new element in a namespace.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.qualifiedName` | string | Yes | |

- `options.namespaceURI`: may be `null`.

The source says the new element's `localName`, `prefix` and `namespaceURI` are `null`.

> [!SOURCE-ERROR] As with `createAttributeNS`, describing a namespaced element as having null
> namespace properties contradicts what the method is for and looks copied from `createElement`.
> Inspect the created element instead of assuming those values.
<!-- source-error: N/xml.Document#createElementNS | - | namespace_properties_said_to_be_null -->

**Returns:** `xml.Element`, the new element.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.getElementById(options)
<!-- id: N/xml.Document#getElementById -->

Finds the element whose ID attribute has the given value.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.elementId` | string | Yes | |

**Returns:** `xml.Element`, or `null` when no element has that ID.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.createComment(options)
<!-- id: N/xml.Document#createComment -->

Creates a comment node with the given text.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.data` | string | Yes | |

**Returns:** `xml.Node`, the new comment node.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.getElementsByTagNameNS(options)
<!-- id: N/xml.Document#getElementsByTagNameNS -->

Finds every element in the document with a given namespace and local name, in document order.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.localName` | string | Yes | |

- `options.namespaceURI`: `*` matches any namespace.
- `options.localName`: `*` matches any local name.

**Returns:** `xml.Element[]`

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Document.createCDATASection(options)
<!-- id: N/xml.Document#createCDATASection -->

Creates a CDATA section node holding the given text, useful for content that would otherwise need
heavy escaping.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.data` | string | Yes | |

**Returns:** `xml.Node`, the new CDATA node.

> [!SOURCE-ERROR] The source states the created node's type as a document fragment, which does not
> fit a method that makes a CDATA section. Expect `CDATA_SECTION_NODE`, but check `nodeType` if your
> code depends on it.
<!-- source-error: N/xml.Document#createCDATASection | - | node_type_stated_as_document_fragment -->

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the input is malformed.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Document.inputEncoding
<!-- id: N/xml.Document#inputEncoding -->

`string`, read-only. The encoding that was in effect when the document was parsed. The value varies
by browser.

#### Document.documentElement
<!-- id: N/xml.Document#documentElement -->

`xml.Element`, read-only. The root element of the document; the usual starting point for walking
the tree.

#### Document.xmlEncoding
<!-- id: N/xml.Document#xmlEncoding -->

`string`, read-only. The encoding named in the XML declaration. In client scripts not supported in
Internet Explorer or Firefox.

#### Document.doctype
<!-- id: N/xml.Document#doctype -->

Typed `xml.Element`, read-only. The document's DOCTYPE. In client scripts not supported in Internet
Explorer.

> [!SOURCE-ERROR] A DOCTYPE is not an element, yet the source types this property as
> `xml.Element`. Do not call element methods on it without checking; its `nodeType` should tell you
> what you actually received.
<!-- source-error: N/xml.Document#doctype | - | doctype_typed_as_xml_element -->

#### Document.documentURI
<!-- id: N/xml.Document#documentURI -->

`string`. Where the document came from, or `null` when that is not known.

#### Document.xmlStandalone
<!-- id: N/xml.Document#xmlStandalone -->

`boolean`. `true` when the document is declared standalone. In client scripts not supported in
Internet Explorer or Firefox.

#### Document.xmlVersion
<!-- id: N/xml.Document#xmlVersion -->

`string`, read-only. The version from the XML declaration; it cannot be changed. In client scripts
not supported in Internet Explorer or Firefox.

### Element (object)
<!-- id: N/xml.Element -->

One element of a document, with methods for its attributes on top of the inherited
[`Node`](#node-object) members. Note that an element's text is not stored on the element itself
but in a child text node: for `<year>2015</year>`, the `2015` lives in the element's first child.
Read it through `textContent` or `firstChild.nodeValue`. Client and server; since 2015.2.

#### Element.getAttributeNodeNS(options)
<!-- id: N/xml.Element#getAttributeNodeNS -->

Looks up an attribute node by namespace URI and local name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.localName` | string | Yes | |

- `options.namespaceURI`: may be `null`.

**Returns:** `string` as given in the spec. See the [module-level source-error note](#nxml): this
type appears to be swapped with `getAttributeNS`.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Element.getElementsByTagName(options)
<!-- id: N/xml.Element#getElementsByTagName -->

Finds all descendant elements of this element with a given tag name, in document order.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.tagName` | string | Yes | |

- `options.tagName`: case-sensitive; `*` matches every element.

**Returns:** `xml.Element[]`

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.hasAttribute(options)
<!-- id: N/xml.Element#hasAttribute -->

Checks whether the element has an attribute of this name, either set explicitly or supplied as a
default.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

**Returns:** `boolean`

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Element.getAttributeNS(options)
<!-- id: N/xml.Element#getAttributeNS -->

Reads an attribute's value by namespace URI and local name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.localName` | string | Yes | |

- `options.namespaceURI`: may be `null`.

**Returns:** `string`, the attribute value. See the [module-level source-error note](#nxml) about
the return types of this method and `getAttributeNodeNS`.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Element.setAttributeNode(options)
<!-- id: N/xml.Element#setAttributeNode -->

Attaches an attribute node to this element. An existing attribute with the same name is replaced.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.newAttr` | xml.Attr | Yes | |

**Returns:** `xml.Attr`. When the call replaces an existing attribute, the source says the
attribute handed back is the new one (`newAttr`); what comes back when nothing was replaced is not
stated.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails, including when `newAttr` already belongs to another
  element (attribute in use).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.setAttributeNodeNS(options)
<!-- id: N/xml.Element#setAttributeNodeNS -->

The namespace-aware form of `setAttributeNode`: attaches the attribute node, replacing an existing
one with the same namespace URI and local name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.newAttr` | xml.Attr | Yes | |

**Returns:** `xml.Attr`. As with `setAttributeNode`, the new attribute is returned when it replaces
an existing one; the result when nothing was replaced is not stated.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails, including when `newAttr` is already in use on
  another element.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.removeAttributeNode(options)
<!-- id: N/xml.Element#removeAttributeNode -->

Removes the given attribute node from this element.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.oldAttr` | xml.Attr | Yes | |

**Returns:** `xml.Attr`. The source gives only the type and does not state which attribute node is
returned.

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails, including when the element does not have that
  attribute node (not found).

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.getElementsByTagNameNS(options)
<!-- id: N/xml.Element#getElementsByTagNameNS -->

Finds descendant elements of this element by namespace URI and local name, in document order.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.localName` | string | Yes | |

- `options.namespaceURI`: `*` matches any namespace.
- `options.localName`: `*` matches any local name.

**Returns:** `xml.Element[]`

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Element.setAttribute(options)
<!-- id: N/xml.Element#setAttribute -->

Adds an attribute, or changes its value if it already exists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |
| `options.value` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the input is malformed.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.setAttributeNS(options)
<!-- id: N/xml.Element#setAttributeNS -->

Adds a namespaced attribute, or changes its value if it already exists.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.qualifiedName` | string | Yes | |
| `options.value` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.removeAttribute(options)
<!-- id: N/xml.Element#removeAttribute -->

Removes an attribute by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.getAttribute(options)
<!-- id: N/xml.Element#getAttribute -->

Reads an attribute's value by name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

**Returns:** `xml.Attr` according to the spec's type, although the description says you get the
attribute's value.

> [!SOURCE-ERROR] The source contradicts itself here: its return-type row says `xml.Attr`, while
> its description says the method returns the attribute's value. The spec does not settle which is
> right; test the result (for example with `typeof`) before using it.
<!-- source-error: N/xml.Element#getAttribute | - | returns_row_says_xml_attr_description_says_value -->

**Governance:** 0 units.

**Availability:** client and server scripts.

**Since:** 2015.2

#### Element.hasAttributeNS(options)
<!-- id: N/xml.Element#hasAttributeNS -->

Checks whether the element has an attribute with this namespace URI and local name, either set
explicitly or supplied as a default.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.localName` | string | Yes | |

**Returns:** `boolean`

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the input is malformed.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Element.getAttributeNode(options)
<!-- id: N/xml.Element#getAttributeNode -->

Returns the attribute node (not just its value) for a name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.name` | string | Yes | |

**Returns:** `xml.Attr`

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Element.removeAttributeNS(options)
<!-- id: N/xml.Element#removeAttributeNS -->

Removes an attribute identified by namespace URI and local name.

| Parameter | Type | Required | Default |
|---|---|---|---|
| `options` | Object | Yes | |
| `options.namespaceURI` | string | Yes | |
| `options.localName` | string | Yes | |

**Returns:** `void`

**Errors:**

- `SSS_XML_DOM_EXCEPTION`: the operation fails.

**Governance:** 0 units.

**Availability:** client and server scripts; in client scripts not supported in Internet Explorer.

**Since:** 2015.2

#### Element.tagName
<!-- id: N/xml.Element#tagName -->

`string`, read-only. The element's tag name.

### Attr (object)
<!-- id: N/xml.Attr -->

An attribute node that belongs to an element. Obtain one from
[`Element.getAttributeNode`](#elementgetattributenodeoptions) or create one with
[`Document.createAttribute`](#documentcreateattributeoptions). Client and server; since 2015.2.

#### Attr.specified
<!-- id: N/xml.Attr#specified -->

`boolean`. `true` when the value was actually written in the parsed document; `false` when it is a
default coming from a DTD or schema.

#### Attr.ownerElement
<!-- id: N/xml.Attr#ownerElement -->

`xml.Element`, read-only. The element the attribute is attached to, or `null` when the attribute
is not in use on any element. In client scripts not supported in Internet Explorer.

#### Attr.name
<!-- id: N/xml.Attr#name -->

`string`, read-only. The attribute's name. When the owning element's `localName` is `null`, this is
the qualified name.

#### Attr.value
<!-- id: N/xml.Attr#value -->

`string`. The attribute's value as a string. When read, any entity references are replaced by what
they stand for. When set, the text is stored literally and not parsed as markup, so a `<` you assign
stays a character rather than starting a tag. Assigning something other than a string throws
`SSS_XML_DOM_EXCEPTION`.

## Enums

### xml.NodeType
<!-- id: N/xml.NodeType -->

The kinds of node a DOM tree can contain; read through [`Node.nodeType`](#nodenodetype). The values
are read-only. Since 2015.2.

| Value |
|---|
| `ATTRIBUTE_NODE` |
| `CDATA_SECTION_NODE` |
| `COMMENT_NODE` |
| `DOCUMENT_FRAGMENT_NODE` |
| `DOCUMENT_NODE` |
| `DOCUMENT_TYPE_NODE` |
| `ELEMENT_NODE` |
| `ENTITY_NODE` |
| `ENTITY_REFERENCE_NODE` |
| `NOTATION_NODE` |
| `PROCESSING_INSTRUCTION_NODE` |
| `TEXT_NODE` |

## Examples

A RESTlet that accepts a carrier's tracking feed as XML, rejects it if it is malformed, and pulls
out each package's tracking number and status with XPath.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType Restlet
 */
define(['N/xml', 'N/log'], (xml, log) => {
  const post = (body) => {
    let doc;
    try {
      doc = xml.Parser.fromString({ text: body.feed });
    } catch (e) {
      return { ok: false, reason: `Feed is not well-formed: ${e.message}` };
    }

    const packages = xml.XPath.select({ node: doc, xpath: '//shipment/package' });
    const results = packages.map((pkg) => {
      const statusNodes = pkg.getElementsByTagName({ tagName: 'status' });
      return {
        tracking: pkg.getAttributeNode({ name: 'tracking' }).value,
        status: statusNodes.length ? statusNodes[0].textContent : 'unknown'
      };
    });

    log.audit({ title: 'Packages in feed', details: results.length });
    return { ok: true, results };
  };

  return { post };
});
```

Building a small document from scratch. Text supplied by users goes in a text node, so it is
escaped on serialization; `xml.escape` is shown for the case where you assemble a fragment by hand.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/xml'], (xml) => {
  const buildPickNote = (binNumber, lines, memo) => {
    const doc = xml.Parser.fromString({ text: '<picknote/>' });
    const root = doc.documentElement;
    root.setAttribute({ name: 'bin', value: String(binNumber) });

    lines.forEach((line) => {
      const el = doc.createElement({ tagName: 'line' });
      el.setAttribute({ name: 'sku', value: line.sku });
      el.appendChild({ newChild: doc.createTextNode({ data: String(line.qty) }) });
      root.appendChild({ newChild: el });
    });

    const memoTag = `<memo>${xml.escape({ xmlText: memo })}</memo>`;
    const memoDoc = xml.Parser.fromString({ text: memoTag });
    root.appendChild({
      newChild: doc.importNode({ importedNode: memoDoc.documentElement, deep: true })
    });

    return xml.Parser.toString({ document: doc });
  };

  return { buildPickNote };
});
```

Duplicating a template row with an explicit `deep: true` (the default is not documented) and
placing the copy at the end with `insertBefore` and a `null` reference.

```js
/**
 * @NApiVersion 2.1
 */
define(['N/xml'], (xml) => {
  const addBlankRow = (tableXml) => {
    const doc = xml.Parser.fromString({ text: tableXml });
    const rows = doc.getElementsByTagName({ tagName: 'row' });
    if (!rows.length) {
      return tableXml;
    }
    const copy = rows[0].cloneNode({ deep: true });
    copy.textContent = '';
    rows[0].parentNode.insertBefore({ newChild: copy, refChild: null });
    return xml.Parser.toString({ document: doc });
  };

  return { addBlankRow };
});
```

Validating an inbound document against a schema kept in the File Cabinet (server only). The
schema's own imports are resolved from the folder given in `importFolderPathOrId`.

```js
/**
 * @NApiVersion 2.1
 * @NScriptType ScheduledScript
 */
define(['N/xml', 'N/log'], (xml, log) => {
  const isValidOrder = (orderXml) => {
    const doc = xml.Parser.fromString({ text: orderXml });
    try {
      xml.validate({
        xml: doc,
        xsdFilePathOrId: 'SuiteScripts/schemas/purchase_order.xsd',
        importFolderPathOrId: 'SuiteScripts/schemas'
      });
      return true;
    } catch (e) {
      log.error({ title: 'Schema check failed', details: `${e.name}: ${e.message}` });
      return false;
    }
  };

  const execute = () => {
    const sample = '<order><vendor>Acme Tools</vendor></order>';
    log.debug({ title: 'Valid?', details: isValidOrder(sample) });
  };

  return { execute };
});
```
