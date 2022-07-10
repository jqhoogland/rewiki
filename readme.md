# ![rewiki][logo]

<!--
[![Build][build-badge]][build]
[![Coverage][coverage-badge]][coverage]
[![Downloads][downloads-badge]][downloads]
[![Size][size-badge]][size]
[![Sponsors][sponsors-badge]][collective]
[![Backers][backers-badge]][collective]
[![Chat][chat-badge]][chat]
-->

**rewiki** is a tool that transforms wikitext with plugins.
These plugins can inspect and change your markup.
You can use rewiki on the server, the client, CLIs, deno, etc.

It's based on [remark][] a tool for transforming
markdown that is part of the [unified][] ecosystem.

> Note: rewiki is not (yet) affiliated with the unified ecosystem.

## Feature highlights

*   ~~**popular**~~ (this has only been around for about a day)
*   ~~compliant~~ (wikitext is a nightmare of inconsistencies and variations from wiki to wiki)
*   ~~plugins~~ (0 plugins so far that leave you with no choice)
*   [x] **[ASTs][syntax-tree]** (inspecting and changing content made easy)

## Intro

rewiki is a collection of tools that work with wikitext as
structured data, specifically ASTs (abstract syntax trees).
ASTs make it easy for programs to deal with wikitext.
We call those programs plugins.
Plugins inspect and change trees.
You can use the many existing plugins or you can make your own.

*   to learn wikitext, see this [cheatsheet and tutorial][cheat]
*   for more about unified, see [`unifiedjs.com`][site]

<!-- 
*   for updates, see [Twitter][]
*   for questions, see [support][]
*   to help, see [contribute][] or [sponsor][] below
-->

## Contents

