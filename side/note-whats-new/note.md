---
order: 0
icon:
  type: fluent-color:news-24
---

# Whats new

Some updates to Tutors you may find useful for the coming semester

---
[[toc]]
---

# Code in Quizzes

A quiz question, or any of its options, can now carry a fenced code block, highlighted with the same theme as your labs and notes. Open the quiz with **four** backticks (or with `~~~quiz`) so an ordinary three-backtick fence can sit inside it:

`````markdown
````quiz
question: What does this print?

```js
console.log([1, 2, 3].map((x) => x * 2));
```
type: multiple-choice
options:
  - |
    ```js
    [2, 4, 6]
    ```
  - "`[1, 2, 3]`"
  - It throws a `TypeError`
correct: 0
````
`````

Inside a code fence a `---` line or a `field:` line stays part of the code, so a snippet never accidentally starts a new question. A three-backtick `` ```quiz `` block still works as long as every fence inside it names its language, but four backticks is the form to reach for.

[Example here](https://tutors.dev/quiz/reference-course/topic-07-reference/quiz-1), and [documentation here](https://tutors.dev/note/tutors-reference-manual/unit-2-panels-and-videos/note-a-resources#quiz)

# Export your enrollment.yaml

Writing an `enrollment.yaml` by hand meant collecting GitHub usernames one at a time. You can now build one from the people who have actually opened your course: **Educator Control → Enrollment → Export enrollment.yaml** hands you a draft assembled from the course's recorded visitors.

It keeps what you already have — your educators, your whitelist, names and any extra fields you've added — leaves educators out of the new student entries, and deduplicates. As the panel says: review the draft before you use it to restrict access. The export currently supports Netlify-hosted courses, and the server checks you are an educator of the published course before it reads any student data.

[Documentation here](https://tutors.dev/note/tutors-reference-manual/unit-3-advanced/note-e-rbac)

# Reporting Periods in Tutors Time

The period you are reporting on is now part of the address: `?start=2026-09-01&end=2026-12-20`, both days inclusive, either one optional. Every total covers those days and no others.

Because it lives in the URL, a reporting period is bookmarkable and shareable, and it survives reload, back and forward. Sidebar links, student drill-downs, the breadcrumb and the raw tables all carry it, and the header names the period you are looking at. A date that isn't a real calendar date, or a start after the end, now gives you a page saying so rather than a silently wider report.

Labs are also counted properly where two topics each hold a `book-lab-01`: they get a column and a median each instead of being added together.

[Documentation here](https://tutors.dev/note/tutors-reference-manual/unit-3-advanced/note-f-time)

# Courses on your own Domain

Courses hosted somewhere other than Netlify now behave. A course's PIN is read from the host that actually serves it rather than always from `https://<course id>.netlify.app`, so a course on its own domain, or on your own machine, can be opened past the PIN prompt. A full `https://...` course address is fetched once and cached like any other, instead of being requested as `https://https://...` and failing. A course served over `http` from `localhost` links to its own files, including the LLM downloads, rather than to an `https://localhost` address that never loads.

Bare course ids are still treated as Netlify sites, so nothing you already have changes.

# Content Locks, Saved Properly

Content locks are now written by the reader's server, which checks that you are an educator of the course before saving anything. If a lock change doesn't save, the switch goes back and tells you it wasn't saved, instead of showing as in force while your students still see the previous setting.

Worth knowing if you use locks: the old version wrote locks straight from the browser with a key that ships in every page, so in principle anyone could have changed any course's locks. That's closed.

# Smaller Things

- **Whiteboards save.** Whiteboard edits are stored properly now — until this release the saving had never worked on tutors.dev at all.
- **Talks remember their reader.** Switching a talk between the browser PDF reader and Adobe's and back shows the viewer you already had, keeping your page and zoom, instead of a blank frame or an error.
- **Recently accessed respects privacy.** A private course that turns a student away is no longer recorded as a visit, so it stops appearing under Recently accessed for someone it won't let in.
- **Accessibility.** More of the interface now reads the way it looks: the Preferences button is announced as "Preferences", the calendar button leads with the week written on it, and the keyboard hints on Search are announced as shortcuts rather than as part of the button's name. This matters for anyone driving Tutors by voice.
- **Time reports are steadier.** Heat maps are laid out after the fonts have loaded, so the day grid looks the same on every visit.

# Quizzes

Quizzes are now a learning object type. Create a `quiz-*` folder with a markdown file containing a fenced `quiz` block, and Tutors renders an interactive quiz: one question at a time, free navigation between questions, and a scored results page with a retake. Questions can be multiple-choice or true/false, and question and option text is markdown — so `code`, emphasis and links all render. Prose outside the quiz block renders normally, so a quiz can carry its own instructions or diagrams. Answers are not recorded: this is a self-check for students, not an assessment.

::video[src="./img/quiz.mov" poster="img/quiz.png"]::

