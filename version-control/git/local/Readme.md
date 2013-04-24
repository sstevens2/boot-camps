[Back To Testing](https://github.com/UW-Madison-ACI/boot-camps/tree/2013-04-uwmadison/python/testing) - [Forward To Remote Version Control](https://github.com/UW-Madison-ACI/boot-camps/tree/2013-04-uwmadison/version-control/git/remote)

# Local Version Control
----

**Based on materials by Katy Huff, Anthony Scopatz, Joshua R. Smith, Sri 
Hari Krishna Narayanan, and Matthew Gidden**

## Example: A Slide Deck for a Presentation

Imagine you have two computers: the first is a big, powerful desktop machine 
with a big, dual monitor setup. The second is a tiny lightweight netbook you 
take with you when you travel. Imagine also that you have a presentation at a 
conference overseas. You know that no matter how "finished" you think you are 
with your slide deck, inevitably you will realize you need to add an extra figure 
or recalculate some data AFTER leaving the country and your main desktop 
workstation. How can you be sure your slide deck, data, and the programs you've 
written are synchronized between your two machines? How can you be sure that 
every little change you've made on your netbook is reflected on your desktop? 
What happens if you start making changes on your slide deck, only to realize you 
need to drop back to the slide deck you had five days ago?

## git : What is Version Control ?

Very briefly, version control is a way to keep a backup of changing
files, to store a history of those changes, and most importantly to
allow many people in a collaboration to make changes to the same files
concurrently. There are a lot of version control systems. Wikipedia
provides both a nice vocabulary list and a fairly complete table of some
popular version control systems and their equivalent commands.

Today, we'll be using git. Git is an example of a distributed version
control system, distinct from centralized versing control systems. I'll
make the distinction clear later, but for now, the table below will
suffice.

Version Control System Tool Options

- **Distributed** 
  - Decentralized CVS (dcvs)  
  - mercurial (hg)
  - git (git) 
  - bazaar (bzr)
- **Centralized**
  - concurrent versions system (cvs)
  - subversion (svn)

## git --help : Getting Help

The first thing I like to know about any tool is how to get help. From
the command line type

    $ man git

The manual entry for the git version control system will appear before
you. You may scroll through it using arrows, or you can search for
keywords by typing **/** followed by the search term. I'm interested in
help, so I type **/help** and then hit enter. It looks like the syntax
for getting help with git is **git --help**.

To exit the manual page, type q.

Let's see what happens when we type :

    $ git --help

Excellent, it gives a list of commands it is able to help with, as well
as their descriptions.

    $ git --help
    usage: git [--version] [--exec-path[=<path>]] [--html-path]
               [-p|--paginate|--no-pager] [--no-replace-objects]
               [--bare] [--git-dir=<path>] [--work-tree=<path>]
               [-c name=value] [--help]
               <command> [<args>]

    The most commonly used git commands are:
       add        Add file contents to the index
       bisect     Find by binary search the change that introduced a bug
       branch     List, create, or delete branches
       checkout   Checkout a branch or paths to the working tree
       clone      Clone a repository into a new directory
       commit     Record changes to the repository
       diff       Show changes between commits, commit and working tree, etc
       fetch      Download objects and refs from another repository
       grep       Print lines matching a pattern
       init       Create an empty git repository or reinitialize an existing one
       log        Show commit logs
       merge      Join two or more development histories together
       mv         Move or rename a file, a directory, or a symlink
       pull       Fetch from and merge with another repository or a local branch
       push       Update remote refs along with associated objects
       rebase     Forward-port local commits to the updated upstream head
       reset      Reset current HEAD to the specified state
       rm         Remove files from the working tree and from the index
       show       Show various types of objects
       status     Show the working tree status
       tag        Create, list, delete or verify a tag object signed with GPG

    See 'git help <command>' for more information on a specific command.

## git config : Controls the behavior of git

     $ git config --global user.name "YOUR NAME"
     $ git config --global user.email "YOUR EMAIL"
     
## git init : Creating a Local Repository

To keep track of numerous versions of your work without saving numerous
copies, you can make a local repository for it on your computer. What git
does is to save the first version, then for each subsequent version it
saves only the changes. That is, git only records the difference between
the new version and the one before it. With this compact information,
git is able to recreate any version on demand by adding the changes to
the original in order up to the version of interest.

To create your own local (on your own machine) repository, you must
initialize the repository with the infrastructure git needs in order to
keep a record of things within the repository that you're concerned
about. The command to do this is **git init** .

### Exercise : Create a Local Repository

Step 1 : Initialize your repository.

    $ cd
    $ mkdir good_science
    $ cd good_science
    $ git init
    Initialized empty Git repository in /Users/swc/good_science/.git/

Step 2 : Browse the directory's hidden files to see what happened here.
Open directories, browse file contents. Learn what you can in a minute.

    $ ls -A
    .git
    $ cd .git
    $ ls -A
    HEAD        config      description hooks       info        objects     refs 

Step 3 : Use what you've learned. You may have noticed the file called
description. You can describe your repository by opening the description
file and replacing the text with a name for the repository. Mine will be
called "Reproducible Science". You may call yours anything you like.

    $ kate description &

Step 4 : Applications sometimes create files that are not needed. For
example, kate creates a temporary file called 'filename~' when you edit
the file 'filename'. You can ask git to ignore such files by editing
the file '.git/info/exclude'. Edit the file to ignore files the end with '~'.

     git ls-files --others --exclude-from=.git/info/exclude
    # Lines that start with '#' are comments.
    # For a project mostly in C, the following would be a good set of
    # exclude patterns (uncomment them if you want to use them):
    # *.[oa]
    # *~
    
## git add : Adding a File To Version Control

For the git repository to know which files within this directory you
would like to keep track of, you must add them. First, you'll need to
create one, then we'll learn the **git add** command.

### Exercise : Add a File to Your Local Repository

Step 1 : Create a file to add to your repository.

    $ kate readme.rst &

Step 2 : Inform git that you would like to keep track of future changes
in this file.

    $ git add readme.rst

## git status : Checking the Status of Your Local Copy

The files you've created on your machine are your local "working" copy.
The changes your make in this local copy aren't backed up online
automatically. Until you commit them, the changes you make are local
changes. When you change anything, your set of files becomes different
from the files in the official repository copy. To find out what's
different about them in the terminal, try:

    $ git status
    # On branch master
    #
    # Initial commit
    #
    # Changes to be committed:

    #   (use "git rm --cached <file>..." to unstage)
    #
    #       new file:   readme.rst
    #

The null result means that you're up to date with the current version of
the repository online. This result indicates that the current difference
between the repository HEAD (which, so far, is empty) and your
good\_science directory is this new readme.rst file.

## git commit : Saving a Snapshot

In order to save a snapshot of the current state (revision) of the
repository, we use the commit command. This command is always associated
with a message describing the changes since the last commit and
indicating their purpose. Informative commit messages will serve you
well someday, so make a habit of never committing changes without at
least a full sentence description.

**ADVICE: Commit often**

In the same way that it is wise to often save a document that you are
working on, so too is it wise to save numerous revisions of your code.
More frequent commits increase the granularity of your **undo** button.

**ADVICE: Good commit messages**

There are no hard and fast rules, but good commits are atomic: they are the smallest change that remain meaningful. A good commit message usually contains a one-line description followed by a longer explanation if necessary.

[Our repo](https://github.com/USERNAME/boot-camps/commits/YYYY-MM-PLACE) has some good commit messages.

### Exercise : Commit Your Changes

Step 1 : Commit the file you've added to your repository.

    $ git commit -am "This is the first commit. It adds a readme file."
    [master (root-commit) 1863aef] This is the first commit. It adds a readme file.
     1 files changed, 2 insertions(+), 0 deletions(-)
     create mode 100644 readme.rst

Step 2 : Admire your work.

    $ git status
    # On branch master
    nothing to commit (working directory clean)

## git diff : Viewing the Differences

There are many diff tools.

If you have a favorite you can set your default git diff tool to execute
that one. Git, however, comes with its own diff system.

Let's recall the behavior of the diff command on the command line.
Choosing two files that are similar, the command :

    $ diff file1 file2

will output the lines that differ between the two files. This
information can be saved as what's known as a patch, but we won't go
deeply into that just now.

The only difference between the command line diff tool and git's diff
tool is that the git tool is aware of all of the revisions in your
repository, allowing each revision of each file to be treated as a full
file.

Thus, git diff will output the changes in your working directory that
are not yet staged for a commit. To see how this works, make a change in
your readme.rst file, but don't yet commit it.

    $ git diff

A summarized version of this output can be output with the --stat flag :

    $ git diff --stat

To see only the differences in a certain path, try:

    $ git diff HEAD -- [path]

To see what IS staged for commit (that is, what will be committed if you
type git commit without the -a flag), you can try :

    $ git diff --cached

## git log : Viewing the History

A log of the commit messages is kept by the repository and can be
reviewed with the log command.

    $ git log
    commit 1863aefd7db752f58226264e5f4282bda641ddb3
    Author: Joshua Smith <joshua.r.smith@gmail.com>
    Date:   Wed Feb 8 16:08:08 2012 -0600

        This is the first commit. It adds a readme file.

There are some useful flags for this command, such as

    -p
    -3
    --stat
    --oneline
    --graph
    --pretty=short/full/fuller/oneline
    --since=X.minutes/hours/days/weeks/months/years or YY-MM-DD-HH:MM
    --until=X.minutes/hours/days/weeks/months/years or YY-MM-DD-HH:MM
    --author=<pattern>

## git reset : Unstaging a staged file
    git reset filename     (opposite of 'git add filename')

## git checkout : Discarding unstaged modifications (git checkout has other purposes)
    git checkout -- filename     
    
## git rm : Removing files
   git rm filename   (Removes a file from the repository)
   
### Exercise : 
    1) Create 5 files in your directory with one line of content in each file.
    2) Commit the files to the repository.
    3) Change 2 of the 5 files and commit them.
    4) Undo the changes in step 3)
    5) Print out the last entry in the log.
    
