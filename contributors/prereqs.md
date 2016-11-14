---
layout: documentation
title:  Prerequisites
order: 1
---

Phovea is built on a range of standard technologies: Contributors to Phovea should be familiar
with the tools we're already using.

- Understanding what's already in place may save you from reinventing the wheel,
or adding an extra wheel.
- Understanding the idioms of the tool you're working with will make your code more clear
to future developers.

## Version Control

All source code and documentation for Phovea is hosted on [Github](https://github.com/phovea).
Projects built with Phovea can be found in the distinct [Caleydo](https://github.com/caleydo)
organization on Github. If you haven't used git itself before, please read through 
[this tutorial](https://git-scm.com/docs/gittutorial),
or try this [interactive one](https://try.github.io/).

To make contributions, please make sure you're up to date, and then branch from *master*.
When you're done, make a pull request (PR) for your branch on Github. At this point, you might
get feedback from another developer, but for now we do not require that: You can merge your
own PR back into master.

As the project develops we will used tagged releases, following
[semantic versioning](http://semver.org/) conventions, but that is not necessary for every merge.

## Documentation

Each Phoveo repo has a brief README, but documentation of the over-all project can be found here,
as you've already discovered. The source code for the documentation is itself available
[on github](https://github.com/phovea/phovea.github.io). READMEs, this documentation, and Github
issue reports all use [Markdown](https://guides.github.com/features/mastering-markdown/).
Although HTML can be used within Markdown, it is usually not a good idea.

This documentation is hosted by the [Github pages](https://pages.github.com/) platform: Beyond the
basic Markdown, [Jekyll](https://jekyllrb.com/) and 
[Liquid Templates](http://shopify.github.io/liquid/basics/introduction/) are used to create a 
complete site. If you are just contributing documentation, these details won't matter, except for 
one gotcha: New pages must include YAML front matter if they are to be recognized by Jekyll. 
At the top of each file there should be lines like this:

```
---
layout: documentation
title:  Prerequisites
order: 1
---
```

If you find a mistake in the documentation, please correct it. For small edits, it might be easier
to use Github's online editor, but please do save your changes in a branch and create a PR, rather
than committing to master: This gives the system time to do a basic syntax check.

## Packaging

## Server

## Client

## Testing