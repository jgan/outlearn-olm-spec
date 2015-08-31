<!--
{
"name" : "outlearn-markdown-specification",
"version" : "0.2.0",
"title" : "Outlearn Markdown Specification",
"description": "OLM (Outlearn Markdown) is an annotated, markdown-compatible text format for importing simple learning content as Outlearn modules.",
"homepage" : "http://www.github.com/outlearn-content/outlearn-markdown-spec",
"freshnessDate": 2015-07-29,
"author" : "Will Koffel",
"license" : "CC BY",
"contact" : {"email": "will@outlearn.com"}
}
-->

<!-- @section -->

# Outlearn Markdown

> THIS IS A DRAFT DOCUMENT - WE WELCOME FEEDBACK AS THIS FORMAT EVOLVES - CURRENT AS OF JULY 29, 2015

Outlearn Markdown (*OLM*) is a [Github-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/) compatible file format for easily creating content that imports directly to Outlearn.

OLM files are regular Markdown files, annotated with HTML comments in a special format that enriches the content with Outlearn learning features.

**Note:** this specification itself is published in OLM.  Look at the [raw markdown](https://raw.githubusercontent.com/outlearn-content/outlearn-olm-spec/master/olm-spec.md) to see the annotations for this file in context.

## OLM as a full Module definition

OLM is primarily used to enrich content, but in many cases it can be used to define a whole Learning Module.  The next section describes how to author Modules using OLM.

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
"name" : "learning-the-tango-steps",
"version" : "1.0",
"title" : "Learning the Tango Steps",
"description": "An introduction to the steps of the tango, including a video lesson.",
"homepage" : "http://tango.outlearn.com/",
"canonicalSource" : "http://tango.outlearn.com/steps",
"freshnessDate" : 2015-07-29,
"author" : "Dancing Doreen",
"license" : "CC BY",
"organization" : "Outlearn Dance Studios"
}
-->
```

See the full [OLP specification](https://pilot.outlearn.com/learn/outlearn/outlearn-publishing/4) for details.


<!-- @section -->

# Dividing Content into Sections

We strongly advise authors to break their content into bite-sized sections to make it easier for learners to navigate, consume, and track their learning progress.  To divide your OLM into navigable sections for a learner, you can add a `@section` annotation.

<div class="highlight markdown"><table style="border-spacing: 0"><tbody><tr><td class="gutter gl" style="text-align: right"><pre class="lineno">1
2
3
4
5
6
7
8
9
10
11
12
13</pre></td><td class="code"><pre> <span class="nb">&lt;</span>!-- @section --&gt;

# Introduction

Welcome to learning about ...

 <span class="nb">&lt;</span>!-- @section, "title": "Chapter 1 - Basics" --&gt;

Let us get started with some fundamentals ...

 <span class="nb">&lt;</span>!-- @section, "title": "Chapter 2 - Advanced Topics" --&gt;

Now that we are comfortable with the basics ...
</pre></td></tr></tbody></table>
</div>




Each section has a title, which appears in the table of contents and learning experience on Outlearn.

In the simplest form, `<!-- @section -->` can stand alone.  When you import your OLM file, the section will get assigned a title based on the first header tag (line starting with `#`) that is encountered in the section.  In the example above, the opening section will automatically inherit the title "Introduction".

You can also give the title as shown by specifying it in the `@section` annotation.

<!-- @section -->

# Task Items

Users can engage with content through simple Task items.  A trackable checkbox can be created in your OLM content using the `@task` enrichment.


<div class="highlight markdown"><table style="border-spacing: 0"><tbody><tr><td class="gutter gl" style="text-align: right"><pre class="lineno">1</pre></td><td class="code"><pre><span class="nv">&lt;!-- @task, "text" : "Run the above code example on your own machine." --&gt;</span>
</pre></td></tr></tbody></table>
</div>

## Requiring a Deliverable

Your Task item can optionally require a deliverable to be submitted by a learner.  This can be useful when asking learners to do things like:

- author a simple 1-paragraph summary of what they've learned from watching a video
- describe three ways a specific technical strategy might be applicable in your codebase
- submit a pull request URL for a simple code modification in a hands-on lab exercise
- paste a code sample, algorithm analysis, or other small response to a question or task

