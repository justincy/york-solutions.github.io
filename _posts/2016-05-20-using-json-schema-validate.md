---
layout: post
title: "Using JSON Schema to Validate JSON"
date: 2016-05-04
---

When creating a new REST API I wanted to validate the return format of the JSON
response. While sarching for a solution, I discovered [JSON Schema](http://json-schema.org/).

JSON Schema defines a human and machine readable description of a JSON
structure and format. It looks like this:

```js
{
	"title": "Example Schema",
	"type": "object",
	"properties": {
		"firstName": {
			"type": "string"
		},
		"lastName": {
			"type": "string"
		},
		"age": {
			"description": "Age in years",
			"type": "integer",
			"minimum": 0
		}
	},
	"required": ["firstName", "lastName"]
}
```

It's a little verbose, but it's flexible. Most importantly it enables automatic
validation of JSON responses. With many [JSON schema validators](http://json-schema.org/implementations.html)
available, you can quickly integrate them into your tests. Here's an example
using [tv4](https://github.com/geraintluff/tv4).

```js
assert(tv4.validate(data, schema));
```

The first weakness you might run into is that the spec allows undefined properties
by default. In most situations, you want unexpected properties to be an error.
Most validators account for this by having a "ban unknown properties" mode.

If your schema is advanced you might start wanting some form of inheritance,
but JSON schema doesn't support it. [GEDCOM X JSON Schema](https://github.com/rootsdev/gedcomx-json-schema)
gets around this by merging the property objects itself as it assembles the schema.