# Git Reference

## Getting and creating git projects
In order to do anything in Git, we need to have a Git repository. This is where Git stores the data for the snapshots you are saving.

There are two main ways to get a Git repository.
* Simply initialize a new one from an existing directory, such as a new project or a project new to source control.
* Clone one from a public Git repository, as you would do if you wanted a copy or wanted to work with someone on a project.

## git init
_(initialize a directory as a Git repository)_

To create a repository from an existing directory of files, you can simply run git init in that directory. For example, let's say we have a directory with a few files in it, like this:
```bash
$ cd lwup
$ ls
README   package.json
```
This is a new project folder where we'll put a sample code. To start version controlling this with Git, we can simply run git init.
```sh
$ git init
```
Initialized empty Git repository in ```/Projects/lwup/.git/```
Now we can see that there is a .git subdirectory in our project. This is our Git repository where all the data of our project snapshots are stored.
```
$ ls -a
.        ..       .git     README   package.json
```
We now have a skeleton Git repository and we can start snapshotting our project.

In short, we use git init to make an existing directory of content into a new Git repository. We can do this in any directory at any time, completely on our local computer.

## git clone
_(copy a git repository so you can add to it)_

If we need to collaborate with someone on a project, or if we want to get a copy of a project to look at or use the code, we can clone it. We simply run the git clone [url] command with the URL of the project we want to copy.
```console
$ git clone git://github.com/schacon/simplegit.git
Initialized empty Git repository in /private/tmp/simplegit/.git/
remote: Counting objects: 100, done.
remote: Compressing objects: 100% (86/86), done.
remote: Total 100 (delta 35), reused 0 (delta 0)
Receiving objects: 100% (100/100), 9.51 KiB, done.
Resolving deltas: 100% (35/35), done.
$ cd simplegit/
$ ls
README   Rakefile lib
```
This will copy the entire history of that project so we have it locally and it will give us a working directory of the main branch of that project so we can look at the code or start editing it. If we change into the new directory, we can see the .git subdirectory - that is where all the project data is.
```console
$ ls -a
.        ..       .git     README   Rakefile lib
$ cd .git
$ ls
HEAD        description info        packed-refs
branches    hooks       logs        refs
config      index       objects
```
By default, Git will create a directory that is the same name as the project in the URL you give it - basically whatever is after the last slash of the URL. If you want something different, you can just put it at the end of the command, after the URL.

In a nutshell, you use git clone to get a local copy of a Git repository so you can look at it or start modifying it.
