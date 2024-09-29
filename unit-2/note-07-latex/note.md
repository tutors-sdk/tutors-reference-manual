---
icon:
  type: flat-color-icons:steam
---

# Latex

*How to include Latex notation in labs and notes*

Notes and Books can have Latex content, implemented using the [Katex](https://katex.org/) component. 

| Example Resource                                             | Display                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Latex](https://github.com/tutors-sdk/tutors-reference-course/blob/main/topic-01-typical/unit-1/book-a/05.05.md) | [Latex Example](https://tutors.dev/lab/reference-course/topic-01-typical/unit-1/book-a/05) |

You can express Latex content between "$" symbols. For example:

~~~latex
$
x=\frac{ -b\pm\sqrt{ b^2-4ac } } {2a}
$
~~~

Will render as:

$
x=\frac{ -b\pm\sqrt{ b^2-4ac } } {2a}
$

You can also express content inline, so thisL

~~~latex
This is an inline example: $c = \pm\sqrt{a^2 + b^2}$ with text before and after
~~~

This will render link this:

This is an inline example: $c = \pm\sqrt{a^2 + b^2}$ with text before and after.

