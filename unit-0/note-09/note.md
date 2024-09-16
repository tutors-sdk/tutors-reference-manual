---
icon:
  type: flat-color-icons:stack-of-photos
---

# Icons

How to include Icons as card images


If no image file is found in a resource, then Tutors will look to display an SVG Icon instead. This Icon is drawn from the Iconify collection:

- <https://icon-sets.iconify.design/>

| Example Resource                                             | Display                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Icon based cards](https://tutors.dev/topic/reference-course/topic-09-svg) | [Iconify](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-09-svg) |

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
