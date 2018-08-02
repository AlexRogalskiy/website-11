+++
title = "This Week in Amethyst 1"
description = "First issue of This Week in Amethyst."
date = 2016-01-18
aliases = ["/2016/01/19/twia-1.html"]
+++
> News from Jan 11, 2016 – Jan 18, 2016

Hello and welcome to the first official issue of *This Week in Amethyst*, a blog
bringing you the latest changes and updates regarding the Amethyst game engine
every week. If you have any suggestions or ideas, feel free to call me out on
[Gitter][gc].

[gc]: https://gitter.im/amethyst/general

[One major pull request][ep] landed this week.

[ep]: https://github.com/amethyst/amethyst/pulls?q=is:pr+closed:2016-01-11..2016-01-18

## What's cooking on master?

### Notable additions

* [State transitions are finished][e6] in the pushdown automaton code!
* [Official vision for the Amethyst project][av] has been codified in the
  `README.md` file, along with a multitude of links.
* [Issue labeling has been codified][il] for all issue trackers in the
  Contribution Guidelines.
* [Issue #1 for Amethyst-CLI has been resolved.][t1] Cargo output now prints in
  real-time.

[e6]: https://github.com/amethyst/amethyst/pull/6
[av]: https://github.com/amethyst/amethyst#vision
[il]: https://github.com/amethyst/amethyst/blob/master/CONTRIBUTING.md#submitting-issues
[t1]: https://github.com/amethyst/amethyst_cli/issues/1

## New issues

* [Redesign GPU state management (issue #7)][e7]
* [Code name for 1.0 release (issue #8)][e8]
* [Write unit tests for state machine (issue #9)][e9]

[e7]: https://github.com/amethyst/amethyst/issues/7
[e8]: https://github.com/amethyst/amethyst/issues/8
[e9]: https://github.com/amethyst/amethyst/issues/9

## New contributors

No new people have joined this week!

## Other announcements

Right now, there are some experiments being done on the [ecs branch][ec]
combining the traditional entity-component-system with
[tomaka's suggested MVC-with-parallelism design.][mv].

[ec]: https://github.com/amethyst/amethyst/tree/ecs
[mv]: https://www.reddit.com/r/rust/comments/415a1x/github_ebkalderonamethyst_dataoriented_game/cz0dgf1

Clearly, the `World` struct containing our entities and components is
analogous to the *model* in MVC. But systems typically require mutable state for
strictly read-only operations, so they don't fit into either *views* or
*controllers*, nor are they easy to parallelize.

However, there are two possible ways to remedy this issue.

1. Systems are split into separate traits: Updaters and Viewers. The Viewers
   only require read access to the `World`, so they can run in parallel.
   Updaters request mutable access to the `World` and submit a list of changes
   they want to make to the content. These changes are applied serially
   according to a predetermined priority.
2. Systems are kept as one unit (perhaps renamed to "Processors" for clarity),
   and they all read the `World` struct in parallel but can optionally submit
   changes to the `World` which are applied serially once everyone has finished
   reading.

What do you think about these approaches? Leave your thoughts in the
[Gitter chat][gc].