## git branch : Listing, Creating, and Deleting Branches

Branches are parallel instances of a repository that can be edited and
version controlled in parallel. They are useful for pursuing various
implementations experimentally or maintaining a stable core while
developing separate sections of a code base.

Without an argument, the **branch** command lists the branches that
exist in your repository.

    $ git branch
    * master

The master branch is created when the repository is initialized. With an
argument, the **branch** command creates a new branch with the given
name.

    $ git branch experimentals
    $ git branch
    * master
      experimental

To delete a branch, use the **-d** flag.

    $ git branch -d experimental
    $ git branch
    * master

## git checkout : Switching Between Branches, Abandoning Local Changes

The **git checkout** command allows context switching between branches
as well as abandoning local changes.

To switch between branches, try

    $ git branch newbranch 
    $ git checkout newbranch 
    $ git branch

How can you tell we've switched between branches? When we used the
branch command before there was an asterisk next to the master branch.
That's because the asterisk indicates which branch you're currently in.

## git merge : Merging Branches

At some point, the experimental branch may be ready to become part of
the core or two testing branches may be ready to be combined for further
integration testing. The method for combining the changes in two
parallel branches is the **merge** command.

### Exercise : Create and Merge Branches

Step 1 : Create two new branches and list them

    $ git branch first
    $ git branch second

