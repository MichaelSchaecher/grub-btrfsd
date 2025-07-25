---
title: GRUB-BTRFSD.CONF
section: 5
header: User Commands
footer: grub-btrfsd.conf(5)
date: 2025-03-29
author:
    - Michael Lee Schaecher
    - Pascal Jäger
---

# NAME

grub-btrfs - Automatically add btrfs-Snapshots as a Grub submenu

# SYNOPSIS

`/etc/grub.d/41_snapshots-btrfs [-V, --version]`

# DESCRIPTION

Improves grub by adding "btrfs snapshots" to the grub menu.

You can boot your system on a "snapshot" from the grub menu. Supports manual snapshots, snapper and timeshift. Features of grub-btrfs:

---

:   Automatically list snapshots existing on root partition (btrfs).

---

:   Automatically detect if /boot is in separate partition.

---

:   Automatically detect kernel, initramfs and intel/amd microcode in /boot directory on snapshots.

---

:   Automatically create corresponding "menuentry" in grub.cfg

---

:   Automatically detect the type/tags and descriptions/comments of snapper/timeshift snapshots.

---

:   Automatically generate grub.cfg if you use the provided systemd service.

# CONFIGURATION

grub-btrfs is configured via the file `/etc/default/grub-btrfs/config`. Possible options are:

## GENERAL

## `GRUB_BTRFS_DISABLE`

Disable grub-btrfs if true.

---

:   Default: "false"

---

:   Example: `GRUB_BTRFS_DISABLE="true"`

## `GRUB_BTRFS_TITLE_FORMAT`

The snapshot entries submenu in Grub are added according to this line. It is possible to change to order of the fields.

---

:   Default: ("date" "snapshot" "type" "description")

---

:   Example:
    `GRUB_BTRFS_TITLE_FORMAT=("date" "snapshot" "type" "description")`

## `GRUB_BTRFS_LIMIT`

Maximum number of snapshots in the GRUB snapshots sub menu.

---

:   Default: "50"

---

:   Example: `GRUB_BTRFS_LIMIT="50"`

## `GRUB_BTRFS_SUBVOLUME_SORT`

Sort the found subvolumes by "ogeneration" or "generation" or "path" or "rootid".

---

:   See Sorting section in **btrfs-subvolume**(8)

"-rootid" means list snapshot by new ones first.

---

:   Default: "-rootid"

---

:   Example: `GRUB_BTRFS_SUBVOLUME_SORT="+ogen,-gen,path,rootid"`

## `GRUB_BTRFS_SHOW_SNAPSHOTS_FOUND`

Show snapshots found during run "grub-mkconfig"

---

:   Default: "true"

---

:   Example: `GRUB_BTRFS_SHOW_SNAPSHOTS_FOUND="false"`

## `GRUB_BTRFS_ROOTFLAGS`

Comma separated mount options to be used when booting a snapshot. They can be defined here as well as in the "/" line inside the respective snapshots' "/etc/fstab" files. Mount options found in both places are combined, and this variable takes priority over \`fstab\` entries.

NB: Do NOT include "subvol=\..." or "subvolid=\..." here.

---

:   Default: ""

---

:   Example: `GRUB_BTRFS_ROOTFLAGS="space_cache,commit=10,norecovery"`

## `GRUB_BTRFS_OVERRIDE_BOOT_PARTITION_DETECTION`

By default "grub-btrfs" automatically detects your boot partition, either located at the system root or on a separate partition or in a subvolume, Change to "true" if your boot partition is not detected as separate.

---

:   Default: "false"

---

:   Example: `GRUB_BTRFS_OVERRIDE_BOOT_PARTITION_DETECTION="true"`

## CUSTOM KERNELS

## `GRUB_BTRFS_NKERNEL` / `GRUB_BTRFS_NINIT` / `GRUB_BTRFS_CUSTOM_MICROCODE`

By default, "grub-btrfs" automatically detects most existing kernels, initramfs and microcode. Customs kernel, initramfs and microcodes that are not detected can be added in these variables.

---

:   Default: ("")

---

:   Example:

    `GRUB_BTRFS_NKERNEL=("kernel-5.19.4-custom" "vmlinux-5.19.4-custom")`

    `GRUB_BTRFS_NINIT=("initramfs-5.19.4-custom.img" "initrd-5.19.4-custom.img" "otherinit-5.19.4-custom.gz")`

    `GRUB_BTRFS_CUSTOM_MICROCODE=("custom-ucode.img" "custom-uc.img "custom_ucode.cpio")`

## `GRUB_BTRFS_SNAPSHOT_KERNEL_PARAMETERS`

Additional kernel command line parameters that should be passed to the kernelwhen booting a snapshot. For dracut based distros this could be useful to pass "rd.live.overlay.overlayfs=1" or "rd.live.overlay.readonly=1" to the Kernel for booting read only snapshots.

---

:   Default: ""

---

:   Example:
    `GRUB_BTRFS_SNAPSHOT_KERNEL_PARAMETERS="rd.live.overlay.overlayfs=1"`

## SNAPSHOT FILTERING

## `GRUB_BTRFS_IGNORE_SPECIFIC_PATH`

