---
title: " Gerrit Code Review - Searching Changes"
sidebar: gerritdoc_sidebar
permalink: user-search.html
---
## Default Searches

Most basic searches can be viewed by clicking on a link along the top
menu bar. The link will prefill the search box with a common search
query, execute it, and present the results. If exactly one change
matches the search, the change will be presented instead of a list.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Description</th>
<th>Default Query</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>All &gt; Open</p></td>
<td><p>status:open <em>(or is:open)</em></p></td>
</tr>
<tr class="even">
<td><p>All &gt; Merged</p></td>
<td><p>status:merged</p></td>
</tr>
<tr class="odd">
<td><p>All &gt; Abandoned</p></td>
<td><p>status:abandoned</p></td>
</tr>
<tr class="even">
<td><p>My &gt; Watched Changes</p></td>
<td><p>status:open is:watched</p></td>
</tr>
<tr class="odd">
<td><p>My &gt; Starred Changes</p></td>
<td><p>is:starred</p></td>
</tr>
<tr class="even">
<td><p>My &gt; Draft Comments</p></td>
<td><p>has:draft</p></td>
</tr>
<tr class="odd">
<td><p>Open changes in Foo</p></td>
<td><p>status:open project:Foo</p></td>
</tr>
</tbody>
</table>

## Basic Change Search

Similar to many popular search engines on the web, just enter some text
and let Gerrit figure out the meaning:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th>Description</th>
<th>Examples</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Legacy numerical id</p></td>
<td><p>15183</p></td>
</tr>
<tr class="even">
<td><p>Full or abbreviated Change-Id</p></td>
<td><p>Ic0ff33</p></td>
</tr>
<tr class="odd">
<td><p>Full or abbreviated commit SHA-1</p></td>
<td><p>d81b32ef</p></td>
</tr>
<tr class="even">
<td><p>Email address</p></td>
<td><p><a href="mailto:user@example.com">user@example.com</a></p></td>
</tr>
<tr class="odd">
<td><p>Approval requirement</p></td>
<td><p>Code-Review&gt;=+2, Verified=1</p></td>
</tr>
</tbody>
</table>

## Search Operators

