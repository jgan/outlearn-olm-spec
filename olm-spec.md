<!--
name: outlearn-markdown-specification
version : 0.0.1
title : "Outlearn Markdown Specification"
description: "OLM (Outlearn Markdown) is an annotated, markdown-compatible text format for importing simple learning content as Outlearn modules.  This specification describes in plain terms how to easily author OLM files."
homepage : "http://www.outlearn.com/outlearn/outlearn-markdown-spec"
author : "Will Koffel"
license : "Public",
contact : "will@outlearn.com"
-->

### This is a DRAFT document, not yet fully up to date with the latest OLP specification, please be advised.

<!-- @section -->

# Outlearn Markdown

Outlearn Markdown, or *OLM* is a [Github-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/) compatible file format for easily creating content that transpiles to the official [Outlearn Package (OLP)](http://www.outlearn.com/outlearn/outlearn-package-spec) format.

OLM files are regular Markdown files, annotated with HTML comments in a special format that dicates metadata and structure to the Outlearn learning system.

**Note:** this specification itself is published in OLM.  Look at the raw markdown to see the annotations for this file in context.

## Limitations and Recommendations

OLM is *not* a full-featured replacement for OLP, Outlearn's official package format.  Limitations of OLM include:

* OLM is a single file format.  One text file contains all the content and meta-data.  If the amount of content is too large or diverse to naturally fit in a single file, the OLP package format is likely a better fit.
* Only a single module may be defined in an OLM file.  Although it may reference existing modules, such as when compiling a learning path from pre-existing Outlearn modules, it may not provide original content for more than one module.  An OLP package may define and import multiple modules at once.
* All assets (e.g. videos and images) must be hosted externally and referenced within the OLM asset annotations.  This means that all referenced assets must be publicly available.  OLP allows bundled assets, including private videos and content.

If you have great existing Markdown content, OLM is a convenient way to get even more out of it through simple annotations that give it life in the Outlearn catalog.  If you are authoring learning materials from scratch, we recommend using the richer OLP approach for anything but simple, single-file modules with a handful of sections.

### More on OLP

The official Outlearn package format is very easy to author by hand as well.  In the simplest cases, it consists of nothing more than a small JSON file that references assets in the same directory, like images, videos, code snippets, markdown files, or even OLM files.

OLM is pre-processed into OLP before importing, so it will always be a subset of the OLP format.

For more detailed options that OLM supports, it may be helpful to reference analagous sections in the OLP specification. [We should link to analagous sections where appropriate....](#)

### Naming OLM Files

Outlearn will import OLM files that have an extension of `.olm` or an extension of `.md`.  We recommend `.olm` except in cases where `.md` is needed for compatibility with other systems that may require the `.md` suffix to properly render the Markdown.

<!-- @section -->

# Authoring OLM

OLM can be easily authored from scratch, or by non-destructively editing an existing Markdown file to include specific Outlearn annotations in the form of HTML comments.

## Header metadata

All OLM files must have header metadata specified at the top.  The minimum required fields are:

* **name**: a valid Outlearn module name, this remains fixed and is the ID for your module.  It must be unique within your outlearn user or organization account.
* **version**: a [semantic version](http://semver.org/) number for this module.  When you make changes, increase the version number before republishing
* **title**: A descriptive title for your module, which will appear in search results and display wherever your module is shown.

There are many optional fields.  See the full [OLP specification](#) for details.

Fields are specified inside an HTML comment block, one line per field, with the name of the field separated from its value by a single colon.  Extra white space around keys or values is ignored, as are blank lines.

```markdown
<!--
name: outlearn-markdown-specification
version : 0.0.1
title : "Outlearn Markdown Specification"
description: "OLM (Outlearn Markdown) is an annotated, markdown-compatible text format for importing simple learning content as Outlearn modules.  This specification describes in plain terms how to easily author OLM files."
homepage : "http://www.outlearn.com/outlearn/outlearn-markdown-spec"
author : "Will Koffel"
license : public
contact : "will@outlearn.com"
-->

```

To maintain readability, any value may be continued on contiguous lines by beginning the subsequent lines with a single `-` character.

```markdown
<!--
name: capistrano-post
version : "0.0.2"
title : "Capistrano"
description: Capistrano is remote multi-server automation tool
- that is often used for deployment. In this episode, we will
- first learn how to execute command line tasks in several servers,
- then we will deploy a Github repo on a newly fired up EC2 instance.
-->
```

### Value Fields

Most metadata fields are regular values.  Both the value name and the value itself may be quoted or unquoted.

```markdown
<!--
title: "My Great Content"
-->
```

### Array Fields

Some fields, like `author` may contain more than one value.  If a set of comma-separated values are provided, they will be extracted into an array.  Inner-commas may be backslash-escaped.

```markdown
<!--
author: Bob, Alice, Steve, Mary, Supermegacorp\, Inc.
-->
```

## Breaking Modules into sections

We strongly advise authors to break their content into bite-sized sections to make it easier for learners to navigate, consume, and track their learning progress.  To divide your Outlearn Markdown into navigatable sections for a learner, you can add a `@section` annotation.

```markdown

<!-- @section -->

# Introduction

<!-- @section, title: 'Chapter 1 - Basics -->

# Chaper 1

<!-- @section, title: 'Chapter 2 - Advanced Topics' -->

# Chapter 2

<!-- @section, type: 'text/markdown', title: 'Appendix - Additional Materials' -->

# Additional Materials

```

Each section has a title, which appears in the table of contents and learning experience on Outlearn.

In the simplest form, `<!-- @section -->` can stand alone.  When you import your OLM file, the section will get assigned a title based on the first header tag (line starting with `#`) that is encountered in the section.  In the example above, the opening section will automatically inhereit the title "Introduction".

You can override the title as shown by specifying it in the `@section` annotation.

### Assets

Some assets (like images) can natively be included in Markdown.  That will work, and will render acceptably in the Outlearn learning experience.  However, by explicitly declaring an asset with an @asset annotation, it gets benefits, such as a learner's ability to track progress on it, bookmark it for later, and to see it in the table of contents for your learning module.  OLM provides support for a growing number of referenced asset types, described below.

**Note** referenced assets require a `title` attribute since one can not be derived from an inline markdown header.

#### Video Asset

Here are some examples of video assets.

```markdown
<!-- @asset, type: 'video/vimeo', title: 'Watch the Video', location: 'https://vimeo.com/61887298' -->

<!-- @asset, type: 'video/youtube', title: 'Watch the Video', location: 'https://www.youtube.com/watch?v=CmjeCchGRQo' -->

<!-- @asset, type: 'video/mp4', title: 'Watch the Video', location: 'http://www.example.com/training/video1.mp4' -->
```

Where possible, Outlearn will try to play the video in our preferred player, which includes advanced features for learners like keyboard controls, skip-back, multiple size options, and accessibility features.  For certain video sources like YouTube, a standard embedded player may be rendered.

#### Audio Asset

```markdown
<!-- @asset, type: 'audio/soundcloud', title: 'Uptown Funk', location: 'https://soundcloud.com/mmmusic/mark-ronson-uptown-funk' -->

<!-- @asset, type: 'audio/mpeg', title: 'Watch the Video', location: 'http://www.podtrac.com/pts/redirect.mp3/twit.cachefly.net/audio/twit/twit0497/twit0497.mp3' -->
```

#### Image Asset

```markdown
<!-- @asset, type: 'image/jpeg', title: 'Architecture Diagram', location: 'http://ad009cdnb.archdaily.net/wp-content/uploads/2011/05/1304980266-ad30-circulation-diagram.jpg' -->
```

### @no-outlearn and @yes-outlearn annotations

In order to use referenced sections, while still allowing your Markdown to render properly in places like Github or a blog, OLM allows you to include an alternate inline representation of a referenced asset by placing it between a pair of @no-outlearn and @yes-outlearn annotations.

```markdown
<!-- @asset, type: 'video/vimeo', title: 'Watch the Video', location: 'https://vimeo.com/61887298' -->

<!-- @no-outlearn -->

<iframe src="http://player.vimeo.com/video/61887298" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/61887298">Build Podcast 035 Capistrano</a> from <a href="https://vimeo.com/sayanee">Sayanee</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

<!-- @yes-outlearn -->

<!-- @section -->

## Following section

More content after video here.
```

