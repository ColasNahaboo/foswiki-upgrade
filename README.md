# foswiki-upgrade

A linux bash script to upgrade modern Foswiki installations

Usage:

`foswiki-upgrade [options] install_dir old_release new_release`

## Quickstart:

First, make a backup of your installation.

Copy your current running wiki installation into install_dir/
(lets call IR install_dir, OR old_release, NR new_release)

`rsync  -SHAaXx --delete --delete-excluded --force root@webserver:/foswikidir/ ID/`

Be sure to be able to modify it, e.g:

`sudo chmod -R a+rwX ID`

Uncompress the old and new full distributions, 
E.g, migrating from 2.1.10 to 2.1.11:
```
tar xfz ~/Downloads/Foswiki-2.1.10.tgz
mv Foswiki-2.1.10 OR
tar xfz ~/Downloads/Foswiki-2.1.11.tgz
mv Foswiki-2.1.11 NR
```
See what will be merged:
`foswiki-upgrade ID OR NR`
Perform the upgrade:
`foswiki-upgrade -y IS OR NR`
Then re-deploy, and fix permissions
```
rsync  -SHAaXx --delete --delete-excluded --force ID/ root@webserver:/foswikidir/ ID/
ssh root@webserver chown -R web-user:web-group /foswikidir
  ```
Done, Test it!

## Features
This script supposes a modern version of Foswiki (v2+): it overwrites blindly
the files that should not have been modified, so when customizing your wiki be
sure to never directly modify distributed files.

The script however will not overwrite blindly any topic file that has been
edited via Foswiki itself, by checking the author metadata. It will perform a
3-way merge, as automated as possible.

It also works only on a local copy of the site. You will have to download
yourself a copy of your site, upgrade, and then re-upload it in place.
This keeps the script simpler and safer.

It works on linux, but should work in linux-like layers on top of other OSes,
such as cygwin, WSL, Homebrew, ...

## Options
```
  -y      yes, do perform the merges, otherwise do nothing,
          just print the files needing a merge.
  -T DIR  uses DIR instead of tmp to store temporary files.
  -kd3    uses kdiff3 to perform automatic merges, reverting to interactive
          only on conflicts (default).
  -kd3i   uses kdiff3 but always in interactive mode.
  -xxd    uses xxdiff to perform interactive merges.
  -tkd    uses tkdiff to perform interactive merges.
  -mp PROG   merge program to use, will be called with args: OR ID NR.
  -v      verbose mode.
```

## License

MIT License - (c) 2026 Colas Nahaboo.
In a nutshell: do whatever you want with this, and please credit me, but expect no warranty.

## History
- v1.0.0 - 2026-03-26
