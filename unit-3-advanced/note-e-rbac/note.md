---
icon:
  type: material-symbols:admin-panel-settings
---
# Roles & Content Locking

*Educator roles, the educator panel, and content locking*

---

[[toc]]

Tutors includes a role-based access control system that distinguishes between **students** and **educators**. Educators gain access to an expanded panel with content locking, enrollment visibility, and access diagnostics. This builds on the [Authentication](/note/reference-course/unit-1-getting-started/note-d-properties#authentication) and [Enrollment](/note/reference-course/unit-1-getting-started/note-d-properties#enrollment) features described in Course Properties.

# Prerequisites

RBAC requires authentication to be enabled:

~~~yaml
auth: 1
~~~

You must also have an `enrollment.yaml` file in the course root.

# Defining Educators

Add an `educators` array to your `enrollment.yaml` file. Each entry is a GitHub username:

~~~yaml
educators:
  - github-username-1
  - github-username-2

whitelist:
  - github-username-1
  - github-username-2
  - student-github-id-1

students:
  - name: Alice
    github: student-github-id-1
  - name: Bob
    github: student-github-id-2
~~~

When a user signs in with a GitHub account that matches an entry in the `educators` list, they are assigned the **educator** role. All other authenticated users are assigned the **student** role.

# Educator Permissions

The educator role grants the following capabilities:

- **Content locking** — lock and unlock individual learning objects to control what students can see.
- **Analytics access** — view course analytics and usage data via Tutors Time.

# The Educator Panel

When an educator is signed in, the Info button in the course navigator changes to an educator icon and opens an expanded sidebar with the following tabs:

## Info

The standard course information panel, displaying the rendered `course.md` content.

## Locks

Lists every top-level learning object in the course (topics, units, sides). Each item has a toggle switch. When a lock is toggled **on**:

- **Students** will not see the learning object at all — it is hidden from their view.
- **Educators** will still see the learning object, but with a visual lock indicator.

This is useful for progressively revealing course content week by week, or temporarily hiding material that is under revision.

Lock state is shared across all users of the course, so any educator can see and manage locks set by another educator.

## Enrollment

Displays three sections when the corresponding data exists in `enrollment.yaml`:

- **Educators** — the list of GitHub usernames with the educator role
- **Whitelist** — users permitted to access the course (if whitelisting is enabled)
- **Students** — the enrolled student list with names and GitHub IDs

This provides a quick reference without needing to open the YAML file.

## Access

Shows the current authentication level (`auth` value from `properties.yaml`) and lists all educators defined in the enrollment file. Useful for verifying access configuration at a glance.

# Content Locking

Content locking allows educators to control which parts of a course are visible to students. This is managed entirely through the Locks tab in the educator panel — no changes to the course source files are needed.

## How it works

1. Sign in with a GitHub account listed in `educators` in `enrollment.yaml`.
2. Open the educator panel (the shield icon in the navigator).
3. Select the **Locks** tab.
4. Toggle the switch next to any learning object to lock or unlock it.

Locks apply to top-level learning objects (topics and their equivalents). When a topic is locked, all of its nested content (labs, talks, notes, etc.) is also hidden from students.

## Relationship to ignore/ignorepin

Earlier versions of Tutors used the `ignore` and `ignorepin` properties in `properties.yaml` to control topic visibility:

~~~yaml
ignorepin: 4019
ignore:
  - topic-04-starting-play
  - topic-05-introducing-mvc
~~~

With this mechanism, listed topics are hidden from students. The instructor can reveal them by entering the PIN code anywhere on the course canvas. Topics are published progressively by commenting out entries with `#`.

Content locking supersedes this approach — it provides the same progressive-reveal capability but without requiring edits to the course source files or redeployment. Locks are toggled live from the educator panel and take effect immediately for all users.

The `ignore`/`ignorepin` mechanism is still supported and will continue to work, but it is expected to be phased out in a future release. For new courses, content locking is the recommended approach.

# Example

A minimal course setup with RBAC:

**properties.yaml**

~~~yaml
credits: My Course
auth: 1
~~~

**enrollment.yaml**

~~~yaml
educators:
  - my-github-username

students:
  - name: Alice
    github: alice-github
  - name: Bob
    github: bob-github
~~~

With this configuration:

- `my-github-username` signs in and sees the educator panel with lock controls.
- `alice-github` and `bob-github` sign in as students and see only unlocked content.
- Any other GitHub user can sign in but is treated as a student (unless whitelisting is also enabled, in which case they would be blocked).