<div class="highlight markdown"><table style="border-spacing: 0"><tbody><tr><td class="gutter gl" style="text-align: right"><pre class="lineno">1</pre></td><td class="code"><pre><span class="nv">&lt;!-- @task, "hasDeliverable" : true, "text" : "Fork the repository above, fix the broken test, and submit a URL for your pull-request." --&gt;</span>
</pre></td></tr></tbody></table>
</div>

The example above gets rendered on Outlearn as follows:

 <!-- @task, "hasDeliverable" : true, "text" : "Fork the repository above, fix the broken test, and submit a URL for your pull-request."-->

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

The JavaScript code shown above will be rendered by Outlearn as below:

```javascript
function sum(a, b) {
  return a+b;
}
```  

<!-- @section -->

# External Links

For content that you want to link to off Outlearn, a regular Markdown link will work, and will open in a new browser tab.

For a great experience on important external links, OLM provides an `@link` enrichment.

<div class="highlight markdown"><table style="border-spacing: 0"><tbody><tr><td class="gutter gl" style="text-align: right"><pre class="lineno">1</pre></td><td class="code"><pre><span class="nv">&lt;!-- @link, "url" : "https://nodejs.org/", "text": "Install NodeJS" --&gt;</span>
</pre></td></tr></tbody></table>
</div>

At import-time, all `@link` enrichments will be expanded to include a title, summary, and image.  

If a link can be embedded in an iframe, it will be seamless integrated with the Outlearn learning environment.  Otherwise, it will open in a new browser tab.

An optional `task` attribute can be included, as seen above, which will create a Todo item automatically attached to this external link.

<!-- @section -->

# Multiple Choice Exercises

Simple multiple choice exercises include three components.  The _question_ (and any associated exposition), the _answers_, and an optional _hint_.

In OLM, we can define multiple choice questions by putting them between a `@multipleChoice` and an `@end` annotation.

<div class="highlight markdown"><table style="border-spacing: 0"><tbody><tr><td class="gutter gl" style="text-align: right"><pre class="lineno">1
2
3
4
5
6
7
8
9
10
11
12</pre></td><td class="code"><pre><span class="nb">&lt;!-- @multipleChoice --&gt;</span>

What is Node.js?
<span class="p">
-</span> <span class="p">[</span><span class="nv"> </span><span class="p">]</span> A package manager for JavaScript packages
<span class="p">-</span> <span class="p">[</span><span class="nv"> </span><span class="p">]</span> A front-end framework for heavy-traffic web applications
<span class="p">-</span> <span class="p">[</span><span class="nb">X</span><span class="p">]</span> A platform for scalable network applications

Remember that Node.js is more powerful than any individual use that it can be associated with.

<span class="nb">&lt;!-- @end --&gt;</span>
</pre></td></tr></tbody></table>
</div>

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
</br>
`var x = a + b * c + d;`
</br>

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

# Embedding Images and Videos

Outlearn supports the regular Markdown syntax for including images.

```markdown
![sea](https://raw.githubusercontent.com/outlearn-content/outlearn-modules/master/assets/sea.jpg)
```

Video assets hosted on YouTube and Vimeo are supported via the `@asset` tag.

<div class="highlight markdown"><table style="border-spacing: 0"><tbody><tr><td class="gutter gl" style="text-align: right"><pre class="lineno">1
2
3</pre></td><td class="code"><pre><span class="nv">&lt;!-- @asset, "contentType": "outlearn/video", "provider": "vimeo", "url": "https://player.vimeo.com/video/67325705" --&gt;</span>

<span class="nv">&lt;!-- @asset, "contentType": "outlearn/video", "provider": "youtube", "url": "https://www.youtube.com/embed/CmjeCchGRQo" --&gt;</span>
</pre></td></tr></tbody></table>
</div>

Remember to to use `https` and not `http` when specifying the video URL. Otherwise some browsers will not show it. Note that Outlearn uses the embed URLs. Here is how you extract the video ID from the regular URL and turn it into the embed URL.


| Video Provider | Vimeo | YouTube |
|-----------|----------|--------|
| Regular URL | https://vimeo.com/67325705 | https://www.youtube.com/watch?v=CmjeCchGRQo  |
| Video ID | 67325705 | CmjeCchGRQo |
| Embed URL | https://player.vimeo.com/video/67325705 | [https://www.youtube.com/embed/ CmjeCchGRQo](https://www.youtube.com/embed/CmjeCchGRQo)  |





Where possible, Outlearn will try to play the video in our preferred player, which includes advanced features for learners like keyboard controls, skip-back, multiple size options, and accessibility features.


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
