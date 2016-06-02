+++
Description = "What if you lose your data"
Tags = []
date = "2016-06-01T22:40:27+05:30"
title = "What if you lose your data"

+++

You have been working day in and day out. All your important documents are there in your hard disk.
For most of us, ctrl + s (Save) is enough to save us from the trouble of having to deal with the loss of
data. That's bad, because you can still get into trouble. You might ask how, then picture this, what
if your hard disk gets corrupted ? What if you delete your files accidently ? What if you lose your laptop.
<!--more-->

The question we have to ask ourselves is, are we prepared for the above scenarios ? If the answer is no, now
is the time to change that.

I am going to explain the setup I've done in my laptop (Mac os x El Captain).

Its a 2 step process.

**Step 1**

Sync with any of the cloud services like Google Drive or Dropbox. I use [fswatch](https://github.com/emcrisostomo/fswatch)
to watch for changes in the directories I want to sync and use [sync.sh](https://gist.github.com/Dineshs91/ea1cb572eac2fbc28dff459346456e9c#file-sync-sh)
script to sync the directories with Google Drive. fswatch notifies if there are any changes in the
directory and I use rsync to keep the directories in sync. This comes in very handy.

    rsync -a --delete "$my_dir" "$dest_dir"

    -a (Archive and sync recursively.)
    --delete (Delete files that are removed from the directories being watched.)

This frees from manually copying all your documents to Google Drive directory. 

**Step 2**

Create compressed tar files and put them in an external storage. This step is a manual one.
You have to run the script whenever you want to take backup. You could use cron service though if you like to
automate the process. This is the script that I use [backup.sh](https://gist.github.com/Dineshs91/ea1cb572eac2fbc28dff459346456e9c#file-backup-sh)

    **Backup**
    tar -zcvf "$ext_hard_disk_dir" --directory / --exclude='node_modules' "$my_dir"

    $ext_hard_disk_dir is the destination directory where the archives will be stored.
    $my_dir is the directory which will be archived.

    -z (Specify this option, if you want to compress the backup data.)
    -v (Verbose)

    **Restore**
    Test the archive.
    tar -ztvf /archive/full-backup-02-March-2016.tar.gz

    tar -zxvf /archive/full-backup-02-March-2016.tar.gz

**_References:_**

[linux-admin-made-easy](http://www.tldp.org/LDP/lame/LAME/linux-admin-made-easy/backup-and-restore.html)

[Importance of backups](http://www.tldp.org/LDP/sag/html/backups.html)
