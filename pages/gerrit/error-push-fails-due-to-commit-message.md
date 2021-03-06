---
title: " Push fails due to commit message"
sidebar: errors_sidebar
permalink: error-push-fails-due-to-commit-message.html
---
If Gerrit rejects pushing a commit it is often the case that there is an
issue with the commit message of the pushed commit. In this case the
problem can often be resolved by fixing the commit message.

If the commit message of the last commit needs to be fixed you can
simply amend the last commit (please find a detailed description in the
[Git
documentation](http://www.kernel.org/pub/software/scm/git/docs/git-commit.html)):

``` 
  $ git commit --amend
```

If you need to fix the commit messages of several commits or of any
commit other than the last one you have to do an interactive git rebase
for the affected commits. While doing the interactive rebase you can
e.g. choose *reword* for those commits for which you want to fix the
commit messages. For a detailed description of git rebase please check
the [Git
documentation](http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html).

Please use interactive git rebase with care as it rewrites existing
commits. Generally you should never rewrite commits that have already
been submitted in Gerrit.

Sometimes commit hooks are used to automatically insert/update
information in the commit message. If such information is missing in the
commit message of existing commits (e.g. because the commit hook was
only configured later) rewriting the commits will (re)execute the commit
hook and so update the commit messages. If you do an interactive rebase
to achieve this make sure that the affected commits are really
rewritten, e.g. by choosing *reword* for all these commits and then
confirming all the commit messages. Just picking a commit may not
rewrite it.

## GERRIT

Part of [Gerrit Error Messages](error-messages.html)

## SEARCHBOX

