---
layout: post
title: "Add a Table of Contents to Jekyll Blog Written in Markdown"
date: '2018-10-28 20:37'
tags:
  - Jekyll
---

For my Jekyll blog I want to add a table of contents to some of my larger posts so readers have an overview of the post content and may click links to jump to sections which interest them most. I’d like Jekyll to automatically generate the markup for the table of contents based off the headers in the post.

Here’s how I set up Jekyll to get my table of contents (toc) feature.

# Configuration
By default, Jekyll on Github pages is configured to use Kramdown to parse and convert Markdown to html format for blog post pages. Jekyll’s markdown conversion option is set in the `_config.yml` file like this:

```yaml
markdown: kramdown
```

Kramdown, in turn, is set by default to support the automatic generation of header IDs during conversion.

Kramdown (also by default) supports the automatic generation of the table of contents of all headers that have an ID set.

When the auto_ids option is set, all headers will appear in the table of contents as they all will have an ID. Assign the class name “no_toc” to a header to exclude it from the table of contents.

My action step to configure Jekyll to get my table of contents feature was to change nothing.

# Implementation
Implementing the auto table of contents feature is almost as easy as the necessary configuration.

Here are the steps to add the toc:

1. Add an ordered or unordered list to the content body at the point you want the table of contents to appear;
1. Add the following snippet immediately below the list: `{:toc}`
1. Jekyll (using Kramdown as its converter) will replace the list with a toc automatically generated from the headings in the content

Here’s the markup I use to add tocs to my posts:

```
* TOC
{:toc}
```

The number of items in the list and the content of the items do not matter, because they will be replaced by the toc when the content is parsed. Note that the `{:toc}` tag does not generate anything; it must be placed immediately after an ordered or unordered list.
