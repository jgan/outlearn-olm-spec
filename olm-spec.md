<!--
{
"name" : "outlearn-markdown-specification",
"version" : "0.3.2",
"title" : "Outlearn Markdown Specification",
"homepage" : "http://www.github.com/outlearn-content/outlearn-markdown-spec",
"freshnessDate": 2016-03-23,
"author" : "Will Koffel",
"license" : "CC BY",
"contact" : {"email": "will@outlearn.com"}
}
-->

<!-- @section -->

# Outlearn Markdown


> THIS IS A DRAFT DOCUMENT - WE WELCOME FEEDBACK AS THIS FORMAT EVOLVES - CURRENT AS OF MARCH 23, 2016


Outlearn Markdown (*OLM*) is a [Github-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/) compatible file format for easily creating content that imports directly to Outlearn.

OLM files are regular Markdown files, annotated with HTML comments in a special format that enriches the content with Outlearn learning features.

**Note:** this specification itself is published in OLM.  Look at the [raw markdown](https://raw.githubusercontent.com/outlearn-content/outlearn-olm-spec/master/olm-spec.md) to see the annotations for this file in context.

<!-- @section -->

# Authoring OLM

OLM can be easily authored from scratch, or by editing an existing Markdown file to include specific Outlearn annotations in the form of HTML comments.

## Header metadata for full Modules

OLM files defining a Module must have header metadata specified at the top.  Fields are specified using JSON-like syntax inside an HTML comment block.

```markdown
<!--
{
"name" : "learning-the-tango-steps",
"version" : "1.0",
"title" : "Learning the Tango Steps",
"homepage" : "http://tango.outlearn.com/",
"canonicalSource" : "http://tango.outlearn.com/steps",
"freshnessDate" : 2015-07-29,
"author" : "Dancing Doreen",
"license" : "CC BY",
"organization" : "Outlearn Dance Studios"
}
-->
```

See the full [OLP specification](https://www.outlearn.com/learn/outlearn/outlearn-publishing/4) for details.


<!-- @section -->

# Dividing Content into Sections

We strongly advise authors to break their content into bite-sized sections to make it easier for learners to navigate, consume, and track their learning progress.  To divide your OLM into navigable sections for a learner, you can add a `@section` annotation.

```markdown
# Introduction

Welcome to learning about ...

<!-- @section, "title": "Chapter 1 - Basics" -->

Let us get started with some fundamentals ...

<!-- @section, "title": "Chapter 2 - Advanced Topics" -->

Now that we are comfortable with the basics ...
```

Each section has a title, which appears in the table of contents and learning experience on Outlearn.

In the simplest form, `<!-- @section -->` can stand alone.  When you import your OLM file, the section will get assigned a title based on the first header tag (i.e. line starting with `#`) that is encountered in the section.  In the example above, the opening section will automatically inherit the title "Introduction".

You can also give the title directly as shown by specifying it in the `@section` annotation.

<!-- @section -->

# Task and Open Response Items

## Tasks

Users can engage with content through simple Task items.  A trackable checkbox can be created in your OLM content using the `@task` enrichment.

```markdown
<!-- @task, "text" : "Run the above code example on your own machine." -->
```

## Open Responses

An Open Response requires a deliverable to be submitted by a learner.  This can be useful when asking learners to do things like:

- author a simple 1-paragraph summary of what they've learned from watching a video
- describe three ways a specific technical strategy might be applicable in your codebase
- submit a pull request URL for a simple code modification in a hands-on lab exercise
- paste a code sample, algorithm analysis, or other small response to a question or task

```markdown
<!-- @openResponse, "text" : "Fork the repository above, fix the broken test, and submit a URL for your pull-request." -->
```

The example above gets rendered on Outlearn as follows:

<!-- @openResponse, "text" : "Fork the repository above, fix the broken test, and submit a URL for your pull-request."-->

<!-- @section -->

# Code Blocks

Markdown provides native support for code blocks.  If found, these code blocks will also get rendered by our syntax-highlighting library.  Code blocks using triple-back-tick are supported just like on Github, and will get automatic syntax-highlighting.

```javascript
function sum(a, b) {
  return a+b;
}
```  

Outlearn supports a special version of codeblocks allowing more than one language to be present for a particular code example.  Outlearn formats the variations as tabs, and when a user selects a variation, all the other code samples on that page also reflect their preference.

To use multi-tab code blocks, wrap a contiguous set of fenced code blocks (as shown above, and each fenced section needs a language specified) in between an `@codeBlock` and `@end` annotation:


``````markdown
<!-- @codeBlock -->


```javascript
function sum(a, b) {
  return a+b;
}
```

```ruby
def sum(a, b)
  a+b
end
```

<!-- @end -->
``````

Which will appear on Outlearn like:

<!-- @codeBlock -->

```javascript
function sum(a, b) {
  return a+b;
}
```

```ruby
def sum(a, b)
  a+b
end
```

<!-- @end -->

<!-- @section -->

# Multiple Choice Exercises

Simple multiple choice exercises include three components.  The _question_ (and any associated exposition), the _answers_, and an optional _hint_.

In OLM, we can define multiple choice questions by putting them between a `@multipleChoice` and an `@end` annotation.

```markdown
<!-- @multipleChoice -->

What is Node.js?

- [ ] A package manager for JavaScript packages
- [ ] A front-end framework for heavy-traffic web applications
- [X] A platform for scalable network applications

Remember that Node.js is more powerful than any individual use that it can be associated with.

<!-- @end -->
```

Use `- [ ]` to start an incorrect answer, and `- [X]` to start a correct answer.
The markdown above the answers is treated as the question, and the markdown below the answers
is used as a 'hint' or 'explanation' that your learners can choose to reveal. Any additional text after the “@end” will be ignored, so the user can make a note themselves of which asset they are closing with that tag.  For example `<!-- @end (first multiple choice) -->`

When used in an Outlearn Module, the above example appears as follows:

<!-- @multipleChoice -->

What is Node.js?

- [ ] A package manager for JavaScript packages
- [ ] A front-end framework for heavy-traffic web applications
- [X] A platform for scalable network applications

Remember that Node.js is more powerful than any individual use that it can be associated with.

<!-- @end -->

Multiple Choice answers support the full markdown syntax, but without the Outlearn extensions. Below are examples with code highlighting and embedded images. You can see the source code in the [GitHub repo](https://raw.githubusercontent.com/outlearn-content/outlearn-olm-spec/master/olm-spec.md).

<!-- @multipleChoice -->

### Operator Precedence

</br>

The following code snippet:

`var x = a + b * c + d;`

is equivalent to which of the answers below?

- [ ] `var x = (a + b) * (c + d);`
- [ ] `var x = a + ((b * (c + d));`
- [X] `var x = (a + (b * c)) + d;`
- [ ] `var x = ((a + b) * c) + d;`

Remember, `*` has higher precedence than `+`, so it will bind tighter.

<!-- @end -->

<!-- @multipleChoice -->

Which of these people created Linux?

- [ ] ![](http://upload.wikimedia.org/wikipedia/commons/thumb/0/01/Bill_Gates_July_2014.jpg/220px-Bill_Gates_July_2014.jpg)

  **Bill Gates**

- [X] ![](http://upload.wikimedia.org/wikipedia/commons/thumb/5/52/LinuxCon_Europe_Linus_Torvalds_03.jpg/220px-LinuxCon_Europe_Linus_Torvalds_03.jpg)

  **Linus Torvalds**

- [ ] ![](http://upload.wikimedia.org/wikipedia/commons/thumb/6/66/Guido_van_Rossum_OSCON_2006.jpg/200px-Guido_van_Rossum_OSCON_2006.jpg)

  **Guido van Rossum**

<!-- @end -->


<!-- @section -->

# Flexible Link Resources

When you want to link to a page outside of Outlearn, a regular Markdown link will work, and will open in a new browser tab.

If you want to give learners a great experience with external content, OLM provides a bunch of ways to enrich them with the `@resource` element.

The simplest example is to create a resource from a URL:

```markdown
<!-- @resource, "url" : "https://nodejs.org/" -->
```

At import-time, Outlearn will expand `@resource` enrichments to be a screenshot that links out to the original.  You can see the code above rendered below:

<!-- @resource, "url" : "https://nodejs.org/" -->

Instead of the default screenshot, you can customize the image used by specifying an `imageUrl` parameter, like this:

```markdown
<!-- @resource, "url" : "https://nodejs.org/", "imageUrl" : "http://f.cl.ly/items/1P1g3i1O050n3o0T0m24/nodejs-2560x1440.png" -->
```

<!-- @resource, "url" : "https://nodejs.org/", "imageUrl" : "http://f.cl.ly/items/1P1g3i1O050n3o0T0m24/nodejs-2560x1440.png" -->

## Special Resources

Outlearn has support for many custom renderings of resource URLs.  For example, if the resource URL is a YouTube video, it will render inline like this:

```markdown
<!-- @resource, "url" : "https://www.youtube.com/watch?v=RDfjXj5EGqI" -->
```

<!-- @resource, "url" : "https://www.youtube.com/watch?v=RDfjXj5EGqI" -->

Other URLs types you can try:

* GitHub repository URLs
* SlideShare presentations
* Google Docs
* PDFs
* (and much more...)

_Note_: for the curious, we use a combination of support from [Embedly](http://www.embed.ly/) and our own custom handlers to support these richer embedded links.

## Basic Resource Presentation

If screenshots and embeds are not working well for your resource, or you prefer a simpler presentation, resources can also be rendered in a _Basic_ mode which provides plenty of customization.

Basic presentation will be done automatically if no screenshot is available for a link.  But you can force basic presentation with the `"forceBasic": true` option.  For example, in order to link out to a YouTube video instead of embedding it:

```markdown
<!-- @resource, "url" : "https://www.youtube.com/watch?v=RDfjXj5EGqI", "forceBasic": true -->
```

<!-- @resource, "url" : "https://www.youtube.com/watch?v=RDfjXj5EGqI", "forceBasic": true -->

For further customization, you can also override the title, image, and description used for the link. This can be done with the attributes `"title"`, `"imageUrl"`, and `"description"` as follows:

```markdown
<!-- @resource, "url" : "https://nodejs.org", "forceBasic" : true, "title": "Official Node.js site", "imageUrl" : "http://code-maven.com/img/node.png", "description": "Node.js is a JavaScript runtime which uses an event-driven, non-blocking I/O model that makes it lightweight and efficient." -->
```

The link will render like this:

<!-- @resource, "url" : "https://nodejs.org", "forceBasic" : true, "title": "Official Node.js site", "imageUrl" : "http://code-maven.com/img/node.png", "description": "Node.js is a JavaScript runtime which uses an event-driven, non-blocking I/O model that makes it lightweight and efficient." -->


<!-- @section -->

# Embedding Images and Videos

Outlearn supports the regular Markdown syntax for including images.

```markdown
![sea](https://raw.githubusercontent.com/outlearn-content/outlearn-modules/master/assets/sea.jpg)
```

![sea](https://raw.githubusercontent.com/outlearn-content/outlearn-modules/master/assets/sea.jpg)

Video assets hosted on YouTube and Vimeo are supported via the `@resource` tag.

```markdown
<!-- @resource, "url": "https://vimeo.com/67325705" -->

<!-- @resource, "url": "https://www.youtube.com/watch?v=CmjeCchGRQo" -->
```

Remember to to use `https` and not `http` when specifying the video URL. Otherwise some browsers will not show it.


<!-- @section, -->

# Highlight boxes

You can create highlight boxes using the block quote syntax from Markdown.

```markdown
> Important message for everyone to see.
```

This turns into

> Important message for everyone to see.

<!-- @section -->

# Examples of OLM

For many examples of content in OLM and OLP format, please refer to the [Outlearn Content](http://www.github.com/outlearn-content) repo on Github.
