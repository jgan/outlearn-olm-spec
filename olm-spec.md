<!--
{
"name" : "outlearn-markdown-specification",
"version" : "0.2.0",
"freshnessDate": 2015-05-18,
"title" : "Outlearn Markdown Specification",
"description": "OLM (Outlearn Markdown) is an annotated, markdown-compatible text format for importing simple learning content as Outlearn modules.",
"homepage" : "http://www.github.com/outlearn-content/outlearn-markdown-spec",
"author" : "Will Koffel",
"license" : "CC BY",
"contact" : {"email": "will@outlearn.com"}
}
-->

<!-- @section -->

# Outlearn Markdown

> THIS IS A DRAFT DOCUMENT - WE WELCOME FEEDBACK AS THIS FORMAT EVOLVES - CURRENT AS OF May 15, 2015

Outlearn Markdown (*OLM*) is a [Github-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/) compatible file format for easily creating content that imports directly to Outlearn.

OLM files are regular Markdown files, annotated with HTML comments in a special format that enriches the content with Outlearn learning features.

**Note:** this specification itself is published in OLM.  Look at the raw markdown to see the annotations for this file in context.

## OLM as a full Module definition

OLM is primarily used to enrich content, but in many cases it can be used to define a whole Learning Module.  See the [OLP Specification](https://github.com/outlearn-content/outlearn-olp-spec) for details on defining Modules using OLM.

Limitations of OLM-defined Modules include:

* OLM is a single file format.  One text file contains all the content and meta-data.  If the amount of content is too large or diverse to naturally fit in a single file, the OLP package format is likely a better fit.
* Only a single module may be defined in an OLM file. An OLP package may define and import multiple learning modules at once.
* All assets (e.g. videos and images) must be hosted remotely and referenced within the OLM asset annotations.  This means that all referenced assets must be publicly available.  OLP allows bundled assets, including private videos and images.

If you have great existing Markdown content, OLM is a convenient way to get even more out of it through simple enrichments that give it new life in the Outlearn catalog.  If you are authoring learning materials from scratch, we recommend using the richer OLP approach for anything but simple, single-file Modules with a handful of Sections.

## Naming OLM Files

Outlearn will import OLM files that have an extension of `.olm` or an extension of `.md`.  We recommend `.olm` except in cases where `.md` is needed for compatibility with other systems that may require the `.md` suffix to properly render Markdown (notably Github).

<!-- @section -->

# Authoring OLM

OLM can be easily authored from scratch, or by editing an existing Markdown file to include specific Outlearn annotations in the form of HTML comments.

## Header metadata for full Modules

OLM files defining a Module must have header metadata specified at the top.  Fields are specified using JSON-like syntax inside an HTML comment block.

```markdown
<!--
{
"name" : "outlearn-module",
"version" : "1.5",
"freshnessDate": 2015-05-18,
"title" : "Outlearn Sample Module",
"description" : "This sample module can be customized around your learning content.",
"homepage" : "http://www.outlearn.com/",
"author" : "Will Koffel",
"license" : "public",
"contact" : { "email" : "will@outlearn.com" }
}
-->
```

See the full [OLP specification](https://github.com/outlearn-content/outlearn-olp-spec) for details.


<!-- @section -->

# Dividing Content into Sections

We strongly advise authors to break their content into bite-sized sections to make it easier for learners to navigate, consume, and track their learning progress.  To divide your OLM into navigable sections for a learner, you can add a `@section` annotation.

```markdown

 < !-- @section -->

# Introduction

 < !-- @section, "title": "Chapter 1 - Basics" -->

Let us get started with some fundamentals ...

 < !-- @section, "title": "Chapter 2 - Advanced Topics" -->

Now that we are comfortable with the basics ...

```

Each section has a title, which appears in the table of contents and learning experience on Outlearn.

In the simplest form, `<!-- @section -->` can stand alone.  When you import your OLM file, the section will get assigned a title based on the first header tag (line starting with `#`) that is encountered in the section.  In the example above, the opening section will automatically inherit the title "Introduction".

You can also give the title as shown by specifying it in the `@section` annotation.

<!-- @section -->

# Task Items

Users can engage with content through simple Task items.  A trackable checkbox can be created in your OLM content using the `@task` enrichment.


```markdown
   < !-- @task, "text" : "Run the above code example on your own machine."-->
```

## Requiring a Deliverable

Your Task item can optionally require a deliverable to be submitted by a learner.  This can be useful when asking learners to do things like:

- author a simple 1-paragraph summary of what they've learned from watching a video
- describe three ways a specific technical strategy might be applicable in your codebase
- submit a pull request URL for a simple code modification in a hands-on lab exercise
- paste a code sample, algorithm analysis, or other small response to a question or task

```markdown
   < !-- @task, "hasDeliverable" : true, "text" : "Fork the repository above, fix the broken test, and submit a URL for your pull-request."-->
```

<!-- @section -->

# Code Blocks

Markdown provides native support for code blocks.  If found, these code blocks will also get rendered by our syntax-highlighting library.  Code blocks using triple-back-tick are supported just like on Github, and will get automatic syntax-highlighting.

```markdown

```javascript
function sum(a, b) {
  return a+b;
}
```  # close fenced block

```

<!-- @section -->

# External Links

For content that you want to link to off Outlearn, a regular Markdown link will work, and will open in a new browser tab.

For a great experience on important external links, OLM provides an `@link` enrichment.

```markdown
  < !-- @link, "url" : "https://nodejs.org/", "text": "Install NodeJS" -->
```

At import-time, all `@link` enrichments will be expanded to include a title, summary, and image.  

If a link can be embedded in an iframe, it will be seamless integrated with the Outlearn learning environment.  Otherwise, it will open in a new browser tab.

An optional `task` attribute can be included, as seen above, which will create a Todo item automatically attached to this external link.

<!-- @section -->

# Multiple Choice Exercises


<!-- @section -->

# Embedding Videos

Video assets hosted on YouTube and Vimeo are supported via the `@asset` tag.

```markdown
< !-- @asset, "contentType": "outlearn/video", "provider": "vimeo", "url": "https://player.vimeo.com/video/42744689" -->

< !-- @asset, "contentType": "outlearn/video", "provider": "youtube", "url": "https://www.youtube.com/embed/CmjeCchGRQo" -->
```

Where possible, Outlearn will try to play the video in our preferred player, which includes advanced features for learners like keyboard controls, skip-back, multiple size options, and accessibility features.

<!-- @section -->

# @no-outlearn annotations

In order to use referenced sections, while still allowing your Markdown to render properly in places like Github or a blog, OLM allows you to include an alternate inline representation of a referenced asset.

```markdown

< !-- @asset, type: 'video/vimeo', title: 'Watch the Video', location: 'https://vimeo.com/61887298' -->

<!-- @no-outlearn -->

<iframe src="http://player.vimeo.com/video/61887298" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/61887298">Build Podcast 035 Capistrano</a> from <a href="https://vimeo.com/sayanee">Sayanee</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

< !-- @section -->

## Following section

More content after video here.

```

An `@no-outlearn` section will end when the parser encounters either an `@section` annotation, or a `@yes-outlearn` annotation.

<!-- @section, "tracked": false -->

# Examples of OLM

For many examples of content in OLM and OLP format, please refer to the [Outlearn Content](http://www.github.com/outlearn-content) repo on Github.
