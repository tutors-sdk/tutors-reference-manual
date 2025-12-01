---
icon:
  type: flat-color-icons:rotate-to-portrait
---

# Panels

*The Panel Learning Objects*

---

[[toc]]

---

# Panel Resources

Panels appear directly in a unit or topic, and are not represented by a separate card. Instead, their contents are rendered directly onto the parent topic/unit/side.

| Example Resource | Display | 
| ---------------- | ------- | 
| [A full screen width video, hosted in YouTube or HEANet](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/panelvideo-1) | [Main Video](https://tutors.dev/topic/reference-course/topic-03-media)     |
| [Full screen width  presentation in pdf format    ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-05-panel-talk/paneltalk)    | [Main Talk](https://tutors.dev/topic/reference-course/topic-05-panel-talk) | 
| [Full screen width note](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-04-panel-note/panelnote)                               | [Main Note](https://tutors.dev/topic/reference-course/topic-04-panel-note) | 

## Panelvideo 

A video to be displayed directly on the topic or unit resource.

| Example Resource | Display | 
| ---------------- | ------- | 
| [A full screen width video, hosted in YouTube or HEANet](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-03-media/panelvideo-1) | [Main Video](https://tutors.dev/topic/reference-course/topic-03-media) |

Two files are required:

| Resource   | Purpose  |
| ---------- | -------- |
| video.md   | Title for the video. The file can have any suitable name, but must be .md file type |
| videoid    | id of the video |

See video section below for format of this videoid file.

## Paneltalk 

A PDF document to be displayed directly on the parent resource.

| Example Resource | Display | 
| ---------------- | ------- | 
| [Full screen width  presentation in pdf format    ](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-05-panel-talk/paneltalk) | [Main Talk](https://tutors.dev/topic/reference-course/topic-05-panel-talk) | 

Two files are required:

| Resource   | Purpose  |
| ---------- | -------- |
| talk.md   | Title for the document. The file can have any suitable name, but must be .md file type |
| talk.pdf  | The .PDF to display. Its name must be the same as the .md file |


## Panelnote 

A panel note is a full web page layed out directly on the parent resource.

| Example Resource | Display | 
| ---------------- | ------- | 
| [Full screen width note](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-04-panel-note/panelnote) | [Main Note](https://tutors.dev/topic/reference-course/topic-04-panel-note) | 

The content is authored in markdown using the basic syntax:

- <https://www.markdownguide.org/basic-syntax/>

The content is in a single file, images and archives may also be included:

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

