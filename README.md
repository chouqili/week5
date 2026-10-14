# Topic 5 Lab - XML and JSON

158.258 Web Development - Topic 5 laboratory exercises (14 October 2026)

Zezhi Li - Massey student ID 25014437, NZ244 student ID 247107.

This folder is a complete, static web site. It can be opened from a local web
server or published to GitHub Pages exactly as it is, with `index.html` as the
home page.

## The two labs

| Lab | Page | What it does |
| --- | --- | --- |
| 1 - XML | `xml-lab.html` | Fetches `book.xml`, parses the text with `DOMParser` and lists the title, author and price of every book. |
| 2 - JSON | `json-lab.html` | Fetches `userProfiles.json` and shows every user's name, email, date of birth, address and preferences. |

## Files

| File | Purpose |
| --- | --- |
| `index.html` | Contents page: the two labs, where each lab instruction is answered, and the lecture material each file uses. |
| `xml-lab.html` | Lab 1 - the book catalogue page. |
| `json-lab.html` | Lab 2 - the user profiles page. |
| `book.xml` | The book catalogue in XML: eight books, each with an `id` attribute and the child elements `title`, `author` and `price`. |
| `book.dtd` | The DTD that says what a valid catalogue may contain. |
| `book.xsd` | The XML Schema of the same rules, pointed at by `xsi:noNamespaceSchemaLocation` in `book.xml`. |
| `userProfiles.json` | Five user profiles in JSON, each with an address and preferences. |
| `userProfiles.schema.json` | The JSON Schema the profiles can be validated against. |
| `favicon.svg` | The tab icon used by the three pages. |
| `lab.css` | Shared style sheet used by all of the pages. |

## Where the Topic 5 lecture material is used

| Lecture | File |
| --- | --- |
| Introduction to XML - declaration, single root element, closing tags, case, quoted attribute values, comments, `&amp;` entity, attributes for meta information | `book.xml` |
| Topic 5 - XML DTDs - element declarations, `#PCDATA`, `?` and `+`, attribute types, `#REQUIRED` | `book.dtd` |
| Topic 5 - XML Schema - `xs:complexType`, `xs:sequence`, `minOccurs`/`maxOccurs`, `xs:simpleContent`, `xs:extension`, `xs:decimal`, `xs:gYear`, `xs:ID` | `book.xsd` |
| The JSON Data Model - objects, arrays, name/value pairs, string / number / boolean values, nesting | `userProfiles.json` |
| JSON Schema Basics - `$schema`, `type`, `properties`, `required`, `enum`, `pattern`, `minItems`, `additionalProperties` | `userProfiles.schema.json` |
| Asynchronous JavaScript - fetch API, promises, `then()` / `catch()`, `response.ok`, `response.text()`, `response.json()` | `xml-lab.html`, `json-lab.html` |

## How the exercises are covered

**Lab 1 (XML).** `book.xml` holds one root element, `catalogue`, with eight
`book` children. Each book carries the `id` attribute the lab asks for, and the
children `title`, `author` and `price`, with `year` and `genre` as optional extras
that show the vocabulary can be extended. The `currency` attribute on `price`
follows the lecture's rule that an attribute carries information about the value
rather than being the value itself. The ampersand in one title is written as the
pre-defined entity `&amp;` so that the document stays well formed. `xml-lab.html`
fetches the file with the fetch API, gets the text with `response.text()`, turns
the text into a document with `DOMParser` exactly as the lab instructions show,
checks for a `parsererror` element in case the XML is broken, and then reads the
books out of the tree with `getElementsByTagName()`. The page reports how many
books it read and the price range instead of only drawing the table.

**Lab 2 (JSON).** `userProfiles.json` holds five profiles in a `profiles` array.
Each profile has `firstName`, `lastName`, `email` and `dateOfBirth`, an `address`
object with `street`, `city`, `state` and `zipCode`, and a `preferences` object
with `theme`, `language`, `notifications` and the `subscription` field the lab
asks to be added. `json-lab.html` uses the fetch pattern from the lecture,
including the `if (!response.ok) throw ...` guard, and deserialises the text with
`response.json()`. Every profile becomes a card showing all of the fields, with
the date of birth written out in full and the age worked out from it.

**Validation.** The XML is checked for well-formedness by the parser, and its
structure is described by `book.dtd` and `book.xsd`, either of which can be used
with the online validators named in the lab. Both were checked against
`book.xml` and both accept it; both also reject a book with no `title`, a book
with no `price`, a price with no `currency` and a document that repeats an `id`.
The JSON is checked for well-formedness by `JSON.parse()`, and
`userProfiles.schema.json` adds the semantic checks the JSON Schema lecture
describes: the five profiles satisfy it, and it reports a missing `email`, a
three-digit postcode, a theme outside the list and an unexpected extra field.

## Run the pages locally

The two lab pages fetch their data files, and a browser will not fetch a file
from a `file://` address, because that would break the same-origin rule. Open the
pages through a web server instead:

```bash
cd xml-json-lab
python -m http.server
```

Then open <http://localhost:8000/>.

## Publish the site on GitHub Pages

1. Create a new **public** repository on GitHub, for example `week5`.
2. This folder is already a local git repository with one commit. Point it at
   the new repository and push:

   ```bash
   git remote add origin https://github.com/chouqili/week5.git
   git branch -M main
   git push -u origin main
   ```

3. On GitHub open **Settings > Pages**, set *Source* to **Deploy from a branch**,
   branch **main**, folder **/ (root)**, then save.
4. After a minute the site is live at
   `https://chouqili.github.io/week5/`.

## Testing the pages

Both pages were opened in Microsoft Edge before the work was handed in. The XML
page reports "book.xml fetched and parsed with DOMParser: 8 books, $16.40 to
$27.00, average $22.42" and the table lists all eight titles, authors and prices.
The JSON page reports "userProfiles.json fetched and parsed: 5 profiles; 3
subscribed to the newsletter and 3 have notifications switched on", and one card
is drawn per profile. Neither page logs a JavaScript error or a failed request.

The four things that can go wrong were then tested as well, by handing the page a
data file that is not what it should be:

| Test | What the page reports |
| --- | --- |
| XML that is not well formed | `book.xml is not well formed - error on line 1 at column 57: Opening and ending tag mismatch: title line 1 and catalogue` |
| `book.xml` missing | `book.xml could not be loaded (HTTP 404)` |
| JSON that will not parse | `Fetch error: Expected double-quoted property name in JSON at position 27` |
| `userProfiles.json` missing | `Fetch error: 404` |

In each case the message appears in place of the table or the cards, and the page
still logs no JavaScript error.
