# Bash Prompt optimization
## Add git branch name to bash prompt
### Basic idea
In order to add branch name to bash prompt, we have to edit the PS1 variable(set value of PS1 in ~/.bash_profile).

### What is PS1?
PS1 denotes Prompt String 1, which is the one of the prompts available in Linux/Unix shell.
When you open your terminal, it will display the content defined in PS1 variable in your bash prompt.

Example prompt when we login to a machine:
```
[surendra@linuxnix common]$
```
`surendra` is the user who logged in this machine whose name is `linuxnix`, and the present working directory is `common`. `$` means the user is a normal user, and `#` means root user.
```
[root@linuxnix dev] #
```
> useful command can get above info: `w`, `who`, `hostname`, etc.

### Control Commands for PS1
> d - the date in "Weekday Month Date" format (e.g., "Tue May 26")
> e - an ASCII escape character (033)
> h - the hostname up to the first .
> H - the full hostname
> j - the number of jobs currently run in background
> l - the basename of the shells terminal device name
> n - newline
> r - carriage return
> s - the name of the shell, the basename of \$0 (the portion following the final slash)
> t - the current time in 24-hour HH:MM:SS format
> T - the current time in 12-hour HH:MM:SS format
> @ - the current time in 12-hour am/pm format
> A - the current time in 24-hour HH:MM format
> u - the username of the current user
> v - the version of bash (e.g., 4.00)
> V - the release of bash, version + patch level (e.g., 4.00.0)
> w - Complete path of current working directory
> W - the basename of the current working directory
> ! - the history number of this command
> \# - the command number of this command
> \$ - if the effective UID is 0, a \#, otherwise a \$

### Implementation
Add following lines to `~/.bash_profile`:
```Bash
parse_git_branch() {
    git branch 2>/dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}

export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "
```
Here parse_git_branch() function extract the branch name in your git repository.

In above PS1, we defined following properties:
```
\u@\h \[\033[32m\] - user, host name and its displaying color

\w\[\033[33m\] - current working directory and its displaying color

\$(parse_git_branch)\[\033[00m\] - git branch name and its displaying color
```
Reopen terminal and you will see a new bash prompt.

For example:
![Prompt with Git Branch name](../pic/prompt-with-git-branch-name.JPG)