[Example here](https://tutors.dev/quiz/reference-course/topic-07-reference/quiz-1), and [documentation here](https://tutors.dev/note/tutors-reference-manual/unit-2-panels-and-videos/note-a-resources#quiz)

# Create a Course

You can now scaffold a complete, ready-to-run Tutors course in seconds. The Create wizard walks you through a few options — course name, units, topics, and whether to include notes, labs, a calendar and an enrollment list — and hands you an editable course zip, pre-wired with sensible defaults, colour icons, a `properties.yaml` documenting every setting, and a `netlify.toml` ready for deployment. Prefer the terminal? The same scaffolder runs as a CLI via `deno run -A jsr:@tutors/tutors-create`.

::video[src="./img/create.mov"]::

[Try the Create wizard here](https://tutors.dev/create)

# Juypiter Notebooks

Jupyter notebooks are now a first-class learning object type. Place a notebook-* folder with a .ipynb file and it renders directly in the browser — code cells with syntax highlighting, markdown cells, and outputs. Interactive execution is supported via Pyodide for Python notebooks. Other languages to follow.

::video[src="./img/juypter.mov" poster="img/juypter.png"]::

[Example here](https://tutors.dev/notebook/python-fundmentals/topic-03-operators/unit-1/notebook-operators), and [documentation here](https://tutors.dev/note/tutors-reference-manual/unit-2-panels-and-videos/note-a-resources#notebook-(jupyter))



# Mermaid Diagrams

You can now include Mermaid diagrams directly in any course markdown — notes, labb and notebooks. Just use a standard mermaid code block and the diagram renders inline. Flowcharts, sequence diagrams, class diagrams, ER diagrams, and all other Mermaid types are supported. LLMs love generating mermaid diagrams.


::video[src="./img/mermaid.mov" poster="img/mermaid.png"]::

[Example here](https://tutors.dev/note/python-fundmentals/topic-03-operators/unit-1/note-1-operator-precedence), and [documentation here](https://tutors.dev/note/tutors-reference-manual/unit-3-advanced/note-a-mermaid)

All the diagrams in [this note](https://tutors.dev/note/tutors-reference-manual/unit-4-dev/note-architecture) were generated by Claude by analysing the source code of the tutors repo.

# Marp Talks

Talks can now be authored as Marp markdown instead of PDF. Write your slides as a .md file with Marp front matter and they render as a full slide deck in the browser. This gives you version-controlled, text-based presentations alongside your other course content.

::video[src="./img/marp.mov" poster="img/marp.png"]::

[Example here](https://tutors.dev/talk/python-fundmentals/topic-03-operators/unit-1/talk-1-operators), and [documentation here](https://tutors.dev/note/tutors-reference-manual/unit-2-panels-and-videos/note-a-resources#marp-talk)


# Educator Role & Content Locking


Courses with an enrollment.yaml can now designate educators. Add an educators array with GitHub usernames and those users get access to a new educator panel in the course navigator. From there, you can lock and unlock individual topics — locked topics are hidden from students but remain visible to you. This replaces the old ignore/ignorepin approach for progressive content release, with no source file edits or redeployment needed.

::video[src="./img/locks.mov" poster="img/locks.png"]::

[Documentation here](https://tutors.dev/note/tutors-reference-manual/unit-3-advanced/note-e-rbac)

# New Calendar Format

The calendar.yaml file now supports a richer format with explicit week numbers, topic labels, and assessment details (name, due date, percentage, submission method). Break weeks are marked with week: ~. The calendar button in the navigator shows the current week, and assessment columns appear automatically when present. The old format continues to work.

::video[src="./img/calendar.mov" poster="img/calendar.png"]::

[Example here](https://tutors.dev/course/python-fundmentals), and [documentation here](https://tutors.dev/note/tutors-reference-manual/unit-1-getting-started/note-d-properties#calendar)


# Course Prompting

With these new features you can now prompt an entire course into existence.  I guess the quality of the course will depend on the LLM, the prompt and the context you provide. You should at least provide the [tutors reference manual - llm version](https://tutors-reference-manual.netlify.app/llms/tutors-reference-manual-complete-llms.txt) - and also perhaps an example complete course. Just the course descriptor is not sufficient. However, some of the generated courses may give you an interesting structure to play around with.

Here are two examples :

- [Introducing Python](https://tutors.dev/course/python-fundmentals) ([source repo](https://github.com/tutors-sdk/python))

- [Introducing Svelte](https://tutors.dev/course/svelte-kit-course.netlify.app) ([source repo](https://github.com/tutors-sdk/svelte-kit))

When prompting, you could ask the LLM to emulate the use of icons from Iconify as demonstrated in the above repos (give the course source repo, not the course web for context).

# Layout Menu

 Check out the layout menu for internationalisation (including Gaeilge) and a guided tour (unfinished as yet). Also, there have been accessibility updates throughout.
