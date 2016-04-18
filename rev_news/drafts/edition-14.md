---
title: Git Rev News Edition 14 (April 20th, 2016)
layout: default
date: 2016-04-20 21:06:51 +0100
author: chriscool
categories: [news]
navbar: false
---

## Git Rev News: Edition 14 (April 20th, 2016)

Welcome to the 14th edition of [Git Rev News](http://git.github.io/rev_news/rev_news/),
a digest of all things Git. For our goals, the archives, the way we work, and how to contribute or to
subscribe, see [the Git Rev News page](http://git.github.io/rev_news/rev_news/) on [git.github.io](http://git.github.io).

This edition covers what happened during the month of March 2016 and
also what happened at the Git Contributor Summit on April 4 2016 and
at the Git Merge conference on April 5 2016.

## Discussions

### General

* Discussions at the Git Contributor Summit, part 1, about big repos and big files

The Git Contributor Summit happened on April 4th at the panoramic
rooftop bar of
[the citizenM hotel in New York City, USA](https://www.citizenm.com/destinations/new-york/new-york-times-square).
The whole bar had been reserved by GitHub for the Summit from 10am to
4pm.

Around 20 developers attended. Drinks were provided, and food was
served during the lunch break. Afterwards GitHub also invited
attendants to a diner at a nearby Italian restaurant.

As usual it was an unconference. Attendants interested to discuss a
topic wrote it on a board or voted on the board for it.

The topics that attracted most votes were about performance on big
repositories.

First it was said that it is unfortunate that the performance of many
git commands, like for example `git log`, depends linearly on the
number of refs in the repository.

One possible solution to this problem would be to use some kind of
binary search algorithm to looks up refs, but it's not so easy because
replace refs in 'refs/replace/' should anyway all be read and also
because with the current backend some stat(2) calls are needed to
check if there are loose refs.

This problem should at least partially be solved with the current work
going on to implement other ref backends, especially a
[lmdb](http://symas.com/mdb/) based one.

(This work on an lmdb based ref backend has been reported on in
[Git Rev News edition 12](http://git.github.io/rev_news/2016/02/10/edition-12/) and
[Git Rev News edition 7](http://git.github.io/rev_news/2015/09/09/edition-7/).)

From there the discussion switched to the case sensitiveness of ref
names and the different problems created by having ref names that
should also be proper filenames: slow filesystems like NTFS, unicode
normalizing filesystems like HFS+, file/directory collisions, reflog
deletion...

It was then mentioned that the tree object lookup could be speed up,
but it would require a more efficient packfile format.

The current effort to implement an `index-helper` daemon to speed up
index reading was also described.

Then some time was spent discussing large objects. There are objects
that can be stored locally, other that cannot. This means that for
example doing something like rsync that manages chunk of files might
not always be possible.

About Git LFS that is now offered by at least GitHub and Microsoft,
there are still a number of problems with it from a user
perspective. First it is not shipped with Git. Another one is that it
is not easy to know which files should go into it.

Also it looks like the filters that are used by git LFS to trigger big
file downloads are executed sequencially which is not good for
performance.

This made people mention potential problems with parallelizing
`git checkout`.

One possibility to improve on Git LFS would be to create another kind
of backend for git objects that would be optimized for large files and
would sit along loose objects and packfiles.

Then the subject switched to possible ways to speed up git status. The
`index-helper` daemon effort was described again, as it can use
[the watchman library](https://facebook.github.io/watchman/) to
efficiently monitor the working tree for changes. The watchman service
must be run manually for now though. And running daemons on Windows
might require some admin rights.

The recently merged effort on improving the untracked cache in the
index was also mentionned.

* [Linux Kernel Development - Going Faster Than You Think](https://github.com/gregkh/kernel-development)

The [Git Merge conference](http://git-merge.com/) happened this year
on April 5th at [New World Stages](http://newworldstages.com/) in New
York City, USA.

The first presentation of [the packed schedule](http://git-merge.com/#schedule)
was given by [Greg Kroah-Hartman](https://en.wikipedia.org/wiki/Greg_Kroah-Hartman)
and was about how the Linux Kernel developers are using Git.

(The slides are available in
[Greg's Github repo for this presentation](https://github.com/gregkh/kernel-development)
either in
[pdf format](https://github.com/gregkh/kernel-development/blob/master/kernel-git.pdf) or in
[odp format](https://github.com/gregkh/kernel-development/blob/master/kernel-git.odp).)

Greg works at the [Linux Foundation](http://www.linuxfoundation.org/).
He is the maintainer of the Kernel '-stable' branches and of many
subsystems like USB.

He said that the Linux Kernel is made of more than 21 million lines of
code in more than 53 000 files. Everything is in the tree. Driver are
one third of the size. Nearly 4000 developers and around 400 companies
are involved.

This makes the Linux Kernel the largest software project ever.

Around 10 000 lines are added, 5300 lines are removed and 1800 lines
are modified, everyday!

That's on average 7.8 changes per hour accross the whole tree with 5%
in the core, 10% in the networking subsystem and 55% in the drivers.

This goes against any previously thought methodology for stable
software. And things are going faster and faster.

Things are going so fast that it costs money to keep your code outside
the kernel.

There is a new release every 2.5 months, so if your code get rejected
you have to wait 2 months before it can be in the next release. This
is very predictable.

The release cycle is made of a two week long "merge window" and then
some "rc" releases one per week. During the "merge window", code is
merged from subsystem maintainers. The "rc" releases, "rc-1", "rc-2",
... , "rc-7" are bug fixes only. Once all major bugs and regressions
are fixed a release is made and the cyce starts over with a ne merge
window.

For "stable" kernels that Greg maintains, they are forked from Linux
releases. Commits have to go in Linus' tree first before Greg will
accept them. It should be the identical patch as what is in Linus'
tree and it should be bug fixes only or new device ids. The Linux
distributions usually run from "stable" kernels.

The "longterm" kernel is maintained for 2 years. Current "longterm"
kernels are 3.14, 4.1 and 4.4.

This works well for many companies. But sometimes maintaining a kernel
for 2 years is not enough. Japan is converting all its infrastructure
to Linux and people want some kernels maintained for 20 years.

The patches sent for inclusion into the Kernel should be standalone.
None of them should break the build. One half to one third of the
patches get accepted. They should be obvious, broken down in the
smallest possible patches and every change should be correct.

This puts work on the developer's shoulders. But that's on purpose
because there are much more developers than maintainers. There are
1000 maintainers but only around 700 are active.

To reach the maintainers, patches should be sent to the relevant
mailing list, like the usb mailing list or the scsi mailing list.
Andrew Morton reads the lkml mailing list.

The email format should be plain text. It's old school but works very
very well. The Developer's Certificate of Origin (DCO) is used to
ensure that people have the right to submit a patch. This is the same
as what Git does.

After a patch has been reviewed by a file or driver maintainer, the
maintainer will add his own "Signed-off-by" and will send it to a
subsystem maintainer.

There are around 200 subsystem maintainers. They have their own trees
on git.kernel.org.

Every night the "linux-next" is created from these 200 trees and is
built and boot tested on different platforms.

Andrew Morton uses quilt to maintain his "-mm" kernels made from
patches picked by himself from the lkml mailing list or elsewhere.

A couple years ago Kernel developers realized that nobody actually
tested the kernel.

There is now a 0 day bot that run tests and static analysis tools,
like coccinelle or sparse, automatically on all the kernel trees. It
tests all the commits. It has a script that writes patches for common
problems. The bot also picks patches from mailing lists to test them.

When a new merge window opens, the subsystem maintainers and Andrew
Morton send what they think is ready to Linus, so he can merge it. The
rule is that what is sent to Linus should have already been in
linux-next. 10 000 to 11 000 patches are usually merged in the 2 week
long merge window.

<!--- ### Reviews -->

<!---
### Support
-->

## Releases


## Other News

__Various__


__Light reading__


__Git tools and sites__


## Credits

This edition of Git Rev News was curated by Christian Couder &lt;<christian.couder@gmail.com>&gt;,
Thomas Ferris Nicolaisen &lt;<tfnico@gmail.com>&gt; and Nicola Paolucci &lt;<npaolucci@atlassian.com>&gt;,
with help from XXX.