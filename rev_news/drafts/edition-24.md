---
title: Git Rev News Edition 24 (February 22nd, 2017)
layout: default
date: 2017-02-22 12:06:51 +0100
author: chriscool
categories: [news]
navbar: false
---

## Git Rev News: Edition 24 (February 22nd, 2017)

Welcome to the 24th edition of [Git Rev News](https://git.github.io/rev_news/rev_news/),
a digest of all things Git. For our goals, the archives, the way we work, and how to contribute or to
subscribe, see [the Git Rev News page](https://git.github.io/rev_news/rev_news/) on [git.github.io](http://git.github.io).

This edition covers what happened during the month of January 2017 and
the [Git Merge 2017](http://git-merge.com/) conference which happened in Brussels on February 2nd and 3rd 2017.

## Discussions

### General

* Git Contributor Summit

  The [Git Merge 2017](http://git-merge.com) conference took place in
  Brussels on February 2nd (workshops and contributor summit) and 3rd
  (main conference day). Quite some of the attendees seamlessly joined
  the [FOSDEM](https://fosdem.org/2017/) think tank right after the
  Git Merge after-party. For those interested in the "runtime side of
  life", the [Config Management Camp](http://cfgmgmtcamp.eu) in Ghent
  offered yet another 2 days to get exhausted.

  On February 2nd, as part of the Git Merge, a Git Contributor Summit
  had been organized. While both Lars Kruse's and Ed Thomson's blogs,
  listed in the "Events" section below, very nicely express the great
  atmosphere at the summit, and Johannes Schindelin, alias Dscho,
  [sent](http://public-inbox.org/git/alpine.DEB.2.20.1702021007460.3496@virtualbox/)
  a [collaborative report](https://docs.google.com/document/d/1KDoSn4btbK5VJCVld32he29U0pUeFGhpFxyx9ZJDDB0/edit)
  written by the contributors, here are the unconference's agenda items:

	* Erik van Zijst presented Atlassian's "clone bundles"
          approach, which had been added to Mercurial some years ago,
          was later included in Bitbucket.

		* Atlassian would like to add it to Git, but would
                  like to discuss the approach first.

		* Stefan Saasen had sent
		  [an email about it](http://public-inbox.org/git/CADoxLGPFgF7W4XJzt0X+xFJDoN6RmfFGx_96MO9GPSSOjDK0EQ@mail.gmail.com/)
		  a few days before the Summit.

		* The main motivation for this feature are CPU and I/O
                  usage on the server side.

		* The feature would be good for large collections of
                  repos, e.g. the set for Android; the Google "repo"
                  tool would be an alternative.

		* One alternative would be the design of the Git
                  protocol version 2, with capabilities negotiation
                  first.

		* Jeff King, alias Peff, suggested to keep the
                  solution as simple as possible (as an example, a
                  sliced history etc. would be nice to have, but
                  things could get quite complicated).

		* One downside is that it could take twice the disk
                  space if the bundle is not generated on the fly.

		* At present, the Atlassian client is a script
                  mimicking `git clone`, while a proper solution would
                  involve the Git client.

		* Peff expressed his willingness to help Atlassian on
                  this subject.

		* The solution is experienced as more efficient, but
                  no real statistics / conclusions for real world
                  repos are available yet.

	* Jeff King, alias Peff, presented a "1 minute version" of the
          current status of the Software Freedom Conservancy; details
          available [on the mailing list](http://public-inbox.org/git/20170202024501.57hrw4657tsqerqq@sigill.intra.peff.net/).
	  (See also the next article in this edition of Git Rev News.)

		* The SFC acquired git-scm.com, maintained by Peff.

		* Most effort is spent on trademark/policy work.

	* Christian Couder and Ævar Bjarmason presented "Big repos".

		* The topic is very related to the „references database“
		  item below, and deals with improved rebase / staging
		  (split index).

		* The main idea is to make the git client faster for
                  some index related operations (that is, `git status`).

		* Some work on a daemon for file system notifications
                  in the background (inotify) has been done by Duy
                  Nguyen and David Turner, and a rework was planned,
                  but the present contributor is currently busy with
                  other things.

		* On Windows, there is no inotify feature available
                  (also applies to some Unix flavours).

		* Some discussion arose whether the Windows journal
                  feature or
                  [Watchman](https://facebook.github.io/watchman/) was
                  the appropriate road to follow.

	The participants with really large repos shared their
	experience.

	* Stefan Beller presented the `git submodule` state.

		* Instead of `git submodules`, Android with its 1000
                  Git repos uses the [`repo`][repo] tool; the Android
                  repositories are somewhat unrelated, and most
                  contributors just work in 1 or 2 repos.

		* Regarding submodules, actually the fetch is
                  parallelized, checkout is being worked on, next are
                  worktree and submodules.

		* Inside Git, there is no real module awareness for
                  submodules yet, so each and every Git module needs
                  to be made „submodule ready“.

		* At present, one Git process is spawned for each
                  repo, which is quite slow on Windows.

		* The submodules design is considered not to be
                  optimal yet; as an example, subsets are not
                  possible.

		* Within Gerrit, project changes lead to superproject
                  changes, which might collide, and end up in
                  superproject merge conflict mess.

		* David Turner considered merge conflict helpers for
                  submodules.

		* Johannes Schindelin remarked that offloading such
                  issues to tools like Gerrit looks suboptimal.

		* The topic was concluded with a short discussion on
                  `git bisect` with submodules.

	* Jacob Vosmaer presented the GitLab solution for [Gitaly][], aka
          „Git RPC“.

		* GitLab tries to get away from NFS, and would like to
                  see a LRU disk cache, as the NFS cache not good
                  enough for heavy use.

		* The question is how far only GitLab is affected
                  (vs. the general community).

		* Use case example are bad refs and storing diffs.

		* The work on the Git cache started, but is stalled at
                  present; it will be MIT licensed (uses both Git and
                  Mercurial code).

	* Johannes Schindelin started a brainstorming session about
          "Better tools for reviews and contributions"; see also
		  his post [Clarification about "Better tooling for reviews", was Re: Google Doc about the Contributors' Summit](https://public-inbox.org/git/alpine.DEB.2.20.1702151215570.3496@virtualbox/)

		* The mailing list currently drops contributions via
                  mailers which produce HTML or mixed code. It is too
                  difficult for people to supply patches by mail; the
                  “it worked for me, should work for others, too" approach
                  (“and then everybody failing is stupid”) is considered
                  simply wrong. One of the rejected clients is
                  Microsoft Outlook, the other is Gmail, the main
                  problem being the handling of white space. Perhaps
                  some tool might help with transmitting code
                  corrections with white space. In general, patches
                  would be better attached to mails than just placed
                  inline.

		* It is problematic to follow long threads (e.g. one
                  thread stalling for 8 months), while the state of
                  patches discussed is sometimes unclear.

		* Often, reviewers' suggestions are lost in the clutter
                  of the mailing list threads, and no tool helps us to
                  catch those issues.

		* It happens from time to time that the latest iteration
                  of a patch series was not picked up before merging to
                  `master`, and we have no tools that help us prevent that.

		* The [“What’s cooking” emails](https://github.com/git/git/blob/v2.11.1/Documentation/SubmittingPatches#L384-L386)
                  are sent only to the mailing list, but none of the original
                  contributors are Cc:ed in that email. At present, the
                  “what’s cooking” email is mostly generated in an automated
                  way from the [`todo` branch](https://github.com/git/git/commits/todo)
                  using special-purpose shell scripts.

		* Is there a way to get from the mail client directly
                  into the mentioned source code?

		* The Git terminology is sometimes strange ("outside this
                  room, *nobody* uses the term 're-roll'"), the Git
                  glossary is hard to find, and could be improved; using
                  terms only the email community uses does not improve
                  readability for others outside the list
                  “inhabitants”.

		* Johannes regularly submits other peoples’ patches,
                  and would like to see better tool support for this
                  use case. Peff asked whether we need different
                  terms, or better concepts?

	Regarding patches, the maintainer decides when and where they are merged / implemented.
	The maintainer workflow may prevent us from just using GitHub Pull Requests.
	Whether to merge or rebase is then finally a manual decision of the maintainer,
	independently of what the author intended.
	A longer discussion arose around this subject; Stefan Beller
	mentioned that a similar problem appeared in the Linux Kernel
	world (especially on the Intel side), and now the Kernel
	community says "it can be ok to attach patches if you cannot
	do otherwise".
	Johannes emphasized his willingness to establish tools which help to improve the situation.
	It was mentioned that code/commit notes should point to mailing list where appropriate.
        Josh Triplett steered the discussion toward distributed review tools, and mentioned that
        there is already a data format for that purpose: ReviewDB. This format, however,
        would need to be extended to allow for addressing the issues raised in the discussion.

	* Carlos Martín Nieto presented the state of the references
          database, to get them away from the file system.

		* A recurring problem seems to be that usable
                  databases do not have a Java implementation
                  (e.g. [lmdb][] last tried); this would help GitHub but
                  still have the potential to split the Git
                  community.

		* According to Peff, Git currently reads the whole
                  packref file into memory – mmap could help but would
                  need a lot of refactoring.

		* An important question is whether it is a client or a
                  GitHub hosting problem; some workflows could cause
                  unhealthy numbers of refs on a client, too.

		* At present, not all race conditions fixed yet. The
                  final solution should be portable to JGit / libgit2.

		* After the efforts of David Turner, there is now an
                  appropriate ref API within Git.

	* Brendan Forster presented the gitignore „spec“.

		* The goal is to be more standardized and robust (that
                  is, understandable).

		* Docs should be more structured.

		* One problem is that different implementations of
                  Git, and other tools use gitignore, but implement
                  the stuff differently.

		* An interesting question would be if we could have a
                  common minimal implementation of gitignore in a
                  generally reusable library.

		* Git attributes has similar problems (reading
                  attributes from files in the Index is a nightmare,
                  in some case with read / change / read / change /
                  read sequences (or the like) involved).

	* Josh Triplett presented "Git commit refs, tag refs, and
          compatibility (future of [git-series][])".

		* Junio has suggested a different type of Git refs
                  (in-tree, like hard links).

		* Compatibility: support old clients / libgit either
                  on repo level, or unless an operation hits an object
                  type unknown for the old version (the latter being
                  more complicated).

		* Use cases should be implemented as easy as possible.

		* Would be great to disallow the capability per repo.

	* Other topics:

		* Planned Open Source Hackathons in London and New
                  York in April/May. The intent is to get some stuff
                  in on that day, also recruitment of future
                  contributors. Libgit2 should perhaps be included
                  there. Appropriate contributors should be on board
                  guiding the process.

[repo]: https://code.google.com/p/git-repo/ "repo - The multiple repository tool"
[Gitaly]: https://gitlab.com/gitlab-org/gitaly/
[lmdb]: https://symas.com/offerings/lightning-memory-mapped-database/ "Lightning Memory-mapped Database"
[git-series]: https://github.com/git-series/git-series "git-series: Track changes to a patch series over time"

* [Git / Software Freedom Conservancy status report](http://public-inbox.org/git/20170202024501.57hrw4657tsqerqq@sigill.intra.peff.net/)

At the previous Git Contributor Summits which were also part of the
previous Git Merge conferences, Jeff King, alias Peff, used to give a
talk about the status of the Git project as part of the
[Software Freedom Conservancy](https://sfconservancy.org/).

It used to be a detailed talk about the different aspects of what the
Git "Project Leadership Committee" (PLC) which represents Git in
the Conservancy and the Conservancy itself are doing.

This is an important report as the PLC consists only of Junio Hamano,
Shawn Pearce, and Peff, so most the community otherwise doesn't know
what happens in some areas of the Git project, like legal and money
related things.

This year, though, Peff sent detailed reports in emails to the mailing
list just before the Contributor Summit, and at the Summit his one
minute long talk consisted in referring everyone to these emails.

The [main report](http://public-inbox.org/git/20170202024501.57hrw4657tsqerqq@sigill.intra.peff.net/)
gives a bit of background, and then details the financials of the project.
There is not a lot of news there, as the Git project is not much
interested in raising fund as it has not a lot of expenses.
The main report also talks a bit about mentoring, but for the git-scm.com domain
and trademark activities, it refers to separate emails that Peff sent
at around the same time as the main one.

The ["git-scm.com status report" email](http://public-inbox.org/git/20170202023349.7fopb3a6pc6dkcmd@sigill.intra.peff.net/)
is indeed quite long and informative. It was surprising to learn that
the Git project got control of the git-scm.com and git-scm.org domains
and the associated web site this year, and interesting to know what
technologies and hosting services the web site has been using.

This report was then [posted and discussed](https://news.ycombinator.com/item?id=13554065)
on Hacker News. This in turn seems to have attracted a number of new people toward the project.
Some of them replied to Peff's email and proposed to help him maintain
and improve the web site.

Other people seem to have been the reason why Peff subsequently sent an email about
[Software Freedom Conservancy donations](http://public-inbox.org/git/20170208183444.wlk4vjveg7cmmi5a@sigill.intra.peff.net/)
telling that "a lot of people offered financial assistance" and that
"we _don't_ have a dire need for money to keep hosting the site", and
pointing people back to the main report he previously sent, as well as
suggesting people donate directly to [Conservancy's general fund](https://sfconservancy.org/donate/)
or [become Conservancy Supporters](https://sfconservancy.org/supporter/).

The email that Peff sent about
["Git trademark status and policy"](http://public-inbox.org/git/20170202022655.2jwvudhvo4hmueaw@sigill.intra.peff.net/)
had also a lot of interesting details, but has not attracted much public interest.


* Git Merge 2017 — General Sessions

  * "Intro & Welcome - Software Freedom Conservancy"
    Karen Sandler, _Software Freedom Conservancy_

    Karen wrote about her intro talk in [Git Merge and FOSDEM 2017!](https://sfconservancy.org/blog/2017/feb/17/git-merge-fosdem/):

    > For me, FOSDEM this year started two days early with Git Merge, the annual Git conference. Git Merge is organized by GitHub, and so far in all three years of its organization the conference has donated the proceeds from ticket sales to Conservancy! I’d been hoping to get to Git Merge one of these years, so I was very excited with the organizing team asked me to do an talk introducing Conservancy.

    > I got to kick off the conference, and introduced myself by explaining how investigating my heart condition and defibrillator caused me to become passionate about software freedom. I then delved into what Conservancy does and in particular talked about some of the work we’ve done with Git. The talk had a good impact, and all day long I was able to speak with people who were excited about Conservancy and thinking about the ethics of all of our software. It’s always especially thrilling to speak at our member projects’ conferences. I love meeting up with leadership committee members and also putting faces to the names that we see go by while monitoring the activities of our projects.

    Karen has a pacemaker (implanted defibrillator) and was worried
    about what software it runs on.  Everyone knows that software is
    potentially buggy.  She asked to see the source code, but was
    refused, as the software is proprietary.

    That was the kick-off for starting
    the [Software Freedom Conservancy](https://sfconservancy.org/).
    The conservancy does legal work, handle foundations and
    fundraising. They give advise on licensing and trademark
    protections.  Git is one of SFC member projects.

    Karen's defibrillator is a metaphor for all the software out there
    that we rely on - as she said:

    > **We’re only as free as the software that we use.**


  * "Top Ten Worst Repositories to host on GitHub"
    Carlos Martin Nieto, _GitHub_

    Carlos give insights into some of the challenges that GitHub meets
    when hosting their 14 million user infrastructure. What makes repo
    "the worst" varies: a huge amount of files, data, forks, tags,
    contributors or pushes.

    Here are some of the worst Carlos showcased:

     * An everyday example is a community that is organizing commit
       wars; thousands of users committing to the same repo, at the
       same time, so see who gets in:  
       [commitwars/commitwars](https://github.com/commitwars/commitwars)

     * Another is repository hosting mirror of 9gag, with 365GB of
       data, and 300-400MB pushed every 10 minutes:  
	   [gambuzzi/gambuzzi.github.io](https://github.com/gambuzzi/gambuzzi.github.io)

     * Others are just large networks:
       * [torvalds/linux](https://github.com/torvalds/linux) is forked
         16K times; note that it also shows "∞ contributors".
       * [octocat/Spoon-Knife](https://github.com/octocat/Spoon-Knife)
         is GitHub's sample repo, used to illustrate what forking is
         all about - it’s forked 88K times
       * [barryclark/jekyll-now](https://github.com/barryclark/jekyll-now),
         where each fork is someone’s website, forked 13K times

     * There are projects that are just large, with long history
       and/or large number of files,
       like [torvalds/linux](https://github.com/torvalds/linux), with
       42GB data on disk.

     * Other just go to the extreme with the use of GitHub-based
       workflow, that is with extensive use of pull-requests and
       issues, like [kubernetes/kubernetes](https://github.com/kubernetes/kubernetes)
       with 24K pull requests in total, and 5K+ open issues, plus 12K
       closed

     * Some have just large number of commits,
       like [CocoaPods/Specs](https://github.com/CocoaPods/Specs) –
       160K commits; though it is nothing on Linux, with 650K commits.

    Carlos has this to say about first two:

    > Please don't


  * "Scaling Git at Microsoft"
    Saeed Noursalehi, _Microsoft_

    Saeed talked about scaling Git at Microsoft with more that 30K
    developers.  When they started with the [humongous Windows repository](https://arstechnica.com/information-technology/2017/02/microsoft-hosts-the-windows-source-in-a-monstrous-300gb-git-repository/)
    it would literally take (almost) days to clone the repository.
    Git's assumptions generally work well for reasonable
    repositories - but it is still a problem to be solved.
    It was beyond help of Git LFS and similar solutions.

    They have implemented [GVFS - Git Virtual File System](https://blogs.msdn.microsoft.com/visualstudioalm/2017/02/03/announcing-gvfs-git-virtual-file-system/);
    which is an [open-source project](https://github.com/Microsoft/GVFS),
    just released by Microsoft.  This helped them a lot with this
    Windows repo:

    > Git experience on Windows repo (with GVFS)
    >
    >   ~~12 hrs~~ 5 mins    - clone  
    >   ~~3 hrs~~ 30 secs    - checkout  
    >   ~~8 mins~~ 4 secs    - status  
    >   ~~30 mins~~ 13 secs  - commit

    GVFS is basically a file system driver combined with a read-object
    hook in Git, and a persistent daemon process driving said hook.
    The read-object hook is a new concept that Microsoft has
    introduced to Git.  This solution allows for (their fork of) Git
    to populate files and download objects (commits, trees, blobs) on
    demand.

    Saeed started a live demo on a VSO.  He has shown how the files
    got populated ("hydrated"), that is their content downloaded, on
    first access (on first use).  You could also see live the
    JSON-based protocol used to communicate with GVFS daemon
    (service).

    The trees doesn’t grow in to the skies - when asked about
    searching globally in your repo - answer was “_Yes, that will
    still be very painful_”.  Browsing history was also a problem, as
    it would "hydrate" objects; though in Q&amp;A session there was a
    suggestion of running `git log` on server, like for centralized
    version control systems.

    Currently GVFS is limited to MS Windows, and requires hacked Git.
    Microsoft also cares about other platforms: the message is “_We’re
    hiring - especially if you know file systems on Mac and Linux_”.
    Microsoft is also working on core Git integration: see the
    discussion in thread started by Bob Peart
    [[RFC] Add support for downloading blobs on demand](https://public-inbox.org/git/20170113155253.1644-1-benpeart@microsoft.com/).


  * "What’s Wrong With Git?"
    Santiago Perez De Rosso, _Software Design Group, MIT_

    According to Santiago, Git is hard to learn and the documentation
    is scary.  He presented one after another the slide with on-line
    version of [Git documentation](https://www.kernel.org/pub/software/scm/git/docs/),
    and the [fake Git-manpage generator](https://git-man-page-generator.lokaltog.net/).
    There was also a jokingly provoking slide on how commits can
    easily be represented as a manifold in a multi dimensional space.

    In his PhD work, Santiago has been surveying user response to the
    usability of Git, and experimenting with fundamental improvements
    to the Git experience.  In his talk, he covered a new theory of
    top-down design, focusing on the purposes and concepts underlying
    software; the material expanding on [Purposes, Concepts, Misfits, and a Redesign of Git](http://people.csail.mit.edu/sperezde/pre-print-oopsla16.pdf)
    paper - covered in [Git Rev News: Edition 20](https://git.github.io/rev_news/2016/10/19/edition-20/).
    Santiago showed the complexity of understanding concepts like
    _stash_, _detached head_, and _untracked files_, and how they
    relate (or not) to concepts and purposes of an VCS.

    In his experiment of improving Git usage, he
    presented [Gitless](http://gitless.com/), an experimental version
    control system built on top of Git (also covered in edition 20).
    In this DVCS, a branch includes it’s working dir, so dirty files
    never prevent a branch switch. Other similar changes were made to
    the other identified confusing topics. Gitless was then used to
    run parallel experiments with non-expert and expert Git users to
    compare how learning Gitless compared to Git.

    Gitless was just an experiment, but Santiago suggested that the
    world might be ready for new VCSs, or at least a new porcelain or
    GUI on top of Git, based on this research.


  * "Git: The Tool Loved and (sometimes) Feared"
    Caren Garcia, _BazaarVoice_

    Caren was sharing her experiences teaching Git.  According to
    Caren, Git conflicts induces panic states in students. The command
    order confuses them, but they love the utility of having
    everything in Git.

    Caren recommends the web site [OhShitGit](http://ohshitgit.com/)
    the get help to get out of panic situations.  There is always a
    possibility of screwing up, and this site would help you to figure
    how to fix your mistakes.  (This site was covered in
    [Git Rev News: Edition 19](https://git.github.io/rev_news/2016/10/19/edition-19/))).
    Her students has had great success using Git CLI for learning Git.

    She also wants everyone to use Git - writers, governments and
    school.  Git is more than a software; a concept, a way of
    thinking, and because of that it can be used it outside the IT
    field.


  * "Scaling Mercurial at Facebook: Insights from the Other Side"
    Durham Goode, _Facebook_

    Durham talked about how Mercurial (Hg) has been scaled at
    Facebook, and about the way software version control is organized
    there.  (Reasons for choosing Mercurial can be found in
    [Scaling Mercurial at Facebook](https://code.facebook.com/posts/218678814984400/scaling-mercurial-at-facebook/)
    blog post from 2014 by Durham Goode and Siddharth P. Agarwal;
    [an update](https://groups.google.com/forum/#!topic/mozilla.dev.version-control/nh4fITFlEMk)
    on how they are using it internally was covered
    in [Git Rev News: Edition 21](https://git.github.io/rev_news/2016/11/16/edition-21/).)

    The Facebook development environment composed of monorepos, no
    feature branches, rebases and no merges, single commits per
    push.  They assume that all developers work online, that “_everyone
    commits on master_” (in order to overcome their tremendous size),
    and that “_every commit is pushed_”.  The size of their version
    control support team is very small compared to the large number of
    developers they employ - therefore they need simple workflow, and
    the amount of time for teaching version control is limited.

    To solve the issue of push conflicts, they have implemented
    `PushRebase`, which basically allows the server to perform your
    rebase for you in some conditions, eliminating the problem of push
    being rejected because the local repository is not up to date
    (someone pushed first).

    They tend to assume their developers are online; thus they
    implemented `InifinityPush`, which assures that every commit ever
    is pushed (helped with new scalable way of storing data).  This
    solves many problems of error recovery.

    The implementation (fork) of Mercurial that Facebook uses allows
    to check out only part of the code that is actually needed.  This
    is especially needed for the monorepos.  The feature is similar to
    Git's sparse checkout; the important difference is a far better
    UX.  Their implementation allows for example easy switching
    between pre-defined (via in-repository file with known format)
    sets of files, selected for well-defined tasks.  This is something
    worth porting to Git, to improve its sparse checkout feature.

    The question of User Experience (UX) is very important for
    Facebook.  Durham asks: “_Why isn’t the default behavior of the
    git log useful? - Why do we all have to go through hoops to
    customize the log enough to make it useful?_”.  They have made
    `SmartLog`, which allows to see only useful commits for a given
    developers.  It also allows to easily recover for instance commits
    that are not reachable from any branches, without need for extra
    knowledge and to reach for any advanced features.  This is
    something worth borrowing; [git-log-compact](https://mackyle.github.io/git-log-compact/)
    is a project that implements something similar (covered in detail
    by [Git Rev News: Edition 20](https://git.github.io/rev_news/2016/10/19/edition-20/)).

    Durham has emphasized the need to focus on the UX, the need to
    enable new users to do power user moves. He uses `hg unamend` and
    `hg uncommit` as good examples for that.

    At the end, the critical crowd asked “_Why isn't this Open
    Source_” to which the one acceptable answer came… “_It is!_”.
    Facebook runs the latest stable Mercurial build and hereby catches
    a lot of Mercurial bugs before they hit others.  Not everything is
    contributed back to hg-core, mostly due to the fact that not all
    contributions they make are backward compatible.


  * "Git LFS at Light Speed"
    Lars Schneider, _Autodesk_ and Taylor Blau, _GitHub_

    This was a two part presentation, about a way to dramatically
    improve performance in a popular Git extension - LFS (Large File
    Storage) - that required changes in both Git Core, and the
    extension itself.

    In first part Lars walked through how to contribute to Git.  You
    can find description of (part of) his experience, namely
    developing and reviewing the "Git filter protocol" feature, in
    [Git Rev News: Edition 18](https://git.github.io/rev_news/2016/08/17/edition-18/).
    "Developer Spotlight: Lars Schneider" can be also found in this
    edition.

    It’s not just about implementing the feature; it needs to be
    documented, and tested as well.  The commit message should be
    self-contained, meaning among others that you need no external
    dependencies to figure out what the commit does.  There are many
    steps that one has to go through, and for such far reaching
    feature (with the need for backward compatibility) also usually
    many iterations.  As an example of the friction in this approach,
    Lars gave us the example: this patch series gave a very hefty
    performance improvement... and it took him 6 months and 380 emails
    to get it approved.

    > Git is used by millions all over the world, you don’t want to be
    > the one breaking it.

    In second part Taylor took over to talk a bit about the server
    side of Git LFS.  Go was used as a language of choice for
    implementing it.  Here using the Scanner pattern in Go turned out
    to be a perfect match for the way Git plumbing passes information
    to Git-LFS process on server.  He shared description of
    refactoring performed to speed up the operation.

    All this of course needed to be synchronized across different
    projects, different programming languages, and different
    developers.

    At the end, Taylor shared the ideas for next steps for Git-LFS
    server, namely: Promise capability, file-path exchange, and work
    on figuring out how to make Git-LFS even faster.


  * "Git Aliases of the Gods!"
    Tim Pettersen, _Atlassian_

    Aliases are not just about saving time and keystrokes, though as
    Tim said the most basic and common example of using aliases is to
    create shortcuts, for example `git co` for `git commit`, or `git
    st` for `git status`.  It is also about reducing cognitive burden.
    Whenever you find yourself googling the same thing over and over
    (or browsing manpages, or searching StackOverflow) - you should
    consider make an alias.

    As an example of the latter, Tim took us through the tour of four
    different ways that you can create stash, and how that interacts
    with the index and working directory.  The option names are not
    easy to remember; because of the way they were developed there is
    no commonality of names between them.  Therefore he had made 3
    aliases, so that the more `a`s you have in your command the more
    you stash: `stsh`, `stash`, `staash` and `staaash`.

    He created a funny alias called `standup` and `lazy standup`:
    ```INI
    [alias]
    standup = !git log --all \
                       --author=$USER \
                       --since='9am yesterday' \
                       --format=%s
    lazy-standup = !git standup | say
    ```

    Tim has also described how to create more involved aliases; how to
    join more than one command together, and how to pass parameter to
    inside of such pipeline.  He has shown both `sh -c '...' -` trick,
    and immediately invoked shell function trick... while reminding us
    that if alias gets too complicated it is better to just create a
    new git command (by putting `git-sth` executable, for example a
    shell script, in your PATH), rather that worrying about
    appropriate escaping and quoting.

    Tim has also presented an awesome, and very silly way, to spoof
    Git SHA-1’s with the help of emojis and raw processing power.  As
    an example with [git-sham](https://bitbucket.org/tpettersen/git-sham)
    you can make it so that subsequent commits have SHA-1 identifiers
    beginning with subsequent numbers.

    Atlassian's Bitbucket has joined the Continuous Development (CD)
    world for real, providing a lot of what services like GitLab and
    Travis are doing, called there "pipelines".  They have enabled
    [pipelines](https://bitbucket.org/product/features/pipelines)
    in Bitbucket - to allow you to build any branch.

    Tim expertly finishes by combining Bitbucket Pipeline with an
    alias, so he can do with it an equivalent of `git bisect` by just
    running all the steps in the bisect parallel - that is
    brute-forcing tests.  All this without the need of involving the
    developer, at the cost of CPU time (though possibly not wallclock
    time, assuming sufficient parallelism and small number of commits
    to test).  The CPU time is worth less than developer's time.

    You can find aliases used in this presentation in Tim's
    [git-aliases](https://bitbucket.org/tpettersen/git-aliases)
    repository.  (MediaWiki has also a
    [wiki page with the list of useful aliases](https://www.mediawiki.org/wiki/Git/aliases).)


  * "Lightning Talk / Lessons Learned: Transforming from ClearCase to Git"
    Tamir Gefen, _ALMtoolbox_

    This lightning talk presented a case study on moving to Git from
    IBM Rational ClearCase.  Tamir has described challenges they faced
    (among others time to switch and the amount of support, and the
    impedance mismatch between ClearCase and Git) and how they
    resolved them, plus some technical tips and insights.  The
    solution chosen was to adjust ClearCase views one at a time, and
    then dump content on new branches in Git.


  * "Lightning Talk / Git: The Original Blockchain"
    Meredith L. Patterson, _Mautinoa Technologies_

    Meredith shown how Git chosen as the underlying platform for
    implementing a money-less transaction exchange ledge system,
    implemented as an actual monetary system in Sierra Leone, where
    _phone minutes_ is actually considered a viable FX currency, since
    otherwise the national bank would go bankrupt.

    Bitcoin and other blockchain-based distributed ledger systems are
    "gossip protocols", and require always-on Internet access, not
    available in much of Africa.  What is needed is a way to perform
    transactions off-line, with later reconciliation step.  This
    means that the system needs to store timestamps in its
    blockchain... Meredith noticed that is what Git does!  The
    presented solution uses [ledger-cli][] / [hledger][]
    [plain text accounting][plaintextaccounting] format as a format
    for storing transactions, Git commits for blockchain, remotes as
    wallets / accounts, and GPG keys with short expiration dates for
    signing.

    This shows how technologies, such as Git, that do one thing well
    can change the world by doing the thing they do well in unexpected
    places.


  * "Lightning Talk / go-git: A Pure Go Implementation of Git"
    Santiago M. Mola, _source(d)_

    [source{d}](http://sourced.tech/) wants to clone all open-source
    projects from major Git hosting sites (17M repositories planned,
    6M repositories archived), and then analyze all code history using
    AI techniques.

    For this they needed a special-purposes Git implementation.
    Santiago introduced [go-git](https://github.com/src-d/go-git)
    project, a Git library written entirely in Go, designed to be
    developer-friendly and highly extensible.  He has shared some of
    source{d} use cases, and also problems storing (archiving) such a
    large number of repositories, many of which are forks.


  * "Git for Pair Programming"
    Cornelius Schumacher, _SUSE Linux_

    Cornelius started the presentation by describing in detail the
    pair programming workflow; why it is used, and how it is done.
    The important issue is that in this workflow commit has more than
    one author - developers often change even during writing of a
    commit message.  Pairs are not forever; the pairing change quite
    often.

    He described the solution used by the
    [git-duet](https://github.com/git-duet/git-duet), which utilizes
    committer and author to show the identity of both contributors in
    the pair, optionally rotating them.  This tool has its advantages
    and disadvantages; among others it is less useful for mob
    programming.

    Cornelius then went into details with the multiple authors problem
    using anything from emails, commit messages and headers.  The
    sketch of a proposed solution and discussion how Git could get
    native support for pair programming can be found on git mailing
    list as
    [[RFC] Proof of concept: Support multiple authors](https://public-inbox.org/git/1485713194-11782-1-git-send-email-schumacher@kde.org/).
    The thread didn't end in an accepted solution, though when one
    would be created it would most probably follow the
    [Junio's advice](https://public-inbox.org/git/xmqqinowuvd7.fsf@gitster.mtv.corp.google.com/)
    there of utilizing "Co-Authored-by: " trailers in the commit
    message, and extending `git shortlog` and other commands as
    needed.


  * "Trust But Verify"
    Thordur Bjornsson, _Yubico_

    The last talk covered cover the details on how to
    cryptographically sign your work.  Thordur started with PGP
    fundamentals and best practices (among others the need for setting
    an expiration date of PGP keys, to one year at most).  Then he
    went through what can get signed in Git: commits (including merge
    commits), tags and pushes (the last requiring help from the
    server) - though the concept of mergetags was missing from the
    talk.  Then Thordur described how to verify those signatures,
    explaining possible impacts (and the amount of work required), and
    benefits to ones workflow.

    As one way of verifying signatures, Thordur took a look at GitHub’s
    interface changes from [earlier this year](https://github.com/blog/2144-gpg-signature-verification)
    that added visual cues to highlight verification status of commits
    and tags.

    The slides from this talk (and links to other related resources are
    available at [thorduri/git-merge-2017](https://github.com/thorduri/git-merge-2017)
    repository.


[ledger-cli]: http://www.ledger-cli.org/
[hledger]: http://hledger.org/
[plaintextaccounting]: http://plaintextaccounting.org/

<!---
### Reviews
-->

<!---
### Support
-->

## Developer Spotlight: Michael J Gruber

* Who are you and what do you do?

I am a mathematical physicist - I do research on mathematical problems
in quantum physics, and I teach mathematics as a lecturer. Sharing and
free exchange of knowledge are fundamental for that. Consequently, being
involved in open source software projects feels like just another side
of the same medal.

* How did your introduction to Git come about?

For a larger project with multiple moving parts (habilitation thesis) I
had used Subversion (that thesis started before git). It made two things
clear: I could not have done this without a version control system; and
I needed something else (a VCS with actual merges, to say the least).

Git had some geek appeal, but I couldn't get it to compile on my first
attempts (`configure && make...`, on a system with libs without headers,
no root). So I went with Mercurial since I was getting into Python
anyway. Only to be confused by Hg's mantra "to clone is to branch and to
branch is to clone" when there were two commands `clone` and `branch`
which did something completely different and - in the case of the latter -
not very useful, it appeared to me. (Hg has the more useful
"bookmarks" these days.)

In the end, it was the branch concept and the tone on the respective
mailing lists at that time that drove me to Git. I had learned not to
use `configure` for Git by now, and have been compiling it happily ever
after.

* What would you name your most important contribution to Git?

There is no single big topic. Mostly, I try to make Git easier and less
surprising to use by doing stuff here and there. The rev-list options
`--min-parent`, `--max-parent` and `--cherry-mark` were fun to do. I
also consider "--textconv" a killer feature and was very successful in
getting Peff to do most things in that area that I wanted to have, and
did a few things "myself" - which is really the wrong term, given how
collaborative our development on git.git is.

Strangely, I was involved in several GPG-related things. I do not use
signed tags nor signed commits myself, but I care about GPG and about
git making the right calls when it comes to notions like "trust" etc.

* What are you doing on the Git project these days, and why?

What: almost nothing; why: work

During term breaks, I try to follow up an lingering topics and to
participate more actively in the Git mailing list.

* If you could get a team of expert developers to work full time on
something in Git for a full year, what would it be?

  - refs namespace: have tags, notes, replace etc. under
    `remotes/<remote>` with proper merging so that those features learn
    to fly; requires a transition plan

  - rework the UI and make it less surprising: e.g. unify short option
    names, introduce pseudorefs for the index and worktree and the
    like; requires a transition plan

* If you could remove something from Git without worrying about
backwards compatibility, what would it be?

I would do the above without the need for a transition plan :)

* What is your favorite Git-related tool/library, outside of Git itself?

Anything that makes `textconv` fly (`unoconv`, `pdftotext`); `tig` when
`log --graph` is ambiguous; I should use `tig` more ;)

## Releases

* Git [v2.11.1](https://github.com/git/git/blob/v2.11.1/Documentation/RelNotes/2.11.1.txt) was released
* Git [v2.12.0-rc0](https://public-inbox.org/git/xmqqtw8bdjtf.fsf@gitster.mtv.corp.google.com/)
* Git for Windows [v2.11.1](https://github.com/git-for-windows/git/releases/tag/v2.11.1.windows.1) was released
* libgit2/rugged [v0.26.0b2](https://github.com/libgit2/rugged/releases/tag/v0.26.0b2)
* nodegit [v0.17.0](https://github.com/nodegit/nodegit/releases/tag/v0.17.0)
* GitLab [8.15.7 - 8.14.10 ](https://about.gitlab.com/2017/02/16/gitlab-8-dot-15-dot-7-security-release/) and [8.16.6](https://about.gitlab.com/2017/02/17/gitlab-8-dot-16-dot-6-released/)
* GitKraken [v2.0](https://blog.axosoft.com/2017/01/25/gitkraken-v2/)

## Other News

__Events__

___Git Merge 2017___

Apparently, an increasing number of excellent bloggers attend the conference:

* [Git Merge 2017 recap](https://github.com/blog/2317-git-merge-2017-recap) on GitHub Blog
* [We’ll be at Git Merge 2017!](https://blog.bitbucket.org/2017/02/01/well-git-merge-2017/) on Bitbucket Blog
* [Report from Git Merge 2017](http://www.praqma.com/stories/work-on-git-merge-2017/) by Lars Kruse, _Praqma_
* [Git Merge 2017 – what you missed](https://blog.recast.ai/git-merge-2017/) on Recast.AI Blog
* [Review – Git Merge 2017](http://neoshops.de/2017/02/04/review-git-merge-2017/) - short impressions by Carmen Bremen, Magento freelancer
* [Git Merge 2017](http://hryniuk.pl/post/git-merge-2017/), a short read from Łukasz Hryniuk
* [Git Merge and FOSDEM 2017!](https://sfconservancy.org/blog/2017/feb/17/git-merge-fosdem/) by Karen Sandler from the Software Freedom Conservancy
* [The Git Contributor Summit](https://www.edwardthomson.com/blog/git_contributor_summit.html) - Ed Thomson's impressions
* [Git Contributor Summit collaborative report](https://docs.google.com/document/d/1KDoSn4btbK5VJCVld32he29U0pUeFGhpFxyx9ZJDDB0/edit) by Johannes Schindelin and other contributors

__Various__

* Extensive ebook on [Trunk Based Development](https://trunkbaseddevelopment.com/)
* [Scaling Git (and some back story)](https://blogs.msdn.microsoft.com/bharry/2017/02/03/scaling-git-and-some-back-story/) by Brian Harry
* [How to run a Google Summer of Code project on GitHub](https://github.com/blog/2312-how-to-run-a-google-summer-of-code-project-on-github)

__Light reading__

* A bit old, but here is [Linus Torvalds about properly formatted commit messages](https://github.com/torvalds/subsurface-for-dirk/commit/b6590150d68df528efd40c889ba6eea476b39873)
  * Also quite old [How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit/)
* [Mind-blowing git tips for beginners](https://zabanaa.github.io/notes/mind-blowing-git-tips.html)
* [The Biggest and Weirdest Commits in Linux Kernel Git History](https://www.destroyallsoftware.com/blog/2017/the-biggest-and-weirdest-commits-in-linux-kernel-git-history)
* [Top ten pull request review mistakes](https://blog.scottnonnenberg.com/top-ten-pull-request-review-mistakes/) by Scott Nonnenberg
* [The next billion programmers (won’t use Git)](https://medium.com/@gerstenzang/the-next-billion-programmers-wont-use-git-5e8b0ea57886#.qt9zcct4k) by Sam Gerstenzang

__Git tools and sites__

* [Microsoft's GVFS (Git Virtual File System)](https://blogs.msdn.microsoft.com/visualstudioalm/2017/02/03/announcing-gvfs-git-virtual-file-system/)
  * ArsTechnica wrote about it in [Microsoft hosts the Windows source in a monstrous 300GB Git repository](https://arstechnica.com/information-technology/2017/02/microsoft-hosts-the-windows-source-in-a-monstrous-300gb-git-repository/)
  * [Git Virtual File System](https://github.com/Microsoft/GVFS) on GitHub
* [Overview - pagure - Pagure](https://pagure.io/pagure) - a git-centered forge, python based using pygit2
* Not a Git tool, but an interesting challenger on the DVCS arena: [Pijul](https://pijul.org/)
* [**RawGit** serves raw files directly from GitHub with proper **Content-Type** headers](http://rawgit.com/)
* [json-git: A pure JS local Git to versionize any JSON](https://github.com/RobinBressan/json-git)
* [go-git: A highly extensible Git implementation in pure Go](https://github.com/src-d/go-git)
* [git-recall: Handy tool to easily recall what you've done](https://github.com/Fakerr/git-recall)
* [Git List Helper](https://github.com/larsxschneider/git-list-helper) - A few helpers to interact with the Git mailing list
* [The 2git conversion engine](http://www.2git.io/) - The 2git project is an SCM migration engine that enables you to migrate to git using a Groovy DSL
* [reposurgeon](http://www.catb.org/esr/reposurgeon/) is a tool for editing version-control repository history (including [improving DVCS migration](http://www.catb.org/~esr/reposurgeon/dvcs-migration-guide.html), working with any version control system that can export and import git fast-import streams, and with Subversion dump files; source code repository [available on GitLab](https://gitlab.com/esr/reposurgeon))


## Credits

This edition of Git Rev News was curated by
Christian Couder &lt;<christian.couder@gmail.com>&gt;,
Thomas Ferris Nicolaisen &lt;<tfnico@gmail.com>&gt;,
Jakub Narębski &lt;<jnareb@gmail.com>&gt; and
Markus Jansen &lt;<mja@jansen-preisler.de>&gt;
with help from Michael J Gruber.