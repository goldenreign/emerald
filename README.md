# HTML 5 templating engine for Nimrod

## Overview

NimHTML is a macro-based type-safe templating engine for producing well-formed
HTML 5 web pages with Nimrod. It's currently under development. Most features
are still missing.

It is basically a domain-specific language implemented on top of the Nimrod
parser, utilizing Nimrod's macro system. Your HTML template will be parsed
along with your code. The template engine checks whether your HTML structure
is well-formed according to the HTML 5standard - however, it will not cover all
restrictions and rules of the specification. Rules that are covered are:

 * HTML tags are only allowed to contain child tags that are allowed by the
   specification.
 * HTML tags may only have attributes that are allowed by the specification
 * HTML tags must have attributes and childs that are required by the
   specification.

NimHTML has been inspired by [Jade][1] and [HAML][2]. But it doesn't use
RegEx for parsing its templates like Jade does *HOW COULD YOU EVEN THINK OF
DOING THAT SERIOUSLY*. Instead, it uses the awesome Nimrod compile-time
infrastructure.

Here is an example template to demonstrate what already works:

```nimrod
proc templ(youAreUsingNimHTML: bool): string =
	result = ""
	html5:
	    head:
	        title: "pageTitle"
	        script (`type` = "text/javascript"): """
	            if (foo) {
	                bar(1 + 5)
	            }
	            """
	    body:
	        h1: "NimHTML - Nimrod HTML5 templating engine"
	        d.content:
	            if youAreUsingNimHTML:
	                p: "You are amazing"
	            else:
	                p: "Get on it!"
	            p: """
	                NimHTML is a macro-based type-safe
	                templating engine which validates your
	                HTML structure and relieves you from
	                the ugly mess that HTML code is.
	                """
```

This produces:

```html
<!doctype html>
<html>
    <head>
        <title>pageTitle</title>
        <script type="text/javascript">                if (foo) {
                    bar(1 + 5)
                }
                </script>
    </head>
    <body>
        <h1>NimHTML - Nimrod HTML5 templating engine</h1>
        <div class="content">
            <p>You are amazing</p>
            <p>                    NimHTML is a macro-based type-safe
                    templating engine which validates your
                    HTML structure and relieves you from
                    the ugly mess that HTML code is.
                    </p>
        </div>
    </body>
</html>
```

## Features

This list of features grows as things get implemented.

### HTML tags

Currently, only a small list of HTML tags are supported, most of them are used
in the example above. More will be added. HTML tags currently can only be used
in block notation as shown in the example. If a name of a HTML tag is a keyword
in nimrod, you can escape the tag name with accents.

Because the `div` tag is used so frequently, it has `d` as shorthand.

**TODO:**

 * support non-block style for empty HTML tags (like `<br />`).
 * support using HTML tags along with strings

### String content

String content can be included as string literals. Long literals and infix
operators work, too.

**TODO:**

 * Fix output of long strings (identation and such)
 * Add ability to call procs that return a string

### Control structures

`if`, `elif`, `else` and `for` work exactly as in Nimrod.

**TODO:**

 * Support `while`
 * Support `case`

### Variables

You can declare variables using `var` everywhere.

### HTML attributes

You can add HTML attributes to HTML tags in braces right after the tag.
The syntax is

	tagName = expression

If the tag name is a Nimrod keyword
like `type`, you can escape the name with accents.

There is a shorthand notation for classes of an HTML tag: write the
class names right behind the tag, separated with `.`, like this:

    body.class1.class2:

**TODO:**

 * Provide a shorthand notation for `id`

### License

[WTFPL][3].

 [1]: http://jade-lang.com
 [2]: http://haml.info
 [3]: http://www.wtfpl.net