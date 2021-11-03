---
title: "Robocopy Notes"
author: "JMarks"
weight: 1
---

When moving large files or large amounts of files/folders across the network you should use the robocopy app. It will look something like this:
```
robocopy <Source> <Destination> [<File>[...]] [<Options>]
```
In practice it will look something like this
```
robocopy $NETWORK_PATH $NEW_NETWORK_PATH /e /zb /dcopy:t /copy:all /r:2 /w:5 /v /tee /LOG:$PATH_TO_LOG
```
{{< hint warning >}}
**List Only Flag**\
It's strongly suggested to do a test run with a robocopy command first to make sure you get the intended results, especially when the command involves moving files across the network. When running a robocopy command add the '/L' flag to run the command in 'List Only' mode which combined with a '/LOG' flag will produce a report you can review to make sure the command works as intended.
{{< /hint >}}


- /E :: copy subdirectories, including Empty ones.
- /ZB :: use restartable mode; if access denied use Backup mode.
- /DCOPY:T :: COPY Directory Timestamps.
- /COPYALL :: COPY ALL file info (equivalent to /COPY:DATSOU).  Copies the Data, Attributes, Timestamps, Ownser, Permissions and Auditing info
- /R:n :: number of Retries on failed copies: default is 1 million but I set this to retry twice.
- /W:n :: Wait time between retries: default is 30 seconds but I set this to 5 seconds
- /V :: produce Verbose output, showing skipped files.
- /TEE :: output to console window, as well as the log file.
- /LOG:file :: output status to LOG file (overwrite existing log).
- /L :: List-only mode, tells robocopy to run through the command and list what will happen but not actually copy anything

{{< hint warning >}}
**Not seeing moved files**\
I've been having an issue with robocopy moving the files but no seeing any results at the target location. Turns out that robocopy will sometimes move the files and then set them as "system files" which defaults them to hidden and they cannot be seen by showing hidden files, however are still accessible by manually typing their location. In order to prevent the system file bug use this command
`/A-:SH`
If the files have already been moved and set as hidden you can use this command in an elevated command line to unhide them
`attrib -s -h <directory>`
{{< /hint >}}
