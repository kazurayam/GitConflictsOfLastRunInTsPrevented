# How to prevent Git conflicts of `<lastRun>...</lastRun>` in `*.ts` files in Katalon Studio

## Problem to solve

When you create a Test Suite `Test Suite/TS1`, Katalon Stuido will make `<projectDir>/Test Suites/TS1.ts` file. The file will look like, for example, as follows:
```
<?xml version="1.0" encoding="UTF-8"?>
<TestSuiteEntity>
   <description></description>
   <name>TS1</name>
   <tag></tag>
   <isRerun>false</isRerun>
   <lastRun>2018-12-03T10:12:41</lastRun>
...
```
Here you will find a timestamp info `<lastRun>2018-12-03T10:12:41</lastRun>` included. This timestamp will be updated by Katalon Studio everytime you ran the `Test Suites/TS1`. If you share the project via Git remote repository with your team mates, this `lastRun` info likely to cause conflicts when you try merge the remote `origin/master` branch into your local `master` branch.

### How to reproduce conflicts

I have made another Katalon Studio project and published it on GitHub:
- https://github.com/kazurayam/GitConflictsOfLastRunInTsReproduced

in there I described how the annoying conflicts occurs.


## Solution proposed









## install `sed for Windows`

The method I present here requires `sed` command on your machine operational in the command line. MacOS and Linux has `sed` built-in. But Windows does not have it.

The following steps show you how to install `sed for Windows` into your Windows PC.

1. You can download the Setup program for `sed for Windows` from [here](http://gnuwin32.sourceforge.net/packages/sed.htm).
2. Find "Download > Complate package, except sources Setup" and click the [link](http://sourceforge.net/projects/gnuwin32/files//sed/4.2.1/sed-4.2.1-setup.exe/download). The Setup program will be downloaded.
3. Execute the downloaded `sed-4.2.1-setup.exe`. The installer will install the `sed` command.
4. Confirm that the `sed` is operational in the command line. Open the command prompt window, and type:

```
C:\Users\kazurayam>sed --version
```
`sed` command will respond :
```
GNU sed 4.2.1

Copyright (C) 2009 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE,
to the extent permitted by law.

GNU sed home page: <http://www.gnu.org/software/sed/>.
General help using GNU software: <http://www.gnu.org/gethelp/>.
E-mail bug reports to: <bug-gnu-utils@gnu.org>.
Be sure to include the word 'sed' somewhere in the 'Subject:' field.
```

If you see garbled characters in the Command prompt, you need to change the code page to be UTF-8:
```
C:\Users\kazurayam>chcp 65001
```



Edit the followin file with your favorite text editor.
-  `%USERPROFILE%\.gitconfig`  on Windows
- `~/.gitconfig` on Mac/Linux

You want to insert the following 3 lines:
```
[filter "lastRun-in-ts"]
	smudge = sed "s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!"
	clean = sed "s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!"
```


Then you add `.gitattributes` file into the project directory:
```
*.ts filter=lastRun-in-ts
```

## Related discussions

In the Katalon Studio Forum, there are a few discussions related to the `lastRun` conflicts.

1. [Git: master->master \[rejected - non-fast-forward\]](https://forum.katalon.com/discussion/11284/git-master-master-rejected-non-fast-forward) by Mate Mrse at 11/30/2018
2. [\[Suggestion\]\[Katalon Studio\] Last Run info could be saved in a serarate file \(in the .ts file\)](https://forum.katalon.com/discussion/7146/suggestion-katalon-studio-last-run-info-could-be-saved-in-a-separate-fille-not-in-the-ts-file) by Lauro Araujo at 05/31/2018
3. [Every time I run test suite it will be updated then I have a new git commit](https://forum.katalon.com/discussion/9587/every-time-i-run-test-suite--it--will-be-updated-then-i-have-a-new-git-commit) by qiulang at 09/11/2018
4. [Commits from different machines cause conflicts in Git](https://forum.katalon.com/discussion/4881/commits-from-different-machines-cause-conflicts-in-git) by Alex Brohin at 08/04/2017

Please note one thing.
At 08/10/2017 (16 months ago), Oliver Howard posted a comment https://forum.katalon.com/discussion/comment/10492/#Comment_10492 where he presented a solution to `lastRun` conflicts. He explained how to configure Git with filter and .gitattribute file. Thanks to Oliver, but this comment seems to be unknown by Katalon users. I am going to explain just the same know-how again in a more verbose way so that it is shared by more of Katalon Studio uses working with Git.
