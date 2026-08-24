---
icon:
  type: flat-color-icons:view-details
---
# Course Properties

*The properties.yaml file*

---

[[toc]]

This is a [YAML formatted](https://circleci.com/blog/what-is-yaml-a-beginner-s-guide/) file containing course-wide parameters. In addition, two (optional) supporting files :

---

# Course attribution/credit

The credits entry will be presented as a subtitle on the course home page:

~~~yaml
credits: Tutors Team
~~~

This can be any suitable string.

# Course Image/Icon

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


# Course Parent

Any course can also have (optionally) a single `parent` course

~~~yaml
parent         : course/wit-hdip-comp-sci-2021.netlify.app
~~~

This can be an absolute link, or a relative link to another tutors course (as shown above). The parent will appear in the breadcrumbs toolbar, represented as a `home` icon. Clicking this link will load that course into the current window (if a relative link as shown in the snippet above is used).


# Course Companion Sites

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
      type: academicons:piazza
      color: info
~~~

The above defines a companion icon to a Piazza service.

# Edit Course Button

If your course in on GitHub, and you decide to make repo the course public, then you may like to have tutors include an "Edit this page" button on the main header. To enable this, include  a `github` property in properties.yaml. E.g:

~~~yaml 
github: https://github.com/tutors-sdk/tutors-reference-course/blob/main
~~~

Note that we append `/blob/main` to the github url - indicating you published from the `main` branch. You can change this as appropriate.

You can see it in action here:

- <https://tutors.dev/course/reference-course>

It can be particularly useful for labs - with the edit button opening the markdown page in edit mode when selected:

- <https://tutors.dev/lab/reference-course/topic-01-typical/unit-1/book-a/01>

On GitHub, when an (non-owner) attempts to edit a page then GitHub will prompt the user to fork the repo, and edits will then be proposed as PRs, which the course author can choose to accept or reject.

# Topic show/hide settings for instructors 

When publishing a course, you may wish hide some topics, but have access to them to check formatting or layout aspects. Also, you may have an entire course already laid out, and wish to just publish a subset of the topics.

Consider the following example:

~~~yaml
ignorepin : 4019
ignore : 
  - topic-01-navigation
  - topic-02-templating
  - topic-03-semantic-code
  # - topic-04-starting-play
  # - topic-05-introducing-mvc
~~~

If the above course had 5 topics, then topics 04 and 05 will be 'ignored', i.e. not displayed. Effectively, we are 'commenting out' - via the `#` symbol, the first three topics - which means they will be visible to all students.

The instructor however, who will make up the `ignorepin`, can see all topics by entering the PIN code anywhere on the course canvas. In this way the instuctor can publish a topic per week (by commenting out an entry), but will be able to inspect unpublished topics by entering the PIN code.

# Global Video Hide

Occasionally, you may like to remove all videos from your course. This might be as the academic year rolls over perhaps, and you wish to leave the course online, but remove all videos.

~~~yaml
hideVideos     : true
~~~

This does not delete the video, or remove the video ids, that may be threaded through the content. It merely hides the videos, leaving all the assets in place.

# Authentication

By default, tutors courses are public. You can force users to authenticate before they have access to the course (apart from the landing page) via this setting:

~~~yaml
auth : 1
~~~

With this enabled, then entering a topic will trigger an authentication event. Users will only be permitted further by Signing in with a valid Github account. Any Github account will be accepted. When a user is logged in, a new Profile menu will appear, enabling access to additional course contextual information:

- Tutors Time: Some usage data is being gathered for authenticated users - which is now accessible via this menu. This is largely focussed on labs - specifically the time, in minutes, that a lab is active in the users browser. An instructor can use the PIN code they have set up (`ignorepin`) to reveal data for all students

- Tutors Live: This will show a card for each user currently logged in. It will take approx 1 minute to 'warm up' - and then as users come and go to the course web cards will appear and disappear. Students might use this to get in touch which whomsoever is currently online. The card for each student will display useful context information concerning the current activity of the student:

See [TutorsTime]() for a more detail on data gathering and usage.

# Portfolio

For some courses, you may not wish to have any topics at all, just units and links perhaps to other courses. For example [this course here](https://tutors.dev/course/wit-hdip-comp-sci-2023). Notice that this course does not have:

- Breadcrumbs navigation 
- Table of Contents 

This is enabled with the 'portfolio' property:

~~~
portfolio : true
~~~

This is the [source for a course](https://github.com/wit-hdip-comp-sci-2023/programme-home-page) using this property.

# Calendar

You may choose to prominently display an academic calendar — with the current week highlighted — by including a file called `calendar.yaml` in your course folder. A calendar button will appear in the course navigator, showing the current week's topic. Selecting it opens a sidebar with the full semester schedule.

## Calendar format

~~~yaml
title: Semester 1
year: 2026

weeks:
  - date: 2026-08-31
    week: 0
    topic: Induction
    assessment: ~

  - date: 2026-09-07
    week: 1
    topic: Variables and Data Types
    assessment: ~

  - date: 2026-09-14
    week: 2
    topic: Control Flow
    assessment:
      name: Assignment 1
      due: 2026-09-21
      percentage: 20
      submission: Online (Moodle)

  - date: 2026-09-21
    week: ~
    topic: Reading Week
    assessment: ~

  - date: 2026-09-28
    week: 3
    topic: Functions
    assessment: ~
~~~

### Fields

| Field | Required | Description |
|-------|----------|-------------|
| `title` | Yes | Calendar title displayed in the sidebar header |
| `year` | No | Appended to the title in the UI |
| `weeks` | Yes | Array of week entries |

### Week entry fields

| Field | Required | Description |
|-------|----------|-------------|
| `date` | Yes | Start date of the week (YYYY-MM-DD) |
| `week` | Yes | Week number. Use `~` (null) for non-teaching weeks such as reading weeks or breaks |
| `topic` | Yes | Display label for the week |
| `assessment` | Yes | Assessment details or `~` (null) if none |

### Assessment fields

| Field | Description |
|-------|-------------|
| `name` | Assessment title |
| `due` | Due date (YYYY-MM-DD) |
| `percentage` | Weighting as a percentage |
| `submission` | Submission method (e.g. "Online (Moodle)", "In-person interview") |

### Display behaviour

- The current week is highlighted when today's date falls within its range.
- Weeks with `week: ~` are displayed as break weeks without a week number.
- Week number and assessment columns only appear when at least one entry uses them.
- The calendar button in the navigator shows the current week's topic, or falls back to the calendar title if no week matches today.

## Legacy format

The original calendar format is still supported. Each week entry uses a date string as the key with nested `title` and `type` fields:

~~~yaml
title: Semester 1
weeks:
  - 2026-09-07:
      title: Topic 1
      type: topic
  - 2026-09-14:
      title: Topic 2
      type: topic
  - 2026-09-28:
      title: Reading Week
      type: break
~~~

The `type` field is `topic` for teaching weeks or `break` for non-teaching weeks. This format does not support week numbers or assessments. The parser auto-detects which format each entry uses, so both formats will continue to work.

## Calendar and TutorsTime

Using a calendar in conjunction with authentication adds another dimension to TutorsTime, showing estimates of the time online across the semester specified in the calendar file. An instructor can use the PIN code they have set up (`ignorepin`) to reveal data for all students.


# Enrollment

If authentication is enabled, then any user with a GitHub account can sign in to a course. In some circumstances that can make interpreting the TutorsTime data difficult, as it may include data from users not strictly students on the course. To this issue you can (optionally) provide an enrolment file -  `enrollment.yaml`. If present, then the TutorsTime reports will be limited to the students listed.

~~~yaml
students:
  - name: Student McStudious
    github: github-id-1
  - name: Tutors McTutorios
    github: github-id-2
~~~

## Whitelisting

By default, the contents of the enrollment file is not a Whitelist as such - ie. non-students can still access the module. If you would like to only permit the students listed in the enrolment file to access the course, then include the following:

~~~yaml
whitelist
- github-id-1
- github-id-2
~~~

So a complete enrollment file might look like this:

~~~yaml
whitelist:
- github-id-1
- github-id-2
students:
  - name: Student McStudious
    github: github-id-1
  - name: Tutors McTutorios
    github: github-id-2
~~~

In the above example, students is optional.

Remember, authentication must also be enabled for this to work:

~~~yaml
auth           : 1
~~~

## Educator Roles

You can also designate educators in your enrollment file. Educators gain access to an expanded panel with content locking and enrollment visibility. See [Roles & Content Locking](/note/reference-course/unit-3-advanced/note-e-rbac) for full details.

~~~yaml
educators:
  - github-username-1
  - github-username-2
~~~

# Auto Numbering

You may prefer all steps in all your labs to be autonumbered. This will preppend a number, starting at 01, to all steps

~~~yaml
labStepsAutoNumber: true
~~~

This is independent of the 'sort-key' segment in the lab step name.

# Private

Tutors provides the following services:

- [Gallery](https://tutors.dev/gallery): a selection of 50  or so unique modules

- [Catalogue](https://tutors.dev/catalogue): a general listing of all known Tutors courses
- [Live](https://tutors.dev/live): live view of which course are active live (all users are anonymous on this view)

If you wish your course to be *excluded* from all of the above, then you can mark it private with the following entry in properties.yaml:

~~~yaml
private: 1
~~~

In addition to withdrawing a private course from the above services, the course will also remove the Log in / Profile menus from the header, disabling the dashboard and any sign in capabilities.

# Default PDF reader

The tutors reader will render talks using the [Adobe PDF Embed API](https://developer.adobe.com/document-services/docs/overview/pdf-embed-api/). An alternative reader based on the [Mozilla pdf.js](https://mozilla.github.io/pdf.js/) project is also available. 

To use Mozilla by default in all Talks:

~~~yaml
defaultPdfReader: mozilla
~~~

To use Adobe:

~~~yaml
defaultPdfReader: adobe
~~~

Adobe is also the default.

If you use Adobe, the default mode will be to display the pdf in "landscape" orientation - displaying a single slide at a time in a 16:9 aspect ratio. This is best for presentations / slides. If you would prefer a portrait style display - perhpas an A4 document, then include the following:

~~~yaml
pdfOrientation : portrait
~~~


# Large Language Model support

Your course can be made available in a format suitable for processing by Large Language Models (LLMs). This takes the form of a dedicated page on your course. If this is your course url:

- https://tutors.dev/course/tutors-starter-course

Then this is could be the llm page:

- https://tutors.dev/llm/tutors-starter-course

*(llm* instead of *course* in the route). Your course may need to re-deployed with the latest tutors-publish for this to be available.

This Llm page makes available the following assets:

- A link to a markdown version of the entire course, as a singe standalone page
- A link to a zip archive of all Talks in the course
- Links to the YouTube videos

This is followed by a version of the above for each topic only. This will be in the form of links under each topic in your course. See this [example here](https://tutors.dev/llm/tutors-starter-course).

You can submit all of the above to many Llms. For Pdfs, you may need to upload pdf files individually. Sometimes it is more useful to submit resources by topic. [Google Notebook LLM](https://notebooklm.google.com/) for instance works well for topic based resources. So you could create a Notebook for each topic - and then generate notes, quizzes, FAQs, and Podcasts! confined to each individual topic.

If you would like to advertise the link to these resources, then place this in the properties.yaml:

~~~yaml 
llm: 2
~~~

This will place a small 'idea' icon next to the search button in the course navigator. If you would like to keep the page, but not advertise the link then:

~~~yaml
llm: 1
~~~

If you would like the page to be disabled - and no llm content generate then set to 0:

~~~yaml
llm: 0
~~~

llm: 0 is the default behaviour if no setting is found, i.e. no llm content is generated be default.