Ignore specific path during run "grub-mkconfig". Only exact paths are ignored. e.g : if \`specific path\` = @, only \`@\` snapshot will be ignored.

---

:   Default: ("@")

---

:   Example: `GRUB_BTRFS_IGNORE_SPECIFIC_PATH=("@home")`

## `GRUB_BTRFS_IGNORE_PREFIX_PATH`

Ignore prefix path during run "grub-mkconfig". Any path starting with the specified string will be ignored. e.g : if \`prefix path\` = @, all snapshots beginning with "@/\..." will be ignored.

---

:   Default: ("var/lib/docker" "@var/lib/docker" "@/var/lib/docker")

---

:   Example:
    `GRUB_BTRFS_IGNORE_PREFIX_PATH=("var/lib/docker" "@var/lib/docker" "@/var/lib/docker")`

## `GRUB_BTRFS_IGNORE_SNAPSHOT_TYPE`

Ignore specific type/tag of snapshot during run "grub-mkconfig". For snapper: Type = single, pre, post. For Timeshift: Tag = boot, ondemand, hourly, daily, weekly, monthly.

---

:   Default: ("")

---

:   Example: `GRUB_BTRFS_IGNORE_SNAPSHOT_TYPE=("ondemand")`

## `GRUB_BTRFS_IGNORE_SNAPSHOT_DESCRIPTION`

Ignore specific description of snapshot during run "grub-mkconfig".

---

:   Default: ("")

---

:   Example: `GRUB_BTRFS_IGNORE_SNAPSHOT_DESCRIPTION=("timeline")`

## DISTRIBUTION DEPENDENT SETTINGS

## `GRUB_BTRFS_BOOT_DIRNAME`

Location of kernels/initramfs/microcode. Used by "grub-btrfs" to detect the boot partition and the location of kernels, initramfs and microcodes.

---

:   Default: "/boot"

---

:   Example: `GRUB_BTRFS_BOOT_DIRNAME="/"`

## `GRUB_BTRFS_GRUB_DIRNAME`

Location of the folder containing the "grub.cfg" file. Might be grub2 on some systems. For example, on Fedora with EFI : "/boot/efi/EFI/fedora"

---

:   Default: "/boot/grub"

---

:   Example: `GRUB_BTRFS_GRUB_DIRNAME="/boot/grub2"`

## `GRUB_BTRFS_GBTRFS_DIRNAME`

Location where grub-btrfs.cfg should be saved. Some distributions (like OpenSuSE) store those file at the snapshot directory instead of boot. Be aware that this directory must be available for grub during startup of the system.

---

:   Default: `$GRUB_BTRFS_GRUB_DIRNAME`

---

:   Example: `GRUB_BTRFS_GBTRFS_DIRNAME="/.snapshots"`

## `GRUB_BTRFS_GBTRFS_SEARCH_DIRNAME`

Location of the directory where Grub searches for the grub-btrfs.cfg file. Some distributions (like OpenSuSE) store those file at the snapshot directory instead of boot. Be aware that this directory must be available for grub during startup of the system.

---

:   Default: "\${prefix}" (This is a grub variable that resolves to where grub is

installed. (like /boot/grub, /boot/efi/grub))

---

:   NOTE: If variables of grub are used here like \${prefix}, they need to be escaped

with \`\$\` before the \`\$\`

---

:   Example: `GRUB_BTRFS_GBTRFS_SEARCH_DIRNAME="${prefix}"`

## `GRUB_BTRFS_MKCONFIG`

Name/path of the command to generate the grub menu, used by "grub-btrfs.service" Might be 'grub2-mkconfig' on some systems (e.g. Fedora) Default paths are /sbin:/bin:/usr/sbin:/usr/bin, if your path is missing, report it on the upstream project. You can use the name of the command only or full the path.

---

:   Default: grub-mkconfig

---

:   Example: `GRUB_BTRFS_MKCONFIG=/sbin/grub2-mkconfig`

## `GRUB_BTRFS_SCRIPT_CHECK`

Name of grub-script-check command, used by "grub-btrfs" Might be 'grub2-script-check' on some systems (e.g. Fedora)

---

:   Default: grub-script-check

---

:   Example: `GRUB_BTRFS_SCRIPT_CHECK=grub2-script-check`

## `GRUB_BTRFS_MKCONFIG_LIB`

Path of grub-mkconfiglib file, used by "grub-btrfs" Might be '/usr/share/grub2/grub-mkconfiglib' on some systems (e.g. Opensuse)

---

:   Default: /usr/share/grub/grub-mkconfiglib

---

:   Example:
    `GRUB_BTRFS_MKCONFIG_LIB=/usr/share/grub2/grub-mkconfig_lib`

## SECURITY

## `GRUB_BTRFS_PROTECTION_AUTHORIZED_USERS`

Password protection management for submenu, snapshots Refer to the [Grub documentation](https://www.gnu.org/software/grub/manual/grub/grub.html#Authentication-and-authorisation) and this [commint](https://github.com/Antynea/grub-btrfs/issues/95#issuecomment-682295660) Add authorized usernames separate by comma (userfoo,userbar). When Grub's password protection is enabled, the superuser is authorized by default, it is not necessary to add anything.

---

:   Default: ""

---

:   Example: `GRUB_BTRFS_PROTECTION_AUTHORIZED_USERS="userfoo,userbar"`

## `GRUB_BTRFS_DISABLE_PROTECTION_SUBMENU`

Disable authentication support for submenu of Grub-btrfs only (\--unrestricted) does not work if GRUBBTRFSPROTECTIONAUTHORIZEDUSERS is not empty

---

:   Default: "false"

---

:   Example: `GRUB_BTRFS_DISABLE_PROTECTION_SUBMENU="true"`

# FILES

/etc/default/grub-btrfs/config

# SEE ALSO

*btrfs*(8) *btrfs-subvolume*(8) *grub-btrfsd*(8) *grub-mkconfig*(8)

# COPYRIGHT

Copyright (c) 2022 Pascal Jäger
Copyright (c) 2025 Michael Lee Schaecher
