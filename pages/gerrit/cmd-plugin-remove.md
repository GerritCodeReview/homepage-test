---
title: " plugin remove"
sidebar: cmd_sidebar
permalink: cmd-plugin-remove.html
---
## NAME

plugin remove - Disable plugins.

plugin rm - Disable plugins.

## SYNOPSIS

> 
> 
>     ssh -p <port> <host> gerrit plugin remove | rm
>       <NAME> …

## DESCRIPTION

Disable plugins. The plugins will be disabled by renaming the plugin
jars in the site path’s `plugins` directory to
`<plugin-jar-name>.disabled`.

## ACCESS

  - Caller must be a member of the privileged *Administrators*
    group.

  - [plugins.allowRemoteAdmin](config-gerrit.html#plugins.allowRemoteAdmin)
    must be enabled in `$site_path/etc/gerrit.config`.

## SCRIPTING

This command is intended to be used in scripts.

## OPTIONS

  - \<NAME\>  
    Name of the plugin that should be disabled. Multiple names of
    plugins that should be disabled may be specified.

## EXAMPLES

Disable a plugin:

``` 
        ssh -p 29418 localhost gerrit plugin remove my-plugin
```

## GERRIT

Part of [Gerrit Code Review](index.html)

## SEARCHBOX
