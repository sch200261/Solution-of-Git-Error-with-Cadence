# Solution-of-Git-Error-with-Cadence
A way to solve the conflict between git and cadence's environment variable: %HOME. (Error: Permission Denied (publickey) when use SSH)
### Error: Permission Denied (publickey) when use SSH to make a git pull or push after install Cadence Software.

After we install Cadence Software, the Git can not work properly when we use SSH to make a push to the GitHub service (usually with a error: **Git: git@github.com: Permission Denied(publickey)**).

### Describe of the cause of the problem

The cause of the problem can be easily found  by use "**echo $HOME**" in Git bash, and the result is modified to "**/c/User/Username/AppData/Roaming/SPB_Data**" by Cadence but not "**/c/Users/Username**" where contains Git's SSH key. So Git can not find its SSH key in "**/c/Users/Username/.ssh**" and failed to connect to GitHub's repositories.

### How to solve the conflict between git and cadence's environment variable: %HOME

Open the installation directory of git (e.g. **D:\Program Files\Git**), then open profile in "**D:\Program Files\Git\etc\profile**" by VScode or anything else. Add a "**export HOME="/c/Users/Username**"" before the first line of the file. You can check it by input "**echo $HOME**" in Git bash, if the output is "**/c/Users/Username**", it works. Now you can  pull or push correctly by using Git bash console.

### Additional problem in using VScode to pull or push

Admittedly, we enable Git to find its SSH key now. However, you will find the problem still existed when you use VScode to synchronize with remote repositories because we only "told" the correct path to Git but forgot the VScode.

Now open nsswitch.conf in "**D:\Program Files\Git\etc\nsswitch.conf**". Replace "db_home: env windows cygwin desc" by "**db_home: /%H**". This code makes SSH program to find another environment variable: **$USERPROFILE** but not **$HOME** while **$USERPROFILE**'s value is "**C:\Users\Username**" where contains .ssh folder.

Now you can use SSH to connect remote repositories in any software.
