# blog.leguen.fr

Most of the readme as been written by [mtlynch](https://www.github.com/mtlynch). Huge thanks to him.

## Overview

This is the source for https://blog.leguen.fr.

## Code style guides

New code should adhere to the appropriate Google Style guide for the given language:

* [HTML/CSS](https://google.github.io/styleguide/htmlcssguide.html)
* [JavaScript](https://google.github.io/styleguide/jsguide.html)

### Liquid

#### Line length

* Limit lines to a maximum of 80 columns in length.
  * It is acceptable to exceed 80 columns if there is no way to break up the line, but developers should observe the 80 column limit where possible.


#### Whitespace

##### Tags

Tags should contain a single space after the opening `{%` and before the closing `%}`:

```liquid
{% assign my_variable = "tomato" %}
```

###### Inline vs. block

For tags that have a start and end, write them on a single line if the start tag, end tag, and body all fit within the line limit:

```liquid
{% comment %} Store all files that match the pattern. {% endcomment %}
```

```liquid
{% if page.title == 'Bio' %} Here's my bio! {% endif %}
```

When using inline syntax, there must be a single space after the opening tag and a single space preceding the closing tag.

When the tags and body do not fit on a single line, use block syntax:

```liquid
{% comment %}
  Store all files whose parent no longer exists. This can be due to a variety of
  circumstances, such as purging or filesystem errors.
{% endcomment %}
{% assign orphaned_files = ... %}
```

Indent the body of a block by two spaces from its opening and closing tags.

##### Variables

* Variables should have a single space before the opening `{{` and closing `}}`:

```liquid
{% assign filename = "links.csv" %}
{{ filename }}
```

##### Filters

* Filters should have a single space on either side of the pipe character.
* For filters that take arguments, the filter name should be immediately followed by a colon and a single space.
* For filters with multiple arguments, each argument should be preceded by a single space.

```liquid
{{ "Have you read 'James & the Giant Peach'?" | escape }}
```

```liquid
{{ 4 | plus: 2 }}
```

```liquid
{{ "Take my protein pills and put my helmet on" | replace: "my", "your" }}
```

#### Naming

* Variable names should use snake case (`image_size`, `background_color`).

#### Comments

* Write comments in full sentences using standard English capitalization and punctuation.

## Firebase deploy

1. Connect to firebase and generate a token using ```firebase login:ci```
2. go on travis-ci and add the generated key.

## Dev / prod consistency

`script/_serve_dev.sh` builds the site in dev mode (without google analytics and disqus) and starts a web server on http://localhost:4000.

`.travis.yml` when pushing on github, it builds the site in production mode and pushes the static files to Firebase.

Dev and prod configurations should be as similar as possible. Any change in dev should also be made to prod and vice versa, except for features we deliberately make different for dev and prod (e.g. removing GA tag from dev, removing jekyll-admin from prod).

## Theme

This blog uses the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) theme.

Where possible, avoid duplicating code from the theme. Sometimes this will be unavoidable. The blog currently duplicates theme code in several places, but we seek to minimize this duplication.

## Pull requests

### Merge conflicts

If a PR has merge conflicts with the main repo's `master` branch, rebase the PR onto `master`. Do not include merge commits in a PR.

### Pull request style

PRs should have a descriptive one-line summary to explain the change. The PR description should add any additional required context or explanation for the change. For simple or obvious PRs, a PR description is not required.

If the PR fixes an issue, include the text "Fixes #XX" in the PR description, where `XX` is the [repo issue](https://github.com/PierreLeGuen/blog.leguen.fr/issues) number. This allows Github to cross-reference between PRs and issues.

## Build Failures

### HTMLProofer

If HTMLProofer fails on a broken link, we have three options: suppress the error, fix the link, or remove the link.

You should suppress the error if the link works fine in a browser, but fails in Travis occasionally. To do this, open `_tests/build`, update the `--url-ignore` flag for the `htmlproofer` command. Add a comment above the command to explain why we're adding this suppression. Be especially conservative about suppressing warnings on affiliate links (links to products that can be purchased) because we care when these die.

If the link is just permanently broken and does not load, even in a browser, either replace the link with another that achieves the same effect or remove the link entirely.

## Compatibility targets

The site should render properly on all of the following operating systems (latest stable releases):

* Android
* iOS
* Windows
* OS X
* Linux (any flavor)


The site should render properly on all the following browsers:

* Chrome
* Firefox
* Safari

### Compatibility testing

Developers need not verify every change on every possible OS/browser combination. Developers should test CSS or layout changes on at least one desktop OS and one mobile OS (preferably not the same browser for both). For more complex changes to CSS or layout, developers should test more than two OS+browser combinations, but it's at the developer's discretion how exhaustively to test.

## Prose style guide

### Headings

* Headings start with `<h1>` (single hash in Markdown).
* Headings use sentence-style capitalization (only first word is capitalized).
* Headings do not have trailing punctuation.

### Point of view

* Author is first-person singular (I).
* Reader is second-person singular (you).

### Readers

* Readers of the blog are collectively referred ot as "readers" or "visitors."
  * Readers are not referred to as "users" and should never be described as "traffic."

### Numbers

* For 1-9, spell out the number.
  * *Except*: When the number is a multiplier (2x, 9x).
* For all other numbers, use numerals.
* For numbers 1,000 or over, write commas to separate thousands.
* For number ranges, use a regular hyphen and no space (200-300).

### Time of day

* Use AM/PM, separated by a space (2 AM, 4:30 PM).

### Pronouns

* When referring to non-specific people (e.g. "the user", "the client"), use "they."

### Names

* reddit is written all lowercase.

### e.g. / i.e.

* Put e.g. or i.e. within parentheses (e.g. like this).

### Oxford comma

* Always use an Oxford comma.

### Spelling conventions

* ebook (not e-book)

## Including downloadable file content in posts

### For non-yml files

1. Place the file into the `_files` directory.
1. Add yaml frontmatter to the file with a title key, for example: 
   ```yaml
   ---
   title: "filename.extension"
   ---
   ```
1. In the post, use the below syntax to include the code sample.
    ```
   {% include files.html title="filename.extension" language="bash" %}
    ```
   The `title` param is *required* and needs to match the title key that was inserted in step 2. The `language` param is *optional* and indicates the syntax highlighting language to use for the file.

### For yml files

1. Place the yml file into the `_ymls` directory with no file extension.
2. Add yaml frontmatter to the file with a title key, for example: 
    ```yaml
    ---
    title: "filename.yml"
    ---
   ```
3. In the post, use the below syntax to include the yml file.
    ```
    {% include files.html title="filename.yml" language="yml" %}
    ```
    The `title` param is *required* and needs to match the title key that was inserted in step 2. The `language` param is also *required* and must be set to "yml".