Step 2 : Make changes in each new branch and commit them.

    $ git checkout first
    Switched to branch 'first'
    $ touch firstnewfile
    $ git add firstnewfile
    $ git commit -am "Added firstnewfile to the first branch."
    [first 68eba44] Added firstnewfile to first branch.
     0 files changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 firstnewfile
    $ git checkout second
    Switched to branch 'second'
    $ touch secondnewfile
    $ git add secondnewfile
    $ git commit -am "Added secondnewfile to the second branch."
    [second 45dd34c] Added secondnewfile to the second branch.
     0 files changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 secondnewfile

Step 3 : Merge the two branches into the core

    $ git checkout first
    Switched to branch 'first'
    $ git merge second
    Merge made by recursive.
     0 files changed, 0 insertions(+), 0 deletions(-)
      create mode 100644 secondnewfile
    $ git checkout master
    Switched to branch 'master'
    $ git merge first
    Updating 1863aef..ce7e4b5
    Fast-forward
     0 files changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 firstnewfile
     create mode 100644 secondnewfile

## git clone : Copying a Repository

Yesterday, you checked out a git type repository at
https://github.com/UW-Madsion-CLI/boot-camps/tree/2013-04-uwmadison

When you clone the Original repository, the one that is created on your
local machine is a copy, and will behave as a fully fledged local
repository locally. However, with the right configuration, it will be
able to pull changes from collaborators to your local machine and push
your changes to the Original repository. We'll get to that soon, but for
now, let's **fork** the repository from GitHub.

