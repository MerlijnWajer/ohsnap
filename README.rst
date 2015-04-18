======
ohsnap
======

ohsnap is a simple tool to manage btrfs snapshots.
The tool is very much experimental and should not be considered safe to use.


Design
======

ohsnap creates snapshots of local subvolumes every (configurable) timeperiod,
with 'daily' being a sensible default. It purposely has a very simple
workflow, to make it simple to manipulate the workflow to fit your need(s).

Essentially, ohsnap:

- Creates (daily) snapshots of your subvolumes. 
- Features an algorithm to remove snapshots after a certain time period.
- Create incremental backup-files that can be send to a remote machine.

The design goals:

* Simple to create and manage backups locally
* Resilient and incremental backups
* Pull based -- servers can be offline for a few days and then resume
  pulling backups

Additionally, for remote snapshots, all the 'management' logic is moved to
the receiving side, this means:

* ohsnap will create (daily) dumps of incremental snapshots;
  it does not *delete* those dump files
* Those dumps will be saved locally until their are retreived by the remote,
  using `rsync --delete-after` or file synchronising method

This way, a remote machine can be offline for a longer period of time
and still retain full incremental history.



Quickstart
==========

ohsnap is a tool to automatically create and manage your btrfs snapshots;
it is purposely simple in its design and configuration.
The most basic configuration is as follows::

    snapshot_path: /mnt/raid/btrfs/snapshots
    subvolumes:
      home:
        src: /mnt/raid/my_home


TODO
----

* Make snapshots read-only (-r)
* Support send/receive to other instances of ohsnap (or just btrfs)
* Support removing snapshots
* Support cleaning snapshots after a certain date
* Support 'logarithmic' snapshots (keep one from last year,
  one from last month, one from last week, the last 5 days, etc)
