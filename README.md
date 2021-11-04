# wipbak

*Work-in-progress backup.*

The `wipbak` BASH script creates local work-in-progress backup files.

## Setup for the User

Put the `wipbak` script in a directory that is in the *PATH*.  For example, if the script is in a *bin* directory under the home directory, add this to *.bashrc*:

```bash
  export PATH=$PATH:$HOME/bin
```

Note: If *~/bin* is used for something else, use *~/tools* (or another available directory name) instead.

## Setup for a Project

To use this script, the directory where it is to be run must have two things:
1. A file named `wipbak-list.txt` that contains the names of files for which backup copies are to be made, one per line.
2. A sub-directory named `_0_bak` where the copies will be placed.

## Using the Script

At the console, in a project directory, run the `wipbak` command.

The *wipbak* script reads `wipbak-list.txt` and processes each file in the list. It uses the `find` and `grep` commands to look for the most recent backup. If a backup is found, it uses the `diff` command to compare the backup to the current file. If the current file is different, or no backup exists, a new backup copy is made in the `_0_bak` directory.

## Using the Backups

You may want to compare a backup to the current file, and perhaps restore some, or all, of the file's text to a previous state. You may want to compare backups to each other to look for specific changes, or to see changes over time. A text editor will work for that, but using a visual data comparison (diff) tool seems more effective. I use [Beyond Compare](https://www.scootersoftware.com/). It is a commercial product. There are other free and open source tools [available](https://en.wikipedia.org/wiki/Comparison_of_file_comparison_tools).

## History

While working on a file (a script or other document), I would often make copy-and-paste backups along the way. After pasting a copy I would rename the file to have a *.bak* extension, and include the date and time (or an incremented sequence number) in the file name. These were typically very small projects with a few files in a single folder. 

The manual process of making copy-and-paste backups seemed like a good candidate for automation, so I made a BASH script called `wipbak.sh` in a project I was working on. It was specific to that project. Later I copied it to another project and changed the custom bits for the other project. This went on for a number of small projects on multiple machines (actual and virtual). Then, in one project, I ran into a snag.

There was a file name in the project that revealed a weakness in the simple *grep* command used to select files for backup in the script. It was easy to fix for this specific instance, but I felt like this was a *bug* that would be *lurking* if I did not update all other instances of the script in other places. 

This was not unforeseen. Every time I copied `wipbak.sh`, and customized it for another project, I though about generalizing it, or using git, but that was not the task and hand. The script was easy to set up and worked well for my small-project workflow.  However, with a bug to fix across all instances of the script, I decided to make generalizing it *the* task at hand.

An example of the original per-project script is in `wipbak-sh.txt`.

Maybe I should use *git* (or another SCM) from the start for *everything*, but that seems like a lot of overhead for a small project with few files. I don't want to have to come up with a *commit message* every time I get to an interim stopping point. The purpose of these backups is to be able to recover from a mistake, or go back to a fork in the road and take the other path. Perhaps these should be called *snapshots* instead of *backups*. They are just local copies. As a separate process, I do "real" backups to a file server, and off-site from there. Those backups include the *wipbak* files.
