---
icon:
  type: flat-color-icons:genealogy
---

# Course Structure

*The structure of a tutors course*


## Course Structure

A Tutors course is a folder of learning resources, named according to the conventions described in this manual.

| Example Resource | Display | 
| ---------------- | ------- | 
| [reference-course](https://github.com/tutors-sdk/tutors-reference-course) | [Reference Course](https://tutors.dev/course/reference-course) |

The minimum requirements for a course are a folder containing these three files:

| File name      | Purpose      |  
| -------------- | ------------ | 
| course.md      | Course title + general course information
| course.png     | Course image
| properies.yaml | Course properties

#### course.md

A markdown file, structured as follows:

~~~markdown
Course Title

Course information - a course outline, description or any other information. 
~~~

#### properties.yaml

Course metadata in yaml format. At a minimum, this must contain the following:

~~~yaml
credits: The course author(s) or organisation
~~~

The credits property will appear as a subtitle in the course title bar.

There are a range of other optional properties. See [later in this document]( https://tutors.dev/note/tutors-reference-manual/unit-0/note-10)for a complete list of valid properties.

A course folder will typically contain topics (see below). Occasionally  you may choose to place units directly in a course folder.

Courses typically contain topics, represented as folders with *topic* as the first 5 letters in the folder name. These topics can units (described below) 

### Topic

Top level learning resource for a course. Typically encapsulating a session or week of learning material.

| Example Resource | Display | 
| ---------------- | ------- | 
| [topic-01-typical](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical) | [Typical Topic](https://tutors.dev/topic/reference-course/topic-01-typical) |

Two files are required:

| Files  | Purpose  |
| --------------- | ------------ |
| topic.md  | Topic title + summary. Any file name, file type must be .md file type |
| topic.png | Image for topic. File name must be same as .md file. File type can be .png, .jpg, or .jpeg|

#### topic.md

The title and subtitle are extracted from the .md file, for example:

~~~~markdown
Simple

Units with presentations, labs + resources
~~~~

In addition to the title, subtitle + image files specified above, the topic can contain any number of units (see below) or other learning resources.

### Unit

A unit will encapsulate learning resources, framed by a title. Units can be within topics, or can appear directly in a course folder.

| Example Resource | Display | 
| ---------------- | ------- | 
| [unit-1](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-01-typical/unit-1) | [Main Lesson](https://tutors.dev/topic/reference-course/topic-01-typical) |

One .md file is required:

| Files   | Purpose                                                      |
| ------- | ------------------------------------------------------------ |
| unit.md | Title for the unit. The file can have any suitable name, but must be .md file type |


The title is specified in a single markdown file:

#### unit.md

~~~markdown
Main Lesson
~~~

Units contain any number of learning resources.

### Side

A side will encapsulate learning resources, framed by a title. It is equivalent to a Unit (see above), but framed within a side bar.

| Example Resource | Display | 
| ---------------- | ------- | 
| [side-unit](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-02-side/side-unit) | [Labs for this Topic](https://tutors.dev/topic/reference-course/topic-02-side) |

On .md file is required:

| File name | Purpose                                                      |
| --------- | ------------------------------------------------------------ |
| side.md   | Title for the side bar. The file can have any suitable name, but must be .md file type |

The title is specified in in this file:

#### side.md

~~~markdown
Labs for this Topic
~~~

Side bar can contain any number of learning resources.

## Learning Resources

There are 2 broad types of learning resources

- Card Resources
- Panel Resources

Typically card or panel learning resources are child folders within topic, unit or side folders.
