---
title: " gerrit set-project-parent"
sidebar: cmd_sidebar
permalink: cmd-set-project-parent.html
---
## NAME

gerrit set-project-parent - Change the project permissions are inherited
from.

## SYNOPSIS

> 
> 
>     ssh -p <port> <host> gerrit set-project-parent
>       [--parent <NAME>]
>       [--children-of <NAME>]
>       [--exclude <NAME>]
>       <NAME> …

## DESCRIPTION

Changes the project that permissions are inherited through. Every
project inherits permissions from another project, by default this is
`All-Projects`. This command sets the project to inherit through another
one.

## ACCESS

Caller must be a member of the privileged *Administrators* group.

## SCRIPTING

This command is intended to be used in scripts.

## OPTIONS

  - \--parent  
    Name of the parent to inherit through. If not specified, the parent
    is set back to the default `All-Projects`.

  - \--children-of  
    Name of the parent project for which all child projects should be
    reparented. If the new parent project or any project in its parent
    line is a child of this parent project it is automatically excluded
    from reparenting.

  - \--exclude  
    Name of a child project that should not be reparented. This option
    can only be used if the option --children-of is set. Multiple child
    projects can be excluded from reparenting by specifying the
    --exclude option multiple times. Excluding a project that is not a
    child project has no effect.

## EXAMPLES

Configure `kernel/omap` to inherit permissions from
`kernel/common`:

``` 
        $ ssh -p 29418 review.example.com gerrit set-project-parent --parent kernel/common kernel/omap
```

Reparent all children of `myParent` to `myOtherParent`:

``` 
        $ ssh -p 29418 review.example.com gerrit set-project-parent \
          --children-of myParent --parent myOtherParent
```

## SEE ALSO

  - [Access Controls](access-control.html)

## GERRIT

Part of [Gerrit Code Review](index.html)

## SEARCHBOX