### Exercise : Cloning a Repository from GitHub

Step 1 : Pick any repository you like. There are many cool projects
hosted on GitHub. Take a few minutes here, and pick a piece of code.

Step 2 : Clone it. If you didn't find anything cool, you can chose the
"instructional" Spoon-Knife repository:

    $ git clone git@github.com/octocat/Spoon-Knife.git
    Cloning into Spoon-Knife...
    remote: Counting objects: 24, done.
    remote: Compressing objects: 100% (21/21), done.
    remote: Total 24 (delta 7), reused 17 (delta 1)
    Receiving objects: 100% (24/24), 74.36 KiB, done.
    Resolving deltas: 100% (7/7), done.

Step 3 : You should see many files download themselves onto your
machine. Let's make sure it worked. Change directories to the source
code and list the contents.

    $ cd Spoon-Knife
    $ ls 

## git pull : Pulling updates from the Original Repository

Updating your repository is like voting. You should update early and
often especially if you intend to contribute back to the upstream
repository and particularly before you make or commit any changes. This
will ensure you're working with the most up-to-date version of the
repository. Updating won't overwrite any changes you've made locally
without asking, so don't get nervous. When in doubt, update.

    $ git pull 
    Already up-to-date.

Since we just pulled the repository down, we will be up to date unless
there has been a commit by someone else to the Original repository in
the meantime.

## Make your prompt pretty

In the next section, we'll get into the gritty details of remotes and branches
as we head toward web-based storage of your repositories. It turns out that some
folks have created a way to make this kind of navigation more convenient,
showing you what branch you're on using your bash prompt. Some super nice
properties also include color-coding when you've got changed files or when your
branch is fresh.

### Exercise : Create a Local Repository

Step 1 : Download [git-prompt.sh](http://volnitsky.com/project/git-prompt/git-prompt.sh).

Step 2 : Move it to a local directory.
    
    $ mkdir ~/.source
    $ mv ~/download-dir/git-prompt.sh ~/.source/

Step 3 : Copy the following lines into your ~/.bashrc file (taken from Mike
Stewart's [website](http://mediadoneright.com/content/ultimate-git-ps1-bash-prompt)).
    
    source ~/.source/git-prompt.sh

    Color_Off="\[\033[0m\]"       # Text Reset                                                                                                           
    Yellow="\[\033[0;33m\]"       # Yellow                                                                                                               
    Green="\[\033[0;32m\]"        # Green                                                                                                                
    IBlack="\[\033[0;90m\]"       # Black                                                                                                                
    IRed="\[\033[0;91m\]"         # Red                                                                                                                  
    BYellow="\[\033[1;33m\]"      # Yellow                                                                                                               
    Time12h="\T"
    PathShort="\w"

    export PS1=$IBlack$Time12h$Color_Off'$(git branch &>/dev/null;\                                                                                      
    if [ $? -eq 0 ]; then \                                                                                                                              
      echo "$(echo `git status` | grep "nothing to commit" > /dev/null 2>&1; \                                                                           
      if [ "$?" -eq "0" ]; then \                                                                                                                        
        # @4 - Clean repository - nothing to commit                                                                                                      
        echo "'$Green'"$(__git_ps1 " (%s)"); \                                                                                                           
      else \                                                                                                                                             
        # @5 - Changes to working tree                                                                                                                   
        echo "'$IRed'"$(__git_ps1 " {%s}"); \                                                                                                            
      fi) '$BYellow$PathShort$Color_Off'\$ "; \                                                                                                          
    else \                                                                                                                                               
      # @2 - Prompt when not in GIT repo                                                                                                                 
      echo " '$Yellow$PathShort$Color_Off'\$ "; \                                                                                                        
    fi)'

Step 4 : Play around with it.

## Resources

[git book](http://git-scm.com/book)