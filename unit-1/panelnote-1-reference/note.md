---
order: 4
---

# Tutors Reference

[[toc]]

## Cards

---

The card metaphor is used throughout tutors as a simple visual feature to represent a variety of learning resources. In general the contents of a card are extracted from the following:

- A Markdown file, containing resource name + summary
- An image, in png, jpg, gif or svg formats.

These resources are typically named to match your context, and are contained in a folder whose name is structured to encode the type of learning resource.

## Semantic Resource Names

---

### Folders Names

Folder names convey the type of learning resource, with the first letters determining its type. Folders starting with the following names have a significance in Tutors:

|  Example                                                                                     | Source |
| -------------------------------------------------------------------------------------------- | ------------------------------------------  |
| [topic](https://reader.tutors.dev/topic/reference-course/topic-01-typical)                   | [Top level course topic ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical)|
| [unit](https://reader.tutors.dev/topic/reference-course/topic-01-typical)                    | [Group of learning objects within a topic  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1)|
| [side](https://reader.tutors.dev/topic/reference-course/topic-02-side)                       | [Group of learning objects framed in a sidebar  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-02-side/side-unit)|
| [archive](https://reader.tutors.dev/wall/archive/reference-course)                           | [Downloadable zip file of resources  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-07-reference/archive)|
| [book](https://reader.tutors.dev/lab/reference-course/topic-01-typical/unit-1/book-a)        | [Step by step lab instructions, authored in markdown  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/book-a)|
| [github](https://reader.tutors.dev/wall/github/reference-course)                             | [Link to a GitHub repository  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical)|
| [note](https://reader.tutors.dev/note/reference-course/topic-01-typical/unit-2/note-1)       | [Single web page, authored in markdown ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/note-1)|
| [panelvideo](https://reader.tutors.dev/topic/reference-course/topic-03-media)                | [A full screen width video, hosted in YouTube or HEANet](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/panelvideo-1)|
| [paneltalk](https://reader.tutors.dev/topic/reference-course/topic-05-panel-talk)            | [Full screen width  presentation in pdf format    ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-05-panel-talk/paneltalk)|
| [panelnote](https://reader.tutors.dev/topic/reference-course/topic-04-panel-note)            | [Full screen width note](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-04-panel-note/panelnote) |
| [talk](https://reader.tutors.dev/talk/reference-course/topic-01-typical/unit-1/talk-1-intro) | [Standard presentation in pdf format  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/talk-1-intro)|
| [web](https://reader.tutors.dev/wall/web/reference-course.netlify.app)                       | [Link to an external web site  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/web-1) |

 To sort the name alphabetically you may append numerals. To enhance meaning, append contextual keywords. For example:

| Folder Name            |
| ---------------------- |
| topic-01-introduction  |
| topic-02-learning-html |

For all file & folder names, avoid spaces within a file name.

| Do                     | Don't
| ---------------------- | -------------------- |
| topic-01-introduction  | Topic 01 Introduction
| topic-02-learning-html | Topic 02 Learning HTML

### File Names

Each resource will typically have the following files:

| File                   | Purpose                                                      |
| ---------------------- | ------------------------------------------------------------ |
| some-resource-name.md  | A markdown file, typically containing title + short summary for the resource |
| some-resource-name.png | Image to he used for the resource. Name must be the same as the .md file, image can be .png, .jpg. .jpeg, .gif |

The following filenames are reserved:

| File name       | Description                   |
| --------------- | ----------------------------- |
| course.md       | Title for course (mandatory)  |
| properties.yaml | Course properties (mandatory) |
| course.png      | Course image                  |
| calendar.yaml   | Course calendar               |
| enrolment.yaml  | Student enrolment file        |
| weburl          | link to external web site     |
| videoid         | id of  external video         |
| githubid        | link to github repo           |

## Course Structure

---

| Example Resource | Display | 
| ---------------- | ------- | 
| [reference-course](https://github.com/tutors-sdk/tutors-reference-course) | [Reference Course](https://reader.tutors.dev/course/reference-course) |

The minimum requirements for a course are a folder containing these three files:

| File name      | Purpose      |  
| -------------- | ------------ | 
| course.md      | Course title + general course information
| course.png     | Course image
| properies.yaml | Course properties

### course.md

A markdown file, structured as follows:

~~~markdown
Course Title

Course information - a course outline, description or any other information. Can be any length. Will appear as slide over if the user presser the Info button on the top left.
~~~

### properties.yaml

Course metadata in yaml format. At a minimum, this must contain the following:

~~~yaml
credits: The course author(s) or organisation
~~~

The credits property will appear as a subtitle in the course title bar.

There are a range of other optional properties. See later in this document for a complete list.


## Topic

---

| Example Resource | Display | 
| ---------------- | ------- | 
| [topic-01-typical](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical) | [Typical Topic](https://reader.tutors.dev/topic/reference-course/topic-01-typical) |

Top level learning object for a course. Typically encapsulating a session or week of learning material.


| Files  | Purpose  |
| --------------- | ------------ |
| topic-title.md  | Topic title + summary. Any file name, file type must be .md file type |
| topic-title.png | Image for topic. File name must be same as .md file. File type can be .png, .jpg, or .jpeg|

### topic.md

The title and subtitle are extracted from the .md file, for example:

~~~~markdown
Simple

Units with presentations, labs + resources
~~~~

In addition to the title, subtitle + image specified the file, the topic can contain any number of units (see below) or other learning objects.

## Unit

---


| Example Resource | Display | 
| ---------------- | ------- | 
| [unit-1](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1) | [Main Lesson](https://reader.tutors.dev/topic/reference-course/topic-01-typical) |

A unit will encapsulate learning resources, framed by a title

| Files          | Purpose |
| -------------- | ------- |
| main-lesson.md | Title for the unit. The file can have any suitable name, but must be .md file type |


The title is specified in a single markdown file:

### main-lesson.md

~~~markdown
Main Lesson
~~~

Units contain any number of learning resources.

## Side

---

| Example Resource | Display | 
| ---------------- | ------- | 
| [side-unit](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-02-side/side-unit) | [Labs for this Topic](https://reader.tutors.dev/topic/reference-course/topic-02-side) |

A side will encapsulate learning resources, framed by a title. It is equivalent to a Unit (see above), but framed within a side bar.

- [Example](https://reader.tutors.dev/topic/reference-course/topic-02-side) 
- [Source ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-02-side/side-unit)

| File name     | Purpose |
| ------------- | ------- |
| topic-labs.md | Title for the side bar The file can have any suitable name, but must be .md file type |


The title is specified in a single markdown file

### topic-labs.md

~~~markdown
Labs for this Topic
~~~

Side bar cam contain any number of learning resources.

## Learning Resources

---

### Types

There are 2 broad types of learning resources

- Card Resources
- Panel Resources

### Card Resources

These resources are represented by simple cards that can appear in a topic or unit:

| Example Resource | Display | Cards |
| ---------------- | ------- | ----- |
| [Standard presentation in pdf format](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/talk-1-intro)             | [Lecture 1](https://reader.tutors.dev/talk/reference-course/topic-01-typical/unit-1/talk-1-intro) | [Talks](https://reader.tutors.dev/wall/talk/reference-course) |
| [Single web page, authored in markdown ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/note-1)                | [Note 1](https://reader.tutors.dev/note/reference-course/topic-01-typical/unit-2/note-1)          | [Notes](https://reader.tutors.dev/wall/note/reference-course) | 
| [Step by step lab instructions, authored in markdown  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/book-a) | [Lab 1](https://reader.tutors.dev/lab/reference-course/topic-01-typical/unit-1/book-a)            | [Labs](https://reader.tutors.dev/wall/lab/reference-course) |
| [Link to a web site  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/web-1)                                   | [Web Site](https://tutors.dev)                                                                    | [Web Links](https://reader.tutors.dev/web/talk/reference-course) |
| [Downloadable zip file of resources  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-07-reference/archive)                      | [Archive 1](https://reader.tutors.dev/wall/archive/reference-course)                              | [Archives](https://reader.tutors.dev/archive/talk/reference-course) |
| [Link to a GitHub repository  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical)                                       | [Github Repo 1](https://github.com/tutors-sdk/tutors)                                             | [Repos](https://reader.tutors.dev/wall/repo/reference-course) | 

### Panel Resources

Panels appear directly in a unit or topic, and are not represented by a separate card. Instead, their contents are rendered directly on to the parent topic/unit.

| Example Resource | Display | 
| ---------------- | ------- | 
| [A full screen width video, hosted in YouTube or HEANet](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/panelvideo-1) | [Main Video](https://reader.tutors.dev/topic/reference-course/topic-03-media)     |
| [Full screen width  presentation in pdf format    ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-05-panel-talk/paneltalk)    | [Main Talk](https://reader.tutors.dev/topic/reference-course/topic-05-panel-talk) | 
| [Full screen width note](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-04-panel-note/panelnote)                               | [Main Note](https://reader.tutors.dev/topic/reference-course/topic-04-panel-note) | 

## Talk 

---

| Example Resource | Display | Cards |
| ---------------- | ------- | ----- |
| [Standard presentation in pdf format](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/talk-1-intro) | [Lecture 1](https://reader.tutors.dev/talk/reference-course/topic-01-typical/unit-1/talk-1-intro) | [Talks](https://reader.tutors.dev/wall/talk/reference-course) |


A talk is a PDF presentation, document or other pdf formatted resource. The pdf file, markdown description and image file must all have the same file name, which can be whatever you choose. 

| File name        | Purpose                                                      |
| ---------------- | ------------------------------------------------------------ |
| introduction.md  | Title + short summary for the talk. The file can have any suitable name, but must be .md file type |
| introduction.png | Image for card. File name must be same as .md file. File type can be .png, .jpg, or .jpeg |
| introduction.pdf | Pdf to be rendered if the card is selected. The name must match the .md file name precicely |

The .md file provides the card title + subtitle:

~~~markdown
Lecture 1

A short summary of the talk, no more than two sentences.
~~~

## Note 

---

| Example Resource | Display | Cards |
| ---------------- | ------- | ----- |
| [Single web page, authored in markdown ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/note-1) | [Note 1](https://reader.tutors.dev/note/reference-course/topic-01-typical/unit-2/note-1) | [Notes](https://reader.tutors.dev/wall/note/reference-course) | 

A note is a full web page, authored in markdown using the basic syntax:

- <https://www.markdownguide.org/basic-syntax/>

If you choose to include images or links to zipped archives you wish to be distributed with the note, you must include these in the note folder:

| Resource   | Purpose  |
| ---------- | -------- |
| note.md    | The content of the note. Any suitable name, type must be .md |
| note.png   | Image for card. File name must be same as .md file. File type can be .png, .jpg, or .jpeg |
| img        | A folder containing Images used by the note |
| archives   | A folder contains any zipped archives referred to in the note |


Image links can be structured in include relative references to the image. E.g:

![](img/img-link.png)

The linked images must be included in the img folder in the note resource. Similairly, if you wish to distributed a zipped archive of learning resources, include the zip file(s) in the archives folder, and link like this:

![](img/zip-link.png)

Links to external resources can be included with conventional web links.


## Book 

---

| Example Resource | Display | Cards |
| ---------------- | ------- | ----- |
| [Step by step lab instructions, authored in markdown  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1/book-a) | [Lab 1](https://reader.tutors.dev/lab/reference-course/topic-01-typical/unit-1/book-a) | [Labs](https://reader.tutors.dev/wall/lab/reference-course) |


A series of steps/instructions, authored in Markdown, authored in markdown using the basic syntax:

- <https://www.markdownguide.org/basic-syntax/>


Each step in the lab is held in a separate markdown file. The step will have a full title, extracted from the first line of the step file. It will also have a short title, used for constrained (mobile) screen widths.

The step files are named with three dot separated segments as follows:

- [sort-key].[short-title].md

Where:

- [sort-key] ensures the step sequential position, sorted alphabetically/numerically. Typlicaly you might use a sequence like: 01, 02, 03, 04 etc...
- [short-titie] is a title used for constrained screen widths

For example:

- 00.Lab-01.md
- 01.01.md
- 02.02.md
- 03.03.md

The title for the lab card is extracted from the short title for the first step. In the above example, the lab navigation side bar would be:

- Lab-01
- 01
- 02
- 03

Image links can be structured in include relative references to the image. E.g:

![](img/img-link.png)

The linked images must be included in the img folder in the lab. Similairly, if you wish to distributed a zipped archive of learning resources, include the zip file(s) in the archives folder, and link like this:

![](img/zip-link.png)

Links to external resources can be included with conventional web links.

### Auto Numbering

You may prefer all steps in all your labs to be autonumbered. This will preppend a number, starting at 01, to all steps

~~~
labStepsAutoNumber: true
~~~

This is independent of the 'sort-key' segment in the lab step name.

## Web 

---

| Example Resource | Display | Cards |
| ---------------- | ------- | ----- |
| [Link to a web site  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-2/web-1) | [Web Site](https://tutors.dev) | [Web Links](https://reader.tutors.dev/web/talk/reference-course) |

A simple link to an external web resource.

| Resource   | Purpose  |
| ---------- | -------- |
| web-link.md  | Title + short summary for the link. The file can have any suitable name, but must be .md file type |
| web-link.png | Image for card. File name must be same as .md file. File type can be .png, .jpg, or .jpeg |
| weburl | the full url for the resource |

## Archive  

| Example Resource | Display | Cards |
| ---------------- | ------- | ----- |
| [Downloadable zip file of resources  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-07-reference/archive) | [Archive 1](https://reader.tutors.dev/wall/archive/reference-course) | [Archives](https://reader.tutors.dev/archive/talk/reference-course) |

A link to a downloadable archive (zip). The archive is bundled with the course source and hosted with the course resources.

| Resource   | Purpose  |
| ---------- | -------- |
| archvive.md  | Title + short summary for the archive. The file can have any suitable name, but must be .md file type |
| archive.png | Image for card. File name must be same as .md file. File type can be .png, .jpg, or .jpeg |
| archive.zip | The archive that will be downloaded if the card selected |

## Github 

| Example Resource | Display | Cards |
| ---------------- | ------- | ----- |
| [Link to a GitHub repository  ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical) | [Github Repo 1](https://github.com/tutors-sdk/tutors) | [Repos](https://reader.tutors.dev/wall/repo/reference-course) | 


A link to an GitHub repository.

| Resource   | Purpose  |
| ---------- | -------- |
| github.md  | Title + short summary for the GitHib repo. The file can have any suitable name, but must be .md file type |
| github.png | Image for card. File name must be same as .md file. File type can be .png, .jpg, or .jpeg |
| githubid   | Full url of the repo |

## Panelvideo 

| Example Resource | Display | 
| ---------------- | ------- | 
| [A full screen width video, hosted in YouTube or HEANet](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/panelvideo-1) | [Main Video](https://reader.tutors.dev/topic/reference-course/topic-03-media) |

A video to be displayed directly on the topic or unit resource.

| Resource   | Purpose  |
| ---------- | -------- |
| video.md   | Title for the video. The file can have any suitable name, but must be .md file type |
| videoid    | id of the video |

See video section below for format of this id.

## Paneltalk 

| Example Resource | Display | 
| ---------------- | ------- | 
| [Full screen width  presentation in pdf format    ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-05-panel-talk/paneltalk) | [Main Talk](https://reader.tutors.dev/topic/reference-course/topic-05-panel-talk) | 

A PDF document to be displayed directly on the topic or unit resource.

| Resource   | Purpose  |
| ---------- | -------- |
| talk.md   | Title for the document. The file can have any suitable name, but must be .md file type |
| talk.pdf  | The .PDF to display. Its name must be the same as the .md file |


## Panelnote 

| Example Resource | Display | 
| ---------------- | ------- | 
| [Full screen width note](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-04-panel-note/panelnote) | [Main Note](https://reader.tutors.dev/topic/reference-course/topic-04-panel-note) | 

A panel note is a full web page, authored in markdown using the basic syntax:

- <https://www.markdownguide.org/basic-syntax/>

The note's contents is rendered directly onto the parent topic or unit.

| Resource   | Purpose  |
| ---------- | -------- |
| note.md    | The content of the note. Any suitable name, type must be .md |
| img        | A folder containing Images used by the note |
| archives   | A folder contains any zipped archives referred to in the note |


Image links can be structured in include relative references to the image. E.g:

![](img/img-link.png)

The linked images must be included in the img folder in the note resource. Similairly, if you wish to distributed a zipped archive of learning resources, include the zip file(s) in the archives folder, and link like this:

![](img/zip-link.png)

Links to external resources can be included with conventional web links.


## Videos

Panelvideos above are the primary mechanism for displaying videos prominently. However, most learnings objects can have videos associated with them by dropping in a single file into the Learning Resource folder called:

- videoid

Videos are usually hosted on Youtube - and you will need a link to the video in order to embed in a tutors course. A typical YouTube URL looks like this:

~~~url
https://www.youtube.com/watch?v=Hfw1lbErjws
~~~

You will typically use the video id:

~~~
Hfw1lbErjws
~~~

... which is the last string after the `v=`. This is the contents of the videoid file.

However, this videoid file can be dropped into most Learningg Resources. If a resource has this file, then a video play button will feature in the Card for the resource, which will trigger the video player. 


### Video Talks

| Example Resource | Display | 
| ---------------- | ------- | 
| [Video Only Talk](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/unit-1/talk-3) | [Lecture 7]https://reader.tutors.dev/video/reference-course/topic-03-media/unit-1/talk-3/x09E7b2ESE8?start=1068&1370) | 

For Talk resources only, it is possible to drop the PDF completely and just include the video only. The card will display as with a PDF 



### Video Chapters

| Example Resource | Display | 
| ---------------- | ------- | 
| [Video Chapter]](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/unit-1/talk-2)| [Lecture 6](https://reader.tutors.dev/video/reference-course/topic-03-media/unit-1/talk-2/x09E7b2ESE8?start=106&286)) | 


Youtube supports start/end times when linking to a video. This is via appending a start/end in seconds to the video id:

~~~bash
x09E7b2ESE8?start=106&286
~~~

On youtube, this typically works best if the Youtube description also contains chapter information. Of example, on YouTube Studio, chapters information can be included by inforporating start / end times into the video description. For example:

~~~bash
Introduction 00:00:00
Svelte Core Concepts 00:02:54
Svelte Components 0:19:49
Donation App 00:27:55
Donation-Hapi V1 00:34:24
~~~

For the above, the associated videoid files for each chapter would look like this:

~~~bash
9Srf_ydMdL0?start=0&174
9Srf_ydMdL0?start=174&1189
9Srf_ydMdL0?start=1189&1675
9Srf_ydMdL0?start=1675&2064
9Srf_ydMdL0?start=2064&6348
~~~

### HEANet Hosted Videos

| Example Resource | Display | 
| ---------------- | ------- | 
| [Heanet Video]](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/unit-2-heanet)| [HeaNet video example](https://reader.tutors.dev/topic/reference-course/topic-03-media)) | 

If you require an alternative to Youtube, Tutors support videos hosted on the HEANet media service:

- <https://media.heanet.ie/>

If you wish to use this service, then your videoid file should look like this:

~~~bash
heanet=7e4f1e9afedb40d5996d0703702eaaa4
~~~

The id will be generated when you upload the video to the HEAnet media service.


## Latex

## Ordering Learning Resources

| Example Resource | Display | 
| ---------------- | ------- | 
| [Ordering](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-08-ordering)| [Ordering Example](https://reader.tutors.dev/topic/reference-course/topic-08-ordering) | 

For a topic, unit or side resources, the ordering of the cards is as follows:

- talk
- lab
- note
- web
- github
- archive

This can be customised via the introduction of [FrontMatter](https://docs.zettlr.com/en/core/yaml-frontmatter/) sections in the corresponding markdown files. This should contain an "order" number, which dictates the sequenceing for the cards.

~~~yaml
---
order: 1
---
~~~

If the resouce is a Lab, then the first step should include the FrontMatter/order paramater.

## SVG Icons

| Example Resource | Display | 
| ---------------- | ------- | 
| [Icon based cards]](https://reader.tutors.dev/topic/reference-course/topic-09-svg)| [Iconify](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-09-svg) | 

If no image file is found in a resource, then Tutors will look to display an SVG Icon instead. This Icon is drawn from the Iconify collection:

- <https://icon-sets.iconify.design/>

You can search for an icon in that collection, locate the reference and include this + a colour in a [FrontMatter](https://docs.zettlr.com/en/core/yaml-frontmatter/) section for the Learning Resource. For example:

~~~markdown
---
icon:
  type: vscode-icons:file-type-pdf2
---
~~~

The above icon is drawn from this resource:

- <https://icon-sets.iconify.design/vscode-icons/file-type-pdf2/>

For monochrome icons, you can also specify the icon colour:

~~~markdown
---
icon:
  type: bi:filetype-pptx
  color: green
---
~~~


## Properties.yaml

This is a [YAML formatted](https://circleci.com/blog/what-is-yaml-a-beginner-s-guide/) file containing course-wide parameters. It defines the following aspects:

- Course attribution/credit
- Course Parent
- Course Companion Sites
- The Course Icon
- Topic show/hide settings for instructors (including PIN)
- Authentication + TutorsTime & TutorsLive
- Global video hide
- Global Lab Autonumbering

In addition, two (optional) supporting files :

- calendar.yaml
- enrolment.yaml

allow additional aspects to be specified.


### Course attribution/credit

The credits entry will be presented as a subtitle on the course home page:

~~~yaml
credits: Tutors Team
~~~

This can be any suitable string.

### Course Image/Icon

A course image/ icon will be displayed in the title bar when on the course home page. The image/icon also appears occasionally on other areas on the course web. It can be specified as a single image file in the root folder:

- course.png

Alternatively, you can specify an actual SVG Icon. Select an icon from this service:

- <https://icon-sets.iconify.design/>

Specify the Icon in `properties.yaml` like this:

~~~yaml
icon :
  type: fa-solid:code-branch
  color: FFD601
~~~

This icon is drawn from this resource on Iconfy:

- <https://icon-sets.iconify.design/fa-solid/code-branch/>


### Course Parent

Any course can also have (optionally) a single `parent` course

~~~yaml
parent         : course/wit-hdip-comp-sci-2021.netlify.app
~~~

This can be an absolute link, or a relative link to another tutors course (as shown above). The parent will appear in the breadcrumbs toolbar, represented as a `home` icon. Clicking this link will load that course into the current window (if a relative link as shown in the snippet above is used).


### Course Companion Sites

The companions toolbar hosts a set of links to external services. These can include:

- Slack Channel
- Moodle Course Web
- Youtube Playlist/Channel
- Zoom Meeting Room
- Teams

These can be specified as follows:

~~~yaml
slack        : https://wit-hdip-comp-sci-21.slack.com/
moodle       : https://moodle.wit.ie/course/view.php?id=176625
youtube      : https://www.youtube.com/playlist?list=PLEuhMaR29LyDMF2m4kSS9gVRCuimgo3GU
zoom         : https://wit-ie.zoom.us/j/96265735671
teams        : https://www.microsoft.com/microsoft-teams/join-a-meeting
~~~

You may choose to have a single companion, or none as approprioate. If you have a commpanion not in the above list, then the link, + icon can be specified like this:

~~~yaml
companions:
  piazza:
    link: https://piazza.com/
    title: Piazza Q & A for this course
    icon:
      icon: academicons:piazza
      colour: info
~~~

The above defines a companion icon to a Piazza service.

### Topic show/hide settings for instructors 

When publishing a course, you may wish hide some topics, but have access to them to check formatting or layout aspects. Also, you may have an entire course already laid out, and wish to just publish a subset of the topics.

Consider the following example:

~~~yaml
ignorepin : 4019
ignore : 
  # - topic-01-navigation
  # - topic-02-templating
  # - topic-03-semantic-code
  - topic-04-starting-play
  - topic-05-introducing-mvc
~~~

If the above course had 5 topics, then topics 04 and 05 will be 'ignored', i.e. not displayed. Effectively, we are 'commenting out' - via the `#` symbol, the first three topics - which means they will be visible to all students.

The instructor however, who will make up the `ignorepin`, can see all topics by entering the PIN code anywhere on the course canvas. In this way the instuctor can publish a topic per week (by commenting out an entry), but will be able to inspect unpublished topics by entering the PIN code.

### Global Video Hide

Occasionally, you may like to remove all videos from your course. This might be as the academic year rolls over perhaps, and you wish to leave the course online, but remove all videos.

~~~yaml
hideVideos     : true
~~~

This does not delete the video, or remove the video ids, that may be threaded through the content. It merely hides the videos, leaving all the assets in place.

### Authentication

By default, tutors courses are public. You can force users to authenticate before they have access to the course (apart from the landing page) via this setting:

~~~yaml
auth : 1
~~~

With this enabled, then entering a topic will trigger an authentication event. Users will only be permitted further by Signing in with a valid Github account. Any Github account will be accepted. When a user is logged in, a new Profile menu will appear, enabling access to additional course contextual information:

- Tutors Time: Some usage data is being gathered for authenticated users - which is now accessible via this menu. This is largely focussed on labs - specifically the time, in minutes, that a lab is active in the users browser. An instructor can use the PIN code they have set up (`ignorepin`) to reveal data for all students

- Tutors Live: This will show a card for each user currently logged in. It will take approx 1 minute to 'warm up' - and then as users come and go to the course web cards will appear and disappear. Students might use this to get in touch which whomsoever is currently online. The card for each student will display useful context information concerning the current activity of the student:


### Calendar

You may choose to prominently display an academic calendar - with the current week number highlighted. To set it up, include a file called `calendar.yaml` into your course folder. Here is an example:

~~~yaml
title: Semester 1
weeks:
  - 2021-08-30:
      title: 1
      type: tuition
  - 2021-09-06:
      title: 2
      type: tuition
  - 2021-09-13:
      title: 3
      type: tuition
  - 2021-09-20:
      title: reading 1
      type: reading
  - 2021-09-27:
      title: 4
      type: tuition
  - 2021-10-04:
      title: 5
      type: tuition
~~~

The title of the semester followed by a specification for each week:

- start date
- title
- type

in the format shown in the example above.

Using a calendar in conjunction with authentication adds another dimension to TutorsTime, showng estimates of the time online across the semester specified in the calendar file. An instructor can use the PIN code they have set up (`ignorepin`) to reveal data for all students.


### Enrollment.yaml

If authentication is enabled, then any user with a GitHub account can sign in to a course. In some circumstances that can make interpreting the TutorsTime data difficult, as it may include data from users not strictly students on the course. To this issue you can (optionally) provide an enrolment file -  `enrollment.yaml`. If present, then the TutorsTime reports will be limited to the students listed.

~~~yaml
- github-student-id-1
- github-student-id-2
- github-student-id-3
- github-student-id-4
- github-student-id-5
~~~


How to get the IDs? The simplest method is to wait for a few sessions, and all students have logged in. Then, in TutorsTime enter the `ignore pin` to reveal `Labs for All Students`. The green export icon (a small disk) will allow you to export all data to an Excel file. 

#### Whitelisting

By default, the contents of the enrollment file is not a Whitelist as such - ie. non-students can still access the module. If you would like to only permit the students listed in the enrolment file to access the course, then include the following:

~~~yaml
whitelist      : 1
~~~

Remember, authentication must also be enabled for this to work:

~~~yaml
auth           : 1
~~~

... and you must, of course, have an accompanying enrollment file. 


### Auto Numbering

You may prefer all steps in all your labs to be autonumbered. This will preppend a number, starting at 01, to all steps

~~~
labStepsAutoNumber: true
~~~

This is independent of the 'sort-key' segment in the lab step name.

## Image resizing

Some images you mau choose to use may be hi-resolution, appearing to be super sized on the canvas. This can be a particular issue with screen shots. This service here:

- <https://nodeca.github.io/pica/demo>

Allows you to resize the image to a suitable 'canvas' size.


## Reference Course

A reference course is located here:

- <https://github.com/tutors-sdk/tutors-reference-course>

and is published here:

- <https://reader.tutors.dev/course/reference-course>

This course illustrates all Tutors featues. It can be downloaded here:

- <https://github.com/tutors-sdk/tutors-reference-course/archive/refs/heads/main.zip>

  
