---
title: " Gerrit Code Review - Contributing"
sidebar: gerritdoc_sidebar
permalink: dev-contributing.html
---
## Introduction

Gerrit is developed as a [self-hosting open source
project](https://gerrit-review.googlesource.com/) and very much welcomes
contributions from anyone with a contributor’s agreement on file with
the project.

## Contributor License Agreement

A Contributor License Agreement must be completed before contributions
are accepted. To view and accept the agreements do the following:

  - Click *Sign In* at the top right corner of
    <https://gerrit-review.googlesource.com/>

  - Sign In with your Google account

  - After signing in, go to the
    [Agreements](https://gerrit-review.googlesource.com/#/settings/agreements)
    tab on the settings page

  - Click *New Contributor Agreement* and follow the instructions

For reference, the actual agreements are linked below

  - [Individual
    Agreement](https://cla.developers.google.com/about/android-individual)

  - [Corporate
    Agreement](https://source.android.com/source/cla-corporate.pdf)

## Code Review

As Gerrit is a code review tool, naturally contributions will be
reviewed before they will get submitted to the code base. To start your
contribution, please make a git commit and upload it for review to the
main Gerrit review server. To help speed up the review of your change,
review these guidelines before submitting your change. You can view the
pending Gerrit contributions and their statuses
[here](https://gerrit-review.googlesource.com/#/q/status:open+project:gerrit).

Depending on the size of that list it might take a while for your change
to get reviewed. Naturally there are fewer approvers than contributors;
so anything that you can do to ensure that your contribution will
undergo fewer revisions will speed up the contribution process. This
includes helping out reviewing other people’s changes to relieve the
load from the approvers. Even if you are not familiar with Gerrit’s
internals, it would be of great help if you can download, try out, and
comment on new features. If it works as advertised, say so, and if you
have the privileges to do so, go ahead and give it a +1 Verified. If you
would find the feature useful, say so and give it a +1 code review.

And finally, the quicker you respond to the comments of your reviewers,
the quicker your change might get merged\! Try to reply to every comment
after submitting your new patch, particularly if you decided against
making the suggested change. Reviewers don’t want to seem like nags and
pester you if you haven’t replied or made a fix, so it helps them know
if you missed it or decided against it.

## Review Criteria

Here are some hints as to what approvers may be looking for before
approving or submitting changes to the Gerrit project. Let’s start with
the simple nit picky stuff. You are likely excited that your code works;
help us share your excitement by not distracting us with the simple
stuff. Thanks to Gerrit, problems are often highlighted and we find it
hard to look beyond simple spacing issues. Blame it on our short
attention spans, we really do want your code.

### Commit Message

It is essential to have a good commit message if you want your change to
be reviewed.

  - Keep lines no longer than 72 chars

  - Start with a short one line summary

  - Followed by a blank line

  - Followed by one or more explanatory paragraphs

  - Use the present tense (fix instead of fixed)

  - Use the past tense when describing the status before this commit

  - Include a `Bug: Issue <#>` line if fixing a Gerrit issue, or a
    `Feature: Issue <#>` line if implementing a feature request.

  - Include a `Change-Id` line

### Setting up Vim for Git commit message

Git uses Vim as the default commit message editor. Put this into your
`$HOME/.vimrc` file to configure Vim for Git commit message formatting
and
    writing:

    " Enable spell checking, which is not on by default for commit messages.
    au FileType gitcommit setlocal spell

    " Reset textwidth if you've previously overridden it.
    au FileType gitcommit setlocal textwidth=72

### A sample good Gerrit commit message:

    Add sample commit message to guidelines doc

    The original patch set for the contributing guidelines doc did not
    include a sample commit message, this new patchset does.  Hopefully this
    makes things a bit clearer since examples can sometimes help when
    explanations don't.

    Note that the body of this commit message can be several paragraphs, and
    that I word wrap it at 72 characters.  Also note that I keep the summary
    line under 50 characters since it is often truncated by tools which
    display just the git summary.

    Bug: Issue 98765605
    Change-Id: Ic4a7c07eeb98cdeaf44e9d231a65a51f3fceae52

The `Change-Id` line is, as usual, created by a local git hook. To
install it, simply copy it from the checkout and make it
    executable:

    cp ./gerrit-server/src/main/resources/com/google/gerrit/server/tools/root/hooks/commit-msg .git/hooks/
    chmod +x .git/hooks/commit-msg

If you are working on core plugins, you will also need to install the
same hook in the submodules:

    export hook=$(pwd)/.git/hooks/commit-msg
    git submodule foreach 'cp -p "$hook" "$(git rev-parse --git-dir)/hooks/"'

To set up git’s remote for easy pushing, run the following:

    git remote add gerrit https://gerrit.googlesource.com/gerrit

The HTTPS access requires proper username and password; this can be
obtained by clicking the *Obtain Password* link on the [HTTP Password
tab of the user settings
page](https://gerrit-review.googlesource.com/#/settings/http-password).

### Style

This project has a policy of Eclipse’s warning free code. Eclipse
configuration is added to git and we expect the changes to be warnings
free.

We do not ask you to use Eclipse for editing, obviously. We do ask you
to provide Eclipse’s warning free patches only. If for some reasons, you
are not able to set up Eclipse and verify, that your patch hasn’t
introduced any new Eclipse warnings, mention this in a comment to your
change, so that reviewers will do it for you. Yes, the way to go is to
extend gerrit CI to take care of this, but it’s not yet implemented.

Gerrit generally follows the [Google Java Style
Guide](https://google.github.io/styleguide/javaguide.html).

To format Java source code, Gerrit uses the
[`google-java-format`](https://github.com/google/google-java-format)
tool (version 1.3), and to format Bazel BUILD and WORKSPACE files the
[`buildifier`](https://github.com/bazelbuild/buildifier) tool (version
0.4.5). These tools automatically apply format according to the style
guides; this streamlines code review by reducing the need for
time-consuming, tedious, and contentious discussions about trivial
issues like whitespace.

You may download and run `google-java-format` on your own, or you may
run `./tools/setup_gjf.sh` to download a local copy and set up a wrapper
script. If you run your own copy, please use the same version, as there
may be slight differences between versions.

When considering the style beyond just formatting rules, it is often
more important to match the style of the nearby code which you are
modifying than it is to match the style guide exactly. This is
especially true within the same file.

Additionally, you will notice that most of the newline spacing is fairly
consistent throughout the code in Gerrit, it helps to stick to the blank
line conventions. Here are some specific examples:

  - Keep a blank line between all class and method declarations.

  - Do not add blank lines at the beginning or end of class/methods.

When to use `final` modifier and when not (in new code):

Always:

  - final fields: marking fields as final forces them to be initialized
    in the constructor or at declaration

  - final static fields: clearly communicates the intent

  - to use final variables in inner anonymous classes

Optional:

  - final classes: use when appropriate, e.g. API restriction

  - final methods: similar to final classes

Never:

  - local variables: it clutters the code, and makes the code less
    readable. When copying old code to new location, finals should be
    removed

  - method parameters: similar to local variables

### Code Organization

Do your best to organize classes and methods in a logical way. Here are
some guidelines that Gerrit uses:

  - Ensure a standard copyright header is included at the top of any new
    files (copy it from another file, update the year).

  - Always place loggers first in your class\!

  - Define any static interfaces next in your class.

  - Define non static interfaces after static interfaces in your class.

  - Next you should define static types, static members, and static
    methods, in decreasing order of visibility (public to private).

  - Finally instance types, instance members, then constructors, and
    then instance methods.

  - Some common exceptions are private helper static methods, which
    might appear near the instance methods which they help (but may also
    appear at the top).

  - Getters and setters for the same instance field should usually be
    near each other barring a good reason not to.

  - If you are using assisted injection, the factory for your class
    should be before the instance members.

  - Annotations should go before language keywords (`final`, `private`,
    etc)  
    Example: `@Assisted @Nullable final type varName`

  - Prefer to open multiple AutoCloseable resources in the same
    try-with-resources block instead of nesting the try-with-resources
    blocks and increasing the indentation level more than necessary.

Wow that’s a lot\! But don’t worry, you’ll get the habit and most of the
code is organized this way already; so if you pay attention to the class
you are editing you will likely pick up on it. Naturally new classes are
a little harder; you may want to come back and consult this section when
creating them.

### Design

Here are some design level objectives that you should keep in mind when
coding:

  - ORM entity objects should match exactly one row in the database.

  - Most client pages should perform only one RPC to load so as to keep
    latencies down. Exceptions would apply to RPCs which need to load
    large data sets if splitting them out will help the page load
    faster. Generally page loads are expected to complete in under
    100ms. This will be the case for most operations, unless the data
    being fetched is not using Gerrit’s caching infrastructure. In these
    slower cases, it is worth considering mitigating this longer load by
    using a second RPC to fill in this data after the page is displayed
    (or alternatively it might be worth proposing caching this data).

  - `@Inject` should be used on constructors, not on fields. The current
    exceptions are the ssh commands, these were implemented earlier in
    Gerrit’s development. To stay consistent, new ssh commands should
    follow this older pattern; but eventually these should get converted
    to eliminate this exception.

  - Don’t leave repository objects (git or schema) open. A .close()
    after every open should be placed in a finally{} block.

  - Don’t leave UI components, which can cause new actions to occur,
    enabled during RPCs which update the DB. This is to prevent people
    from submitting actions more than once when operating on slow links.
    If the action buttons are disabled, they cannot be resubmitted and
    the user can see that Gerrit is still busy.

  - GWT EventBus is the new way forward.

  - …and so is Guava (previously known as Google Collections).

### Tests

  - Tests for new code will greatly help your change get approved.

### Change Size/Number of Files Touched

And finally, I probably cannot say enough about change sizes. Generally,
smaller is better, hopefully within reason. Do try to keep things which
will be confusing on their own together, especially if changing one
without the other will break something\!

  - If a new feature is implemented and it is a larger one, try to
    identify if it can be split into smaller logical features; when in
    doubt, err on the smaller side.

  - Separate bug fixes from feature improvements. The bug fix may be an
    easy candidate for approval and should not need to wait for new
    features to be approved. Also, combining the two makes reviewing
    harder since then there is no clear line between the fix and the
    feature.

  - Separate supporting refactoring from feature changes. If your new
    feature requires some refactoring, it helps to make the refactoring
    a separate change which your feature change depends on. This way,
    reviewers can easily review the refactor change as a something that
    should not alter the current functionality, and feel more confident
    they can more easily spot errors this way. Of course, it also makes
    it easier to test and locate later on if an unfortunate error does
    slip in. Lastly, by not having to see refactoring changes at the
    same time, it helps reviewers understand how your feature changes
    the current functionality.

  - Separate logical features into separate changes. This is often the
    hardest part. Here is an example: when adding a new ability, make
    separate changes for the UI and the ssh commands if possible.

  - Do only what the commit message describes. In other words, things
    which are not strictly related to the commit message shouldn’t be
    part of a change, even trivial things like externalizing a string
    somewhere or fixing a typo. This helps keep `git blame` more useful
    in the future and it also makes `git revert` more useful.

  - Use topics to link your separate changes together.

## Process

### Backporting to stable branches

From time to time bug fix releases are made for existing stable
branches.

Developers concerned with stable branches are encouraged to backport or
push patchsets to these branches, even if no new release is planned.

Fixes that are known to be needed for a particular release should be
pushed for review on that release’s stable branch. It will then be
included in the master branch when the stable branch is merged back.

### Updating to new version of GWT

When updating to a new version of GWT, there are several things that
also need to be updated or at least checked.

  - Update common and plugin dependencies in `tools/gwt-constants.defs`.

  - Update to the same GWT version in the cookbook plugin and optionally
    in other plugins that have a dependency on GWT.

  - Update the GWT version in the archetype metadata in the
    `gerrit-plugin-gwt-archetype`.

  - Update the version of `gwt-maven-plugin` in the example pom.xml file
    in [dev-plugins](dev-plugins.html).

  - Update to the same GWT version in the `gwtjsonrpc` project, and
    release a new version.

### Finding starter projects to work on

We have created a
[StarterProject](https://bugs.chromium.org/p/gerrit/issues/list?can=2&q=label%3AStarterProject)
category in the issue tracker and try to assign easy hack projects to
it. If in doubt, do not hesitate to ask on the developer [mailing
list](https://groups.google.com/forum/#!forum/repo-discuss).

### Upgrading Libraries

Gerrit’s library dependencies should only be upgraded if the new version
contains something we need in Gerrit. This includes new features, API
changes as well as bug or security fixes. An exception to this rule is
that right after a new Gerrit release was branched off, all libraries
should be upgraded to the latest version to prevent Gerrit from falling
behind. Doing those upgrades should conclude at the latest two months
after the branch was cut. This should happen on the master branch to
ensure that they are vetted long enough before they go into a release
and we can be sure that the update doesn’t introduce a regression.

## GERRIT

Part of [Gerrit Code Review](index.html)

## SEARCHBOX
