---
title: " The refs/for namespace"
sidebar: gerritdoc_sidebar
permalink: concept-refs-for-namespace.html
---
When pushing a new or updated commit to Gerrit, you push that commit
using a
[reference](https://www.kernel.org/pub/software/scm/git/docs/gitglossary.html#def_ref),
in the `refs/for` namespace. This reference must also define the target
branch, such as `refs/for/[BRANCH_NAME]`.

For example, to create a new change on the master branch, you would use
the following command:

    git push origin HEAD:refs/for/master

The `refs/for/[BRANCH_NAME]` syntax allows Gerrit to differentiate
between commits that are pushed for review and commits that are pushed
directly into the repository.

Gerrit supports using either the full name or the short name for a
branch. For instance, this command:

    git commit
    git push origin HEAD:refs/for/master

is the same as:

    git commit
    git push origin HEAD:refs/for/refs/heads/master

Gerrit uses the `refs/for/` prefix to map the concept of "Pushing for
Review" to the git protocol. For the git client, it looks like every
push goes to the same branch, such as `refs/for/master`. In fact, for
each commit pushed to this ref, Gerrit creates a new ref under a
`refs/changes/` namespace, which Gerrit uses to track these commits.
These references use the following format:

    refs/changes/[CD]/[ABCD]/[EF]

Where:

  - \[CD\] is the last two digits of the change number

  - \[ABCD\] is the change number

  - \[EF\] is the patch set number

For example:

    refs/changes/20/884120/1

You can use the change reference to fetch its corresponding
    commit:

    git fetch https://[GERRIT_SERVER_URL]/[PROJECT] refs/changes/[XX]/[YYYY]/[ZZ] \
    && git checkout FETCH_HEAD

> **Note**
> 
> The fetch command can be copied from the [download
> command](user-review-ui.html#download) in the Change screen.

## GERRIT

Part of [Gerrit Code Review](index.html)

## SEARCHBOX

