<!--
name: outlearn-markdown-specification
version : 0.2.0
title : "Outlearn Markdown Specification"
description: "OLM (Outlearn Markdown) is an annotated, markdown-compatible text format for importing simple learning content as Outlearn modules.  This specification describes in plain terms how to easily author OLM files."
homepage : "http://www.github.com/outlearn-content/outlearn-markdown-spec"
author : "Will Koffel"
license : "Public",
contact : "will@outlearn.com"
-->

<!-- @section -->

# Outlearn Markdown

> THIS IS A DRAFT DOCUMENT - WE WELCOME FEEDBACK AS THIS FORMAT EVOLVES - CURRENT AS OF April 17, 2015

Outlearn Markdown (*OLM*) is a [Github-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/) compatible file format for easily creating content that imports directly to Outlearn.

OLM files are regular Markdown files, annotated with HTML comments in a special format that dicates metadata and structure to the Outlearn learning system.

**Note:** this specification itself is published in OLM.  Look at the raw markdown to see the annotations for this file in context.

## Limitations and Recommendations

OLM is *not* a full-featured replacement for [OLP, Outlearn's official package format](http://www.github.com/outlearn-content/outlearn-package-spec).  Limitations of OLM include:

* OLM is a single file format.  One text file contains all the content and meta-data.  If the amount of content is too large or diverse to naturally fit in a single file, the OLP package format is likely a better fit.
* Only a single module may be defined in an OLM file. An OLP package may define and import multiple learning modules at once.
* All assets (e.g. videos and images) must be hosted remotely and referenced within the OLM asset annotations.  This means that all referenced assets must be publicly available.  OLP allows bundled assets, including private videos and images.

If you have great existing Markdown content, OLM is a convenient way to get even more out of it through simple annotations that give it new life in the Outlearn catalog.  If you are authoring learning materials from scratch, we recommend using the richer OLP approach for anything but simple, single-file modules with a handful of sections.

### More on OLP

OLM provides a subset of the full functionality of [Outlearn Packages (OLP)](http://www.github.com/outlearn-content/outlearn-package-spec). The official Outlearn package format is very easy to author by hand as well.  In the simplest cases, it consists of nothing more than a small JSON file that references assets in the same directory, like images, videos, code snippets, markdown files, or even OLM files.

OLM is functionally pre-processed into OLP before importing, so it will always be a subset of the OLP format.

For more detailed options that OLM supports, it may be helpful to reference analagous sections in the OLP specification.

### Naming OLM Files

Outlearn will import OLM files that have an extension of `.olm` or an extension of `.md`.  We recommend `.olm` except in cases where `.md` is needed for compatibility with other systems that may require the `.md` suffix to properly render Markdown (notably Github).

<!-- @section -->

# Authoring OLM

OLM can be easily authored from scratch, or by editing an existing Markdown file to include specific Outlearn annotations in the form of HTML comments.

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

### Value Fields

Most metadata fields are regular values.  The name is unquoted, the value is optionally quoted.

```markdown

<!--
title: "My Great Content"
-->

```

### Array Fields

Some fields, like `author` may contain more than one value.  In this case, a JSON-style array may be specified.

```markdown

<!--
author: ["Bob", "Alice", "Steve", "Mary", "Supermegacorp, Inc."]
-->

```

## Annotating Module Sections

As per the [OLP Specification](http://www.github.com/outlearn-content/outlearn-package-spec), a module has one or more `sections`.  Each section contains Markdown content, i.e. all the actual text of your learning material.

Your OLM file is broken into sections using the annotations described below.

### Section Basics

We strongly advise authors to break their content into bite-sized sections to make it easier for learners to navigate, consume, and track their learning progress.  To divide your Outlearn Markdown into navigatable sections for a learner, you can add a `@section` annotation.

```markdown

<!-- @section -->

# Introduction

<!-- @section, title: 'Chapter 1 - Basics' -->

# Chaper 1

<!-- @section, title: 'Chapter 2 - Advanced Topics' -->

# Additional Materials

```

Each section has a title, which appears in the table of contents and learning experience on Outlearn.

In the simplest form, `<!-- @section -->` can stand alone.  When you import your OLM file, the section will get assigned a title based on the first header tag (line starting with `#`) that is encountered in the section.  In the example above, the opening section will automatically inhereit the title "Introduction".

You can override the title as shown by specifying it in the `@section` annotation.

### Remote Asset Annotations

Some assets (like images) can natively be included in Markdown.  That will work, and will render acceptably in the Outlearn learning experience.  However, by declaring an asset in its own annotation, it gets benefits, such as nicer rendering and better progress tracking.  OLM provides support for a growing number of referenced asset types, described below.

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

<!-- @section, type: 'audio/soundcloud', title: 'Uptown Funk', location: 'https://soundcloud.com/mmmusic/mark-ronson-uptown-funk' -->

<!-- @section, type: 'audio/mpeg', title: 'Watch the Video', location: 'http://www.podtrac.com/pts/redirect.mp3/twit.cachefly.net/audio/twit/twit0497/twit0497.mp3' -->

```

#### Image Asset

```markdown

<!-- @section, type: 'image/jpeg', title: 'Architecture Diagram', location: 'http://ad009cdnb.archdaily.net/wp-content/uploads/2011/05/1304980266-ad30-circulation-diagram.jpg' -->

```

### @no-outlearn annotations

In order to use referenced sections, while still allowing your Markdown to render properly in places like Github or a blog, OLM allows you to include an alternate inline representation of a referenced asset.

```markdown

<!-- @asset, type: 'video/vimeo', title: 'Watch the Video', location: 'https://vimeo.com/61887298' -->

<!-- @no-outlearn -->

<iframe src="http://player.vimeo.com/video/61887298" width="500" height="281" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe> <p><a href="https://vimeo.com/61887298">Build Podcast 035 Capistrano</a> from <a href="https://vimeo.com/sayanee">Sayanee</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

<!-- @section -->

## Following section

More content after video here.

```

An `@no-outlearn` section will end when the parser encounters either an `@section` annotation, or a `@yes-outlearn` annotation.

<!--  @section -->

# Importing OLM

To import an OLM file to Outlearn, put it in a Github repo that is public or that you are authorized to access.  Log in to Outlearn with your Github credentials (or link your Github account under *Settings*), and navigate to *Import Module*, where you can specify the Github location for your OLM file for immediate import into your Outlearn account.

Many more details on import are coming, including information about a command-line API for importing, and imports from additional web-based sources.

<!--  @section -->

# Example OLM

For many examples of content in OLM and OLP format, please refer to the [Outlearn Content](http://www.github.com/outlearn-content) repo on Github.
