<p align="center">
  <a href="https://tutors.dev">
    <img src="./tutors-light.png"
  </a>
</p>

<h3 align="center">
Tutors: An Open Learning Web Toolkit
</h3>

<p align="center">
  <a href="https://tutors.dev">Website</a> |
  <a href="https://tutors.dev/course/tutors-reference-manual">Documentation</a>
</p>

# What is Tutors?

Tutors is a collection of open source components and services supporting the creation of transformative learning experiences using open web standards. It consists of two key components:

- _Generator:_ transforms a [folder of learning content](https://github.com/tutors-sdk/tutors-reference-course) into a Tutors course

- _Reader_: presents a Tutors course as an intuitive, discoverable and attractive [Web experience](https://tutors.dev/course/reference-course)

Tutors is developed in the open by an active and friendly community, based on a [simple set of values](https://tutors.dev/course/tutors-reference-manual#tutors-values).

This repo is the Tutors Reference Manual - and can be viewed [here](https://tutors.dev/course/tutors-reference-manual).


# Tutors Repos

- [Tutors Mono Repo](https://github.com/tutors-sdk/tutors-mono-repo): One repository for the whole platform. It holds the four applications (the course reader behind tutors.dev, the catalogue, Tutors Live and Tutors Time), the course generators published to JSR (`@tutors/tutors`, `@tutors/tutors-lite`, `@tutors/tutors-create`) and the shared libraries they are built from. It is written in TypeScript, with SvelteKit for the applications.
- [Tutors Release Harness](https://github.com/tutors-sdk/tutors-release-harness): Runs each release candidate of the mono-repo beside production and fails on any difference nobody claimed. Its reports are published [here](https://tutors-sdk.github.io/tutors-release-harness/).
- [Tutors Reference Manual](https://github.com/tutors-sdk/tutors-reference-manual): The manual is itself a tutors course, and is largely written in Markdown.
- [Tutors Reference Course](https://github.com/tutors-sdk/tutors-reference-course): A Tutors course to included all tutors learning objects and structures.

The earlier per-application repositories ([tutors](https://github.com/tutors-sdk/tutors), [tutors-apps](https://github.com/tutors-sdk/tutors-apps)) were folded into the mono-repo, and new work happens there.


### License

[![license](https://img.shields.io/badge/license-MIT-3A929B.svg)