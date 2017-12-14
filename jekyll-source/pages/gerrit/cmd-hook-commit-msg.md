---
title: " commit-msg Hook"
sidebar: gerritdoc_sidebar
permalink: cmd-hook-commit-msg.html
---
## NAME

commit-msg - Edit commit messages to insert a `Change-Id` tag.

## DESCRIPTION

A Git hook automatically invoked by `git commit`, and most other commit
creation tools such as `git citool` or `git gui`. The Gerrit Code Review
supplied implementation of this hook is a short shell script which
automatically inserts a globally unique `Change-Id` tag in the footer of
a commit message. When present, Gerrit uses this tag to track commits
across cherry-picks and rebases.

After the hook has been installed in the user’s local Git repository for
a project, the hook will modify a commit message such as:

``` 
  Improve foo widget by attaching a bar.

  We want a bar, because it improves the foo by providing more
  wizbangery to the dowhatimeanery.

  Signed-off-by: A. U. Thor <author@example.com>
```

by inserting a new \`Change-Id: \` line in the footer:

``` 
  Improve foo widget by attaching a bar.

  We want a bar, because it improves the foo by providing more
  wizbangery to the dowhatimeanery.

  Change-Id: Ic8aaa0728a43936cd4c6e1ed590e01ba8f0fbf5b
  Signed-off-by: A. U. Thor <author@example.com>
```

The hook implementation is reasonably intelligent at inserting the
`Change-Id` line before any `Signed-off-by` or `Acked-by` lines placed
at the end of the commit message by the author, but if no such lines are
present then it will just insert a blank line, and add the `Change-Id`
at the bottom of the message.

If a `Change-Id` line is already present in the message footer, the
script will do nothing, leaving the existing `Change-Id` unmodified.
This permits amending an existing commit, or allows the user to insert
the Change-Id manually after copying it from an existing change viewed
on the web.

The `Change-Id` will not be added if `gerrit.createChangeId` is set to
`false` in the git config.

## OBTAINING

To obtain the `commit-msg` script use `scp`, `wget` or `curl` to
download it to your local system from your Gerrit server.

You can use either of the below
commands:

``` 
  $ scp -p -P 29418 <your username>@<your Gerrit review server>:hooks/commit-msg <local path to your git>/.git/hooks/

  $ curl -Lo <local path to your git>/.git/hooks/commit-msg <your Gerrit http URL>/tools/hooks/commit-msg
```

A specific example of this might look something like
this:

**Example.**

``` 
  $ scp -p -P 29418 john.doe@review.example.com:hooks/commit-msg ~/duhproject/.git/hooks/

  $ curl -Lo ~/duhproject/.git/hooks/commit-msg http://review.example.com/tools/hooks/commit-msg
```

Make sure the hook file is executable:

``` 
  $ chmod u+x ~/duhproject/.git/hooks/commit-msg
```

## SEE ALSO

  - [Change-Id
    Lines](user-changeid.html)

  - [git-commit(1)](http://www.kernel.org/pub/software/scm/git/docs/git-commit.html)

  - [githooks(5)](http://www.kernel.org/pub/software/scm/git/docs/githooks.html)

## IMPLEMENTATION

The hook generates unique `Change-Id` lines by creating a virtual commit
object within the local Git repository, and obtaining the SHA-1 hash
from it. Like any other Git commit, the following properties are
included in the computation:

  - SHA-1 of the tree being committed

  - SHA-1 of the parent commit

  - Name, email address, timestamp of the author

  - Name, email address, timestamp of the committer

  - Proposed commit message (before `Change-Id` was inserted)

Because the names of the tree and parent commit, as well as the
committer timestamp are included in the hash computation, the output
`Change-Id` is sufficiently unique.

## GERRIT

Part of [Gerrit Code Review](index.html)

## SEARCHBOX
