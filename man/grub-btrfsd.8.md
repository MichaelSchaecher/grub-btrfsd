---
title: GRUB-BTRFSD
section: 8
header: System Administration Utilities
footer: GRUB-BTRFSD
date: 2025-03-29
---

# NAME

grub-btrfsd - An OpenRC daemon to automatically update the grub menu
with **grub-btrfs**(8)

when a new btrfs snapshot is created.

# SYNOPSIS

`grub-btrfsd [-h, --help] [-c, --no-color] [-l, --log-file LOG_FILE] [-r, --recursive] [-s, --syslog] [-t, --timeshift-auto] [-o, --timeshift-old] [-v, --verbose] SNAPSHOTS_DIRS`

# DESCRIPTION

Grub-btrfsd is a shell script which is meant to be run as a daemon. Grub-btrfsd watches a directory where btrfs-snapshots are created or deleted via inotifywait and runs grub-mkconfig (if grub-mkconfig never ran before since grub-btrfs was installed) or `/etc/grub.d/41_snapshots-btrfs` (when grub-mkconfig ran before with grub-btrfs installed) when something in that directory changes.

# OPTIONS

## `SNAPSHOTS_DIRS`

This argument specifies the (space separated) paths where grub-btrfsd looks for newly created snapshots and snapshot deletions. It is usually defined by the program used to make snapshots. E.g. for Snapper this would be `/.snapshots`. It is possible to define more than one directory here, all directories will inherit the same settings (recursive etc.). This argument is not necessary to provide if `--timeshift-auto` is set.

## `-c / --no-color`

Disable colors in output.

## `-l / --log-file`

This arguments specifies a file where grub-btrfsd should write log messages.

## `-r / --recursive`

Watch snapshot directory recursively

## `-s / --syslog`

Write to syslog

## `-t / --timeshift-auto`

This is a flag to activate the auto detection of the path where Timeshift stores snapshots. Newer versions (\>=22.06) of Timeshift mount
their snapshots to `/run/timeshift/$PID/backup/timeshift-btrfs`. Where `$PID` is the process ID of the currently running Timeshift session. The PID is changing every time Timeshift is opened. grub-btrfsd can automatically take care of the detection of the correct PID and directory if this flag is set. In this case the argument `SNAPSHOTS_DIRS` has no effect.

## `-o / --timeshift-old`

Look for snapshots in `/run/timeshift/backup/timeshift-btrfs` instead of `/run/timeshift/$PID/backup/timeshift-btrfs`. This is to be used for Timeshift versions \<22.06.

## `-v / --verbose`

Let the log of the daemon be more verbose

## `-h / --help`

Displays a short help message.

# CONFIGURATION

The daemon is usually configured via the file `/etc/conf.d/grub-btrfsd` on openrc-init systems and `sudo systemctl edit --full grub-btrfsd` on systemd systems. In this file the arguments (See OPTIONS), that OpenRC passes to the daemon when it is started, can be configured.

## NOTES

A common configuration for Snapper would be to set `SNAPSHOTS_DIR` to `/.snapshots` and not to set `--timeshift-auto`. For Timeshift `--timeshift-auto` is set to true and `SNAPSHOTS_DIR` can be left as is.

# FILES

`/etc/conf.d/grub-btrfsd` `/usr/lib/systemd/system/grub-btrfsd.service`

# SEE ALSO

*btrfs*(8) *btrfs-subvolume*(8) *grub-btrfsd-conf*(8) *grub-mkconfig*(8)
*inotifywait*(1) *openrc*(8) *rc-service*(8) *timeshift*(1)

# COPYRIGHT

Copyright (c) 2022 Pascal Jäger
Copyright (c) 2025 Michael Lee Schaecher
