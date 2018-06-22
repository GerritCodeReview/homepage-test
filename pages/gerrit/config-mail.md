---
title: " Gerrit Code Review - Mail Templates"
sidebar: gerritdoc_sidebar
permalink: config-mail.html
---
Gerrit uses [Closure
Templates](https://developers.google.com/closure/templates/) (Soy) for
the bulk of the standard mails it sends out. There are builtin default
templates which are used if they are not overridden. These defaults are
also provided as examples so that administrators may copy them and
easily modify them to tweak their contents.

**Compatibility Note:** previously, Velocity Template Language (VTL) was
used as the template language for Gerrit emails. VTL has now been
deprecated in favor of Soy, but Velocity templates that modify text
emails remain supported for now.

## Template Locations and Extensions:

The default example templates reside under: `'$site_path'/etc/mail` and
are terminated with the double extension `.soy.example`. Modifying these
example files will have no effect on the behavior of Gerrit. However,
copying an example template to an equivalently named file without the
`.example` extension and modifying it will allow an administrator to
customize the template.

## Supported Mail Templates:

Each mail that Gerrit sends out is controlled by at least one template.
These are listed below. Change emails are influenced by two additional
templates, one to set the subject line, and one to set the footer which
gets appended to all the change emails (see `ChangeSubject.soy` and
`ChangeFooter.soy` below.)

Many types of Gerrit email message support HTML in addition to
plain-text. Where both are supported, templates to control the HTML part
have `...Html` appended in their file names. For example, for
"Abandoned" emails, the `Abandoned.soy` template determines the text
part of the message, whereas `AbandonedHtml.soy` determines the HTML
part.

### Abandoned.soy and AbandonedHtml.soy

The "Abandoned" templates will determine the contents of the email
related to a change being abandoned. It is a `ChangeEmail`: see
`ChangeSubject.soy` and ChangeFooter.

### AddKey.soy and AddKeyHtml.soy

AddKey templates will determine the contents of the email related to SSH
and GPG keys being added to a user account. This notification is not
sent when the key is administratively added to another user account.

### ChangeFooter.soy and ChangeFooterHtml.soy

The ChangeFooter templates will determine the contents of the footer
that will be appended to emails related to changes (all
\`ChangeEmail\`s).

### ChangeSubject.soy

The `ChangeSubject.soy` template will determine the contents of the
email subject line for ALL emails related to changes.

### Comment.soy

The `Comment.soy` template will determine the contents of the email
related to a user submitting comments on changes. It is a `ChangeEmail`:
see `ChangeSubject.soy`, ChangeFooter and CommentFooter.

### CommentFooter.soy and CommentFooterHtml.soy

The CommentFooter templates will determine the contents of the footer
text that will be appended to emails related to a user submitting
comments on changes. See `ChangeSubject.soy`, Comment and ChangeFooter.

### DeleteVote.soy and DeleteVoteHtml.soy

The DeleteVote templates will determine the contents of the email
related to removing votes on changes. It is a `ChangeEmail`: see
`ChangeSubject.soy` and ChangeFooter.

### DeleteReviewer.soy and DeleteReviewerHtml.soy

The DeleteReviewer templates will determine the contents of the email
related to a user removing a reviewer (with a vote) from a change. It is
a `ChangeEmail`: see `ChangeSubject.soy` and ChangeFooter.

### Footer.soy and FooterHtml.soy

The Footer templates will determine the contents of the footer text
appended to the end of all outgoing emails after the ChangeFooter and
CommentFooter.

### Merged.soy and MergedHtml.soy

The Merged templates will determine the contents of the email related to
a change successfully merged to the head. It is a `ChangeEmail`: see
`ChangeSubject.soy` and ChangeFooter.

### NewChange.soy and NewChangeHtml.soy

The NewChange templates will determine the contents of the email related
to a user submitting a new change for review. This includes changes
created by actions made by the user in the Web UI such as cherry picking
a commit or reverting a change. It is a `ChangeEmail`: see
`ChangeSubject.soy` and ChangeFooter.

### RegisterNewEmail.soy

The `RegisterNewEmail.soy` template will determine the contents of the
email related to registering new email accounts.

### ReplacePatchSet.soy and ReplacePatchSetHtml.soy

The ReplacePatchSet templates will determine the contents of the email
related to a user submitting a new patchset for a change. This includes
patchsets created by actions made by the user in the Web UI such as
editing the commit message, cherry picking a commit, or rebasing a
change. It is a `ChangeEmail`: see `ChangeSubject.soy` and ChangeFooter.

### Restored.soy and RestoredHtml.soy

The Restored templates will determine the contents of the email related
to a change being restored. It is a `ChangeEmail`: see
`ChangeSubject.soy` and ChangeFooter.

### Reverted.soy and RevertedHtml.soy

The Reverted templates will determine the contents of the email related
to a change being reverted. It is a `ChangeEmail`: see
`ChangeSubject.soy` and ChangeFooter.

### SetAssignee.soy and SetAssigneeHtml.soy

The SetAssignee templates will determine the contents of the email
related to a user being assigned to a change. It is a `ChangeEmail`: see
`ChangeSubject.soy` and ChangeFooter.

## Mail Variables and Methods

Mail templates can access and display objects currently made available
to them via the Soy context.

### Warning

Be aware that modifying templates can cause them to fail to parse and
therefore not send out the actual email.

### All OutgoingEmails

All outgoing emails have the following variables available to them:

  - $email.settingsUrl  
    The URL to view the user’s settings in the Gerrit web UI.

  - $email.gerritHost  
    The name of the Gerrit instance.

  - $email.gerritUrl  
    The URL to the Gerrit web UI.

  - $messageClass  
    A String containing the messageClass.

### Change Emails

Change related emails have the following template data available to
them, in addition to what’s available to all outgoing emails.

  - $changeId  
    Id of the current change (a `Change.Key`).

  - $coverLetter  
    The text of the `ChangeMessage`.

  - $fromName  
    The name of the from user.

  - $email.unifiedDiff  
    The diff of the change.

  - $email.changeDetail  
    The details of the change, including the commit message.

  - $email.changeUrl  
    The URL to the change in the web UI.

  - $email.includeDiff  
    Whether the Gerrit instance is configured to include diffs in
    emails.

  - $change.subject  
    The subject of the current change.

  - $change.originalSubject  
    The subject corresponding to the first patch set of the current
    change.

  - $change.shortSubject  
    The subject limited to 72 characters, with an ellipsis if it exceeds
    that.

  - $change.ownerEmail  
    The email address of the owner of the change.

  - $branch.shortName  
    The name of the branch targeted by the current change.

  - $projectName  
    The name of this change’s project.

  - $shortProjectName  
    The project name with the path abbreviated.

  - $sshHost  
    SSH hostname for the Gerrit instance.

  - $patchSet.patchSetId  
    The current patch set number.

  - $patchSet.refname  
    The refname of the patch set.

## GERRIT

Part of [Gerrit Code Review](index.html)

## SEARCHBOX