Operators act as restrictions on the search. As more operators are added
to the same query string, they further restrict the returned results.
Search can also be performed by typing only a text with no operator,
which will match against a variety of fields.

  - age:'AGE'  
    Amount of time that has expired since the change was last updated
    with a review comment or new patch set. The age must be specified to
    include a unit suffix, for example `age:2d`:
    
      - s, sec, second, seconds
    
      - m, min, minute, minutes
    
      - h, hr, hour, hours
    
      - d, day, days
    
      - w, week, weeks (`1 week` is treated as `7 days`)
    
      - mon, month, months (`1 month` is treated as `30 days`)
    
      - y, year, years (`1 year` is treated as `365 days`)

  - assignee:'USER'  
    Changes assigned to the given user.

  - before:'TIME'/until:'TIME'  
    Changes modified before the given *TIME*, inclusive. Must be in the
    format `2006-01-02[ 15:04:05[.890][ -0700]]`; omitting the time
    defaults to 00:00:00 and omitting the timezone defaults to UTC.

  - after:'TIME'/since:'TIME'  
    Changes modified after the given *TIME*, inclusive. Must be in the
    format `2006-01-02[ 15:04:05[.890][ -0700]]`; omitting the time
    defaults to 00:00:00 and omitting the timezone defaults to UTC.

  - change:'ID'  
    Either a legacy numerical *ID* such as 15183, or a newer style
    Change-Id that was scraped out of the commit message.

  - conflicts:'ID'  
    Changes that conflict with change *ID*. Change *ID* can be specified
    as a legacy numerical *ID* such as 15183, or a newer style Change-Id
    that was scraped out of the commit message.

  - destination:'NAME'  
    Changes which match the current user’s destination named *NAME*.
    (see [Named Destinations](user-named-destinations.html)).

  - owner:'USER', o:'USER'  
    Changes originally submitted by *USER*. The special case of
    `owner:self` will find changes owned by the caller.

  - ownerin:'GROUP'  
    Changes originally submitted by a user in *GROUP*.

  - query:'NAME'  
    Changes which match the current user’s query named *NAME* (see
    [Named Queries](user-named-queries.html)).

  - reviewer:'USER', r:'USER'  
    Changes that have been, or need to be, reviewed by *USER*. The
    special case of `reviewer:self` will find changes where the caller
    has been added as a reviewer.

  - cc:'USER'  
    Changes that have the given user CC’ed on them. The special case of
    `cc:self` will find changes where the caller has been CC’ed.

  - revertof:'ID'  
    Changes that revert the change specified by the numeric *ID*.

  - reviewerin:'GROUP'  
    Changes that have been, or need to be, reviewed by a user in
    *GROUP*.

  - commit:'SHA1'  
    Changes where *SHA1* is one of the patch sets of the change.

  - project:'PROJECT', p:'PROJECT'  
    Changes occurring in *PROJECT*. If *PROJECT* starts with `^` it
    matches project names by regular expression. The [dk.brics.automaton
    library](http://www.brics.dk/automaton/) is used for evaluation of
    such patterns.

  - projects:'PREFIX'  
    Changes occurring in projects starting with *PREFIX*.

  - parentproject:'PROJECT'  
    Changes occurring in *PROJECT* or in one of the child projects of
    *PROJECT*.

  - branch:'BRANCH'  
    Changes for *BRANCH*. The branch name is either the short name shown
    in the web interface or the full name of the destination branch with
    the traditional *refs/heads/* prefix.
    
    If *BRANCH* starts with `^` it matches branch names by regular
    expression patterns. The [dk.brics.automaton
    library](http://www.brics.dk/automaton/) is used for evaluation of
    such patterns.

  - intopic:'TOPIC'  
    Changes whose designated topic contains *TOPIC*, using a full-text
    search.
    
    If *TOPIC* starts with `^` it matches topic names by regular
    expression patterns. The [dk.brics.automaton
    library](http://www.brics.dk/automaton/) is used for evaluation of
    such patterns.

  - topic:'TOPIC'  
    Changes whose designated topic matches *TOPIC* exactly. This is
    often combined with *branch:* and *project:* operators to select all
    related changes in a series.

  - ref:'REF'  
    Changes where the destination branch is exactly the given *REF*
    name. Since *REF* is absolute from the top of the repository it must
    start with *refs/*.
    
    If *REF* starts with `^` it matches reference names by regular
    expression patterns. The [dk.brics.automaton
    library](http://www.brics.dk/automaton/) is used for evaluation of
    such patterns.

  - tr:'ID', bug:'ID'  
    Search for changes whose commit message contains *ID* and matches
    one or more of the [trackingid
    sections](config-gerrit.html#trackingid) in the server’s
    configuration file. This is typically used to search for changes
    that fix a bug or defect by the issue tracking system’s issue
    identifier.

  - label:'VALUE'  
    Matches changes where the approval score *VALUE* has been set during
    a review. See [labels](#labels) below for more detail on the format
    of the argument.

  - message:'MESSAGE'  
    Changes that match *MESSAGE* arbitrary string in the commit message
    body.

  - comment:'TEXT'  
    Changes that match *TEXT* string in any comment left by a reviewer.

  - path:'PATH'  
    Matches any change touching file at *PATH*. By default exact path
    matching is used, but regular expressions can be enabled by starting
    with `^`. For example, to match all XML files use `file:^.*\.xml$`.
    The [dk.brics.automaton library](http://www.brics.dk/automaton/) is
    used for the evaluation of such patterns.
    
    The `^` required at the beginning of the regular expression not only
    denotes a regular expression, but it also has the usual meaning of
    anchoring the match to the start of the string. To match all Java
    files, use `file:^.*\.java`.
    
    The entire regular expression pattern, including the `^` character,
    should be double quoted when using more complex construction (like
    ones using a bracket expression). For example, to match all XML
    files named like *name1.xml*, *name2.xml*, and *name3.xml* use
    `file:"^name[1-3].xml"`.

  - file:'NAME', f:'NAME'  
    Matches any change touching a file containing the path component
    *NAME*. For example a `file:src` will match changes that modify
    files named `gerrit-server/src/main/java/Foo.java`. Name matching is
    exact match, `file:Foo.java` finds any change touching a file named
    exactly `Foo.java` and does not match `AbstractFoo.java`.
    
    Regular expression matching can be enabled by starting the string
    with `^`. In this mode `file:` is an alias of `path:` (see above).

  - star:'LABEL'  
    Matches any change that was starred by the current user with the
    label *LABEL*.
    
    E.g. if changes that are not interesting are marked with an `ignore`
    star, they could be filtered out by *-star:ignore*.
    
    *star:star* is the same as *has:star* and *is:starred*.

  - has:draft  
    True if there is a draft comment saved by the current user.

  - has:star  
    Same as *is:starred* and *star:star*, true if the change has been
    starred by the current user with the default label.

  - has:stars  
    True if the change has been starred by the current user with any
    label.

  - has:edit  
    True if the change has inline edit created by the current user.

  - has:unresolved  
    True if the change has unresolved comments.

  - is:assigned  
    True if the change has an assignee.

  - is:starred  
    Same as *has:star*, true if the change has been starred by the
    current user with the default label.

  - is:unassigned  
    True if the change does not have an assignee.

  - is:watched  
    True if this change matches one of the current user’s watch filters,
    and thus is likely to notify the user when it updates.

  - is:reviewed  
    True if any user has commented on the change more recently than the
    last update (comment or patch set) from the change owner.

  - is:owner  
    True on any change where the current user is the change owner. Same
    as `owner:self`.

  - is:reviewer  
    True on any change where the current user is a reviewer. Same as
    `reviewer:self`.

  - is:open, is:pending  
    True if the change is open.

  - is:closed  
    True if the change is either merged or abandoned.

  - is:merged, is:abandoned  
    Same as [status:'STATE'](#status).

  - is:submittable  
    True if the change is submittable according to the submit rules for
    the project, for example if all necessary labels have been voted on.
    
    This operator only takes into account one change at a time, not any
    related changes, and does not guarantee that the submit button will
    appear for matching changes. To check whether a submit button
    appears, use the [Get Revision
    Actions](rest-api-changes.html#get-revision-actions) API.
    
    Equivalent to [submittable:ok](#submittable).

  - is:mergeable  
    True if the change has no merge conflicts and could be merged into
    its destination branch.
    
    Mergeability of abandoned changes is not computed. This operator
    will not find any abandoned but mergeable changes.

  - is:ignored  
    True if the change is ignored. Same as `star:ignore`.

  - is:private  
    True if the change is private, ie. only visible to owner and its
    reviewers.

  - is:wip  
    True if the change is Work In Progress.

  - status:open, status:pending  
    True if the change state is *review in progress*.

  - status:reviewed  
    Same as *is:reviewed*, matches if any user has commented on the
    change more recently than the last update (comment or patch set)
    from the change owner.

  - status:closed  
    True if the change is either *merged* or *abandoned*.

  - status:merged  
    Change has been merged into the branch.

  - status:abandoned  
    Change has been abandoned.

  - added:'RELATION'*LINES*, deleted:'RELATION'*LINES*,
    delta/size:'RELATION'*LINES*  
    True if the number of lines added/deleted/changed satisfies the
    given relation for the given number of lines.
    
    For example, added:\>50 will be true for any change which adds at
    least 50 lines.
    
    Valid relations are \>=, \>, ⇐, \<, or no relation, which will match
    if the number of lines is exactly equal.

  - commentby:'USER'  
    Changes containing a top-level or inline comment by *USER*. The
    special case of `commentby:self` will find changes where the caller
    has commented.

  - from:'USER'  
    Changes containing a top-level or inline comment by *USER*, or owned
    by *USER*. Equivalent to `(owner:USER OR commentby:USER)`.

  - reviewedby:'USER'  
    Changes where *USER* has commented on the change more recently than
    the last update (comment or patch set) from the change owner.

  - author:'AUTHOR'  
    Changes where *AUTHOR* is the author of the current patch set.
    *AUTHOR* may be the author’s exact email address, or part of the
    name or email address.

  - committer:'COMMITTER'  
    Changes where *COMMITTER* is the committer of the current patch set.
    *COMMITTER* may be the committer’s exact email address, or part of
    the name or email address.

  - submittable:'SUBMIT\_STATUS'  
    Changes having the given submit record status after applying submit
    rules. Valid statuses are in the `status` field of
    [SubmitRecord](rest-api-changes.html#submit-record). This operator
    only applies to the top-level status; individual label statuses can
    be searched [by label](#labels).

  - unresolved:'RELATION'*NUMBER*  
    True if the number of unresolved comments satisfies the given
    relation for the given number.
    
    For example, unresolved:\>0 will be true for any change which has at
    least one unresolved comment while unresolved:0 will be true for any
    change which has all comments resolved.
    
    Valid relations are \>=, \>, ⇐, \<, or no relation, which will match
    if the number of unresolved comments is exactly equal.

## Argument Quoting

Operator values that are not bare words (roughly A-Z, a-z, 0-9, @,
hyphen, dot and underscore) must be quoted for the query parser.

Quoting is accepted as either double quotes (e.g. `message:"the value"`)
or as matched curly braces (e.g. `message:{the value}`).

## Boolean Operators

Unless otherwise specified, operators are joined using the `AND` boolean
operator, thereby restricting the search results.

Parentheses can be used to force a particular precedence on complex
operator expressions, otherwise OR has higher precedence than AND.

### Negation

Any operator can be negated by prefixing it with `-`, for example
`-is:starred` is the exact opposite of `is:starred` and will therefore
return changes that are **not** starred by the current user.

The operator `NOT` (in all caps) is a synonym.

### AND

The boolean operator `AND` (in all caps) can be used to join two other
operators together. This results in a restriction of the results,
returning only changes that match both operators.

### OR

The boolean operator `OR` (in all caps) can be used to find changes that
match either operator. This increases the number of results that are
returned, as more changes are considered.

## Labels

Label operators can be used to match approval scores given during a code
review. The specific set of supported labels depends on the server
configuration, however the `Code-Review` label is provided out of the
box.

A label name is any of the following:

  - The label name. Example: `label:Code-Review`.

  - The label name followed by a *,* followed by a reviewer id or a
    group id. To make it clear whether a user or group is being looked
    for, precede the value by a user or group argument identifier
    (*user=* or *group=*). If an LDAP group is being referenced make
    sure to use *ldap/\<groupname\>*.

A label name must be followed by either a score with optional operator,
or a label status. The easiest way to explain this is by example.

First, some examples of scores with operators:

  - `label:Code-Review=2`; `label:Code-Review=+2`;
    `label:Code-Review+2`  
    Matches changes where there is at least one +2 score for
    Code-Review. The + prefix is optional for positive score values. If
    the + is used, the = operator is optional.

  - `label:Code-Review=-2`; `label:Code-Review-2`  
    Matches changes where there is at least one -2 score for
    Code-Review. Because the negative sign is required, the = operator
    is optional.

  - `label:Code-Review=1`  
    Matches changes where there is at least one +1 score for
    Code-Review. Scores of +2 are not matched, even though they are
    higher.

  - `label:Code-Review>=1`  
    Matches changes with either a +1, +2, or any higher score.
    
    Instead of a numeric vote, you can provide a label status
    corresponding to one of the fields in the
    [SubmitRecord](rest-api-changes.html#submit-record) REST API entity.

  - `label:Non-Author-Code-Review=need`  
    Matches changes where the submit rules indicate that a label named
    `Non-Author-Code-Review` is needed. (See the [Prolog
    Cookbook](prolog-cookbook.html#NonAuthorCodeReview) for how this
    label can be configured.)

  - `label:Code-Review=+2,aname`; `label:Code-Review=ok,aname`  
    Matches changes with a +2 code review where the reviewer or group is
    aname.

  - `label:Code-Review=2,user=jsmith`  
    Matches changes with a +2 code review where the reviewer is jsmith.

  - `label:Code-Review=+2,user=owner`;
    `label:Code-Review=ok,user=owner`; `label:Code-Review=+2,owner`;
    `label:Code-Review=ok,owner`  
    The special "owner" parameter corresponds to the change owner.
    Matches all changes that have a +2 vote from the change owner.

  - `label:Code-Review=+1,group=ldap/linux.workflow`  
    Matches changes with a +1 code review where the reviewer is in the
    ldap/linux.workflow group.

  - `label:Code-Review<=-1`  
    Matches changes with either a -1, -2, or any lower score.

  - `is:open label:Code-Review+2 label:Verified+1 NOT label:Verified-1
    NOT label:Code-Review-2`; `is:open label:Code-Review=ok
    label:Verified=ok`  
    Matches changes that are ready to be submitted according to one
    common label configuration. (For a more general check, use
    [submittable:ok](#submittable).)

  - `is:open (label:Verified-1 OR label:Code-Review-2)`; `is:open
    (label:Verified=reject OR label:Code-Review:reject)`  
    Changes that are blocked from submission due to a blocking score.

## Magical Operators

Most of these operators exist to support features of Gerrit Code Review,
and are not meant to be accessed by the average end-user. However, they
are recognized by the query parser, and may prove useful in limited
contexts to administrators or power-users.

  - visibleto:'USER-or-GROUP'  
    Matches changes that are visible to *USER* or to anyone who is a
    member of *GROUP*. Here group names may be specified as either an
    internal group name, or if LDAP is being used, an external LDAP
    group name. The value may be wrapped in double quotes to include
    spaces or other special characters. For example, to match an LDAP
    group: `visibleto:"CN=Developers, DC=example, DC=com"`.
    
    This operator may be useful to test access control rules, however a
    change can only be matched if both the current user and the supplied
    user or group can see it. This is due to the implicit *is:visible*
    clause that is always added by the server.

  - is:visible  
    Magical internal flag to prove the current user has access to read
    the change. This flag is always added to any query.

  - starredby:'USER'  
    Matches changes that have been starred by *USER* with the default
    label. The special case `starredby:self` applies to the caller.

  - watchedby:'USER'  
    Matches changes that *USER* has configured watch filters for. The
    special case `watchedby:self` applies to the caller.

  - draftby:'USER'  
    Matches changes that *USER* has left unpublished draft comments on.
    Since the drafts are unpublished, it is not possible to see the
    draft text, or even how many drafts there are. The special case of
    `draftby:self` will find changes where the caller has created a
    draft comment.

  - limit:'CNT'  
    Limit the returned results to no more than *CNT* records. This is
    automatically set to the page size configured in the current user’s
    preferences. Including it in a web query may lead to unpredictable
    results with regards to pagination.

## GERRIT

Part of [Gerrit Code Review](index.html)

## SEARCHBOX
