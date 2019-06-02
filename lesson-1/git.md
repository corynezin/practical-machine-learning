Version control is a necessary component of large scale software development.  Most work you do in school does not really require it.  While some of you may have used Git, many students don't understand it's full power because they have never truly needed it.  I only began to understand its importance when working in a real work environment.  This is not a course on Git, but it is a very useful tool in all software engineering and you will be expected to know it.

Of course, you don't have to learn it on your own.  I will do a brief overview to cover the basics.  All material is from the "Pro Git" book by Scott Chacon and Ben Straub.  Of course, feel free to ask questions or reference the book if anything I present is not clear.

Git is a program which seeks to enable nonlinear development of software packages distributed among many programmers.  Typically school projects are limited to four or fewer people, which is not many.  If you need motivation to listen, all projects and assignments will be submitted using Git, and you will lose points if you don't do it properly.  I'll explain what this means later.

Some logistics:
On Linux, you can install git with 
$ sudo apt install git-all

You can edit your configuration by editing ~/.gitconfig to read something like the following:

[user]
    email = cory.nezin@gmail.com
    name = corynezin

You could also obtain the same result by running the following commands:

$ git config --global user.name corynezin
$ git config --global user.email cory.nezin@gmail.com

In your case, the email and username should match those of your GitHub account.

to check that this all worked, use
$ git config --list

If you ever need help on a git command, you can use 
$ git help <verb> 
or
$ man git-<verb> 
where <verb> represents the command.  For example, you could use
$ git help config

Now for the basics: here are three sections to any Git project:

Working Directory
Your version of the project

Staging Area
Your modifications you want to make part of the project

Repository
The Git directory, officially the project.

In a typical workflow you take the repository, create some files in your working directory, place the changes you want to keep in your staging area, put those changes into the repository, and then repeat.  

-- Doing --

Now, here is how you start a Git Repository.

$ mkdir ~/example-project
$ cd ~/example-project
$ git init

To add a new file to the repository, first create one with
$ touch README.md
Now to get a view of what's going on, run
$ git status
You should see README.md in red under "Untracked files".  That means the file is in the directory but Git doesn't know about it.  To let Git know about it, add it with
$ git add README.md
and run
$ git status
You should now see "new file: README.md" in green under "Changes to be committed".  That means the file is now in the staging area.
Finally, commit the change with
$ git commit -m 'your message goes here'
and run
$ git status
You should see nothing regarding the file, because it's finally added to the repository.

It would now take quite a lot of work to permanently delete it without deleting the repository.  That is because this file is now in what we call a "tracked state"  You can confirm that it's tracked by running
$ git ls-files
and confirming that you see it there.

Now lets try making a change.  Put some helpful text in the readme:
$ echo "This is a readme!" >> README.md
and run
$ git status
You should see "modified: README.md" in red since the file is different than how the Git repository remembers it.  Again, we can add it with 
$ git add README.md
Running 
$ git status
should show the same message in green now.  Suppose you forgot some important info, run
$ echo "Oops I forgot this line." >> README.md
and run 
$ git status 
again.  You should now see the same file under both "Changes to be committed" and "Changes not staged for commit".  Weird right?  That's because these are actually referring to versions of the file rather than the file itself.  The staged version only has the first line, the change not staged for commit has the second line.  To verify this, let's use the "diff" command:

If you run
$ git diff
You should see a list of differences between your last commit and your current directory.
On the other hand, if you run
$ git diff --staged
You will only see the differences between your last commit and the changes you've staged.  In the first case, you should see both lines, in the second case you should see only one.
You can also run the diff command on particular files by inserting their name after, i.e.
$ git diff README.md
run
$ git add README.md
and
$ git commit -m 'added second line to readme'
to finish changing the file.

Similar to adding files, we can of course remove them - but be careful. If you run
$ git rm README.md
and
$ git status
You should see "deleted: README.md" in green.  The git rm command is similar to git add, so the change is already staged.  Notice that this not only removes the file from your directory, but from the repository as well.  You will still be able to get the file back if you accidentally delete something important!  It will just take a tiny bit of work.  If you want to remove a file from the repository but not your computer, you can run
$ git rm README.md --cached
This is usually the safer option, since you can always remove it manually afterward.

Git also support renaming and moving files through something like
$ git mv README.md README
though this is equivalent to moving the file, deleting the original and renaming the copy.

To check through the changes we've committed you can run
$ git log
By default this will just show you the commit hash (we'll cover this later), author, date, and message that goes along with the most recent commits, though there are options to show more.

-- Undoing --