*   [What is this?](#what-is-this)
*   [When should I use this?](#when-should-i-use-this)
*   [Examples](#examples)
    *   [Example: turning wikitext into HTML](#example-turning-wikitext-into-html)
*   [Syntax](#syntax)
*   [Syntax tree](#syntax-tree)
*   [Types](#types)
*   [Compatibility](#compatibility)
*   [Security](#security)
<!--
 *   [Contribute](#contribute)
*   [Sponsor](#sponsor)
*   [License](#license)
-->

## What is this?

You can use plugins to turn wikitext into HTML.
**In**:

```wikitext
= Hello, 'Mercury'! =
```

**Out**:

```html
<h1>Hello, <em>Mercury</em>!</h1>
```

You can use plugins to change wikitext.
**In**:

```wikitext
= Hi, Saturn! ==
```

**Plugin**:

```js
import {visit} from 'unist-util-visit'

/** @type {import('unified').Plugin<[], import('wtast').Root>} */
function myRemarkPluginToIncreaseHeadings() {
  return (tree) => {
    visit(tree, (node) => {
      if (node.type === 'heading') {
        node.depth++
      }
    })
  }
}
```

**Out**:

```wikitext
== Hi, Saturn! ==
```


## When should I use this?

Don't. I mean seriously. Avoid wikitext if you can and stick to markdown.

Only use this if you're working with existing wikitext, and you want to parse it into a machine-queryable format or convert it to some other form.
## Examples

### Example: turning wikitext into HTML

rewiki is an ecosystem around wikitext.
A different ecosystem is for HTML: [rehype][].
The following example turns wikitext into HTML by combining both ecosystems with
[`rewiki-rehype`][rewiki-rehype]:

```js
import {unified} from 'unified'

// So these plugins actually don't exist yet...
import rewikiParse from 'rewiki-parse'
import rewikiRehype from 'rewiki-rehype'
import rehypeSanitize from 'rehype-sanitize'
import rehypeStringify from 'rehype-stringify'

main()

async function main() {
  const file = await unified[]
    .use(rewikiParse)
    .use(rewikiRehype)
    .use(rehypeSanitize)
    .use(rehypeStringify)
    .process('# Hello, Neptune!')

  console.log(String(file))
}
```

Yields:

```html
<h1>Hello, Neptune!</h1>
```

## Syntax

rewiki follows CommonMark, which standardizes the differences between wikitext
implementations, by default.
Some syntax extensions are supported through plugins.

We use [`micromark`][micromark] for our parsing.
See its documentation for more information on wikitext, CommonMark, and
extensions.

## Syntax tree

The syntax tree format used in rewiki is [wtast][].
It represents wikitext constructs as JSON objects.
**In**:

```wikitext
== Hello ''Pluto''! ==
```

**Out**:

```js
{
  type: 'heading',
  depth: 2,
  children: [
    {type: 'text', value: 'Hello '},
    {type: 'emphasis', children: [{type: 'text', value: 'Pluto'}]}
    {type: 'text', value: '!'}
  ]
}
```

## Types

<!--The rewiki organization and the unified collective as a whole is fully typed
with [TypeScript][].
Types for wtast are available in [`@types/wtast`][types-wtast].-->

For TypeScript to work, it is particularly important to type your plugins
correctly.

We strongly recommend using the `Plugin` type from `unified` with its generics
and to use the node types for the syntax trees provided by this project.

```js
/**
 * @typedef {import('wtast').Root} Root
 *
 * @typedef Options
 *   Configuration (optional).
 * @property {boolean} [someField]
 *   Some option.
 */

// To type options and that the it works with `wtast`:
/** @type {import('unified').Plugin<[Options?], Root>} */
export function myRemarkPluginAcceptingOptions(options) {
  // `options` is `Options?`.
  return function (tree, file) {
    // `tree` is `Root`.
  }
}
```
<!--
## Compatibility

Projects maintained by the unified collective are compatible with all maintained
versions of Node.js.
As of now, that is Node.js 12.20+, 14.14+, and 16.0+.
Our projects sometimes work with older versions, but this is not guaranteed.
-->

## Security

As wikitext can be turned into HTML and improper use of HTML can open you up to
[cross-site scripting (XSS)][xss] attacks, use of rewiki can be unsafe.
When going to HTML, you will likely combine rewiki with **[rehype][]**, in which
case you should use [`rehype-sanitize`][rehype-sanitize].

Use of rewiki plugins could also open you up to other attacks.
Carefully assess each plugin and the risks involved in using them.

For info on how to submit a report, see our [security policy][security].
<!--

## Contribute

See [`contributing.md`][contributing] in [`rewikijs/.github`][health] for ways
to get started.
See [`support.md`][support] for ways to get help.
Join us in [Discussions][chat] to chat with the community and contributors.

This project has a [code of conduct][coc].
By interacting with this repository, organization, or community you agree to
abide by its terms.

## Sponsor

Support this effort and give back by sponsoring on [OpenCollective][collective]!
-->

## License

[MIT](license) © [Jesse Hoogland](https://jessehoogland.com)

<!-- Definitions -->

[remark]: https://github.com/remarkjs/remark

[logo]: logo.svg

[build-badge]: https://github.com/rewikijs/rewiki/workflows/main/badge.svg

[build]: https://github.com/rewikijs/rewiki/actions

[coverage-badge]: https://img.shields.io/codecov/c/github/rewikijs/rewiki.svg

[coverage]: https://codecov.io/github/rewikijs/rewiki

[downloads-badge]: https://img.shields.io/npm/dm/rewiki.svg

[downloads]: https://www.npmjs.com/package/rewiki

[size-badge]: https://img.shields.io/bundlephobia/minzip/rewiki.svg

[size]: https://bundlephobia.com/result?p=rewiki

[chat-badge]: https://img.shields.io/badge/chat-discussions-success.svg

[chat]: https://github.com/rewikijs/rewiki/discussions

[sponsors-badge]: https://opencollective.com/unified/sponsors/badge.svg

[backers-badge]: https://opencollective.com/unified/backers/badge.svg

[security]: https://github.com/rewikijs/.github/blob/main/security.md

[health]: https://github.com/rewikijs/.github

[contributing]: https://github.com/rewikijs/.github/blob/main/contributing.md

[support]: https://github.com/rewikijs/.github/blob/main/support.md

[coc]: https://github.com/rewikijs/.github/blob/main/code-of-conduct.md

[collective]: https://opencollective.com/unified

[xss]: https://en.wikipedia.org/wiki/Cross-site_scripting

[typescript]: https://www.typescriptlang.org

[cheat]: https://github.com/jqhoogland/wtast/wikitext.md

[twitter]: https://twitter.com/unifiedjs

[site]: https://unifiedjs.com

[topic]: https://github.com/topics/rewiki-plugin

[popular]: https://www.npmtrends.com/rewiki-parse-vs-marked-vs-micromark-vs-wikitext-it

[types-wtast]: https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/wtast

[unified]: https://github.com/unifiedjs/unified

[rewiki-gfm]: https://github.com/rewikijs/rewiki-gfm

[rewiki-toc]: https://github.com/rewikijs/rewiki-toc

[rewiki-rehype]: https://github.com/rewikijs/rewiki-rehype

[rewiki-html]: https://github.com/rewikijs/rewiki-html

[rewiki-lint]: https://github.com/rewikijs/rewiki-lint

[awesome-rewiki]: https://github.com/rewikijs/awesome-rewiki

[rehype]: https://github.com/rehypejs/rehype

[rehype-sanitize]: https://github.com/rehypejs/rehype-sanitize

[wtast]: https://github.com/syntax-tree/wtast

[micromark]: https://github.com/micromark/micromark

[rewiki-parse]: packages/rewiki-parse/

[rewiki-stringify]: packages/rewiki-stringify/

[rewiki-core]: packages/rewiki/

[rewiki-cli]: packages/rewiki-cli/

[list-of-plugins]: doc/plugins.md#list-of-plugins

[syntax]: #syntax

[syntax-tree]: #syntax-tree

[plugins]: #plugins

[contribute]: #contribute

[sponsor]: #sponsor
