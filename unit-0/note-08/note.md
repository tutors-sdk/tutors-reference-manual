---
icon:
  type: flat-color-icons:numerical-sorting-12
---

# Ordering Resources

How to specify the order in which cards appear


## Ordering Learning Resources

For a topic, unit or side resources, the ordering of the cards is as follows:

- talk
- lab
- note
- web
- github
- archive

This can be customised via the introduction of [FrontMatter](https://docs.zettlr.com/en/core/yaml-frontmatter/) sections in the corresponding markdown files. 

| Example Resource | Display | 
| ---------------- | ------- | 
| [Ordering](https://github.com/tutors-sdk/tutors-reference-course/tree/main/topic-08-ordering)| [Ordering Example](https://tutors.dev/topic/reference-course/topic-08-ordering) | 

This should contain an "order" number, which dictates the sequencing for the cards.

~~~yaml
---
order: 1
---
~~~

If the resouce is a Lab, then the first step should include the FrontMatter/order paramater.

