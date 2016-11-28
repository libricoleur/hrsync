hrsync
======

A shell script showing how to backup a directory with rsync detecting moved and renamed files while also keeping snapshots of your previous backups.

Rsync is a great tool but it lacks detection of moved and renamed files. With this simple script it is possible to avoid transfer of files which were only moved around or renamed. As an added bonus, your previous backups will be kept as snapshots.

The basic idea is to create a tree (inside source directory) of hard linked files representing the original filesystem structure of the source itself, letting rsync reconstructing links in the initial phase of the transfer.
After the initial run you will find a new directory (.rsync_shadow) inside source and target directories. Please don't touch the trees inside if you want the script to continue working.

For every backup you make, a snapshot directory containing your tree will be created. A symbolic link named 'latest' will point to the latest snapshot. It is safe to delete any older snapshot whenever you want to free up space.

Forked from https://github.com/dparoli/hrsync.

Credits due to this brilliant article: [Detecting File Moves & Renames with Rsync](http://lincolnloop.com/blog/detecting-file-moves-renames-rsync/).

Some background info in this serverfault question: [Handling renamed files or directories in rsync](http://serverfault.com/q/489289/220035)

I provide only this README as documentation. If you want to know more just look at the source code, it's really short!

Requirements
------------

* rsync >= 3.0
* filesystem with support for hard links on both sides
* source and target directory should be on different devices
* source on local filesystem and optionally target on remote (mounted locally)

Usage
-------

If your target directory already contains a copy of your documents, please move it to a subdirectory (eg. 'origin') and make a symbolic link named 'latest' pointing to it.

Run the script with source and target directories as arguments:

```sh
cd <dir>
chmod +x ./hrsync
./hrsync /home/user/Documents /media/user/external/Documents
```

Do your work on the source directory: add, delete and move files, then rerun the script. 
Target directory will be synced without transferring moved or renamed files.

Please note that this script is to be considered only an example, feel free to extend the idea for your own backup solution.
Use it at your own risk, this is alpha software provided "as is" without any warranty in any case.

License
-------

hrsync is © 2014 Daniele Paroli. It is free software, and may be redistributed under the terms specified in the LICENSE file.
