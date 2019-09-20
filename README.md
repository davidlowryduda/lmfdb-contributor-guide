# Contributing to the LMFDB

If you want to contribute to the LMFDB, then there are people who are happy to
help you learn how best to devote your efforts. There are many ways to
contribute to the LMFDB. You could

- Make webpages clearer and better
- Identify problems and report them to the issue tracker
- Fix typos and bugs
- Add data to the LMFDB
- Write or edit knowls to be more informative and clearer

The three major components of the LMFDB are

1. the content of the KNOWLs, the wikipedia-like expandable
   definitions and context on the site,
2. the structure of the webpages showing the data (i.e. what one
   sees on the website itself), and
3. the data (such as the ranks of elliptic curves).

Each of these components can be contributed to. This document will tell you how
to approach contributing to the LMFDB. The goal is to go from having no setup to
making a first change --- we're going to show how one would change the title of
the LMFDB.


### Note on OS Choice

This is written for someone developing on either a Linux-based system or a Mac
OSX system --- for Windows or other systems, it might be easiest to run a linux
virtual machine. Unfortunately we don't go into that here.


### Note on Getting Support

If you want to contribute the the LMFDB, you should contact the LMFDB team. The
best way to do this will be listed on [the LMFDB contact page][lmfdb-contact].
Further, if you encounter trouble while trying to contribute, contact the LMFDB
team or an LMFDB contributor.

[lmfdb-contact]: http://www.lmfdb.org/contact


### Guide Overview

In this guide to contributing to the LMFDB, we'll examine how to

1. [install sage and the dependencies to run the LMFDB](#installation-guide)
2. get a local copy of the LMFDB and set up a development
   environment,
3. inspect pages of the website and determine where the relevant code is
   in the LMFDB itself,
4. make changes to the local copy of the LMFDB and check them for
   errors, and how to
5. submit changes to the LMFDB, and how the review process works.


# Installation guide

(This section is based on the [Getting Started directions][lmfdb-getting-started]
on the LMFDB github repository).

[lmfdb-getting-started]: (https://github.com/LMFDB/lmfdb/blob/master/GettingStarted.md)

The goals of this section are the following:

1. Install a recent version of sage.
2. Check that sage has ssl available, and troubleshoot otherwise. (This
   sometimes affects OSX).
3. Install additional dependencies.

If you are not an administrator for your system and you do not have
permissions to install software, then you will need to ask a system
administrator to set this up for you.


## Installing sage

We discuss three options for installing sage:

- install from pre-built binaries from the sage website
- install from a package manager such as brew or apt
- building from source

### Pre-built binaries

It is easiest to install sage from a pre-built binary for your system.
Instructions for how to install sage from binaries can be found in the
[sage documentation][sage-install-doc]. But in short, this just points
you to the [download page][sage-download-page] and gives some
instructions on how to handle the downloaded file.

[sage-install-doc]: http://doc.sagemath.org/html/en/installation/binary.html
[sage-download-page]: http//www.sagemath.org/download.html

On the download page, make sure you navigate to the downloads for your
operating system: there are linux and OSX downloads.

After you've installed sage, you can run it with

    cd <your unpacked sage directory>
    ./sage

Sage should run for a few minutes and perform initial installation. If you
encounter a problem, you should consult sage's
[documentation page][sage-install-doc]. If that doesn't help, it is probably a
good idea to [contact the LMFDB team][lmfdb-contact].


### Install from a package manager

When this works, it's great. When it doesn't work, that's too bad.

On some systems (such as Ubuntu 18.04 and later), the package manager is
aware of sage. You *might* be able to get away with typing something
like

    sudo apt-get install sagemath

or

    sudo brew install sagemath

Not all package managers have sage. Many don't have up-to-date versions
of sage. And there are too many package managers and too many versions
to handle independently here.

To check your sage installation, open up a terminal and type

    sage

If sage starts up, then congratulations, you now have sage installed!

If not, then you may have to do some troubleshooting on your own, or
install from a pre-built binary as described above.


#### Troubleshooting: My package manager installed an app, but not a terminal application

There is one particular issue that might come up that is worth
mentioning, though. Sometimes a package manager will install sage as an
application, but not make the `sage` command work from the terminal. In
OSX, for instance, it is possible to install sage as an app (so that you
can click on it to start sage from the application folder), but this
doesn't make the `sage` command work from the terminal.

Such an installation of sage will *probably* work, but you'll want to
make it callable from the commandline to follow the rest of the
contribution guide (and generally). The easiest way to do that is to either
create an **alias** or a **symlink**.

For example, in a recent OSX installation, sage was installed to
`/Applications/SageMath-8.2.app/Contents/Resources/sage/sage`. A
shortterm solution was to create an alias so that typing `sage` resulted
in that application being called. This is done by typing

    alias sage=/Applications/SageMath-8.2.app/Contents/Resources/sage/sage

into a terminal. This solution can be made permanent by adding this alias line
to your `.bashrc` file.

(It is also possible to add a symbolic link to your PATH.)


### Building sage from source

Installing from source works on many, many systems. But your system
should have at least 6 GB of disk space and 2GB of RAM, and you should
know that compiling sage from source can take many hours.

This guide doesn't cover building sage from source. If you intend to build sage
from source, follow the instructions at the [sage math
documentation][sage-source].

[sage-source]: http://doc.sagemath.org/html/en/installation/source.html


## Check that sage has ssl installed

Now that you have sage, check that sage has ssl capabilities. To do
this, open a terminal, start sage, and then ask it to import ssl. That
is, you'll type

    sage

(at which time the terminal should print out what version of SageMath
you're using and give it's typical header), and then

    # (in sage)
    import ssl

A successful import will print nothing at all. If that's the case, then
congratulations! You have ssl installed and ready.

If errors are printed, then ssl is not available and this needs to be
remedied. On many systems, it is possible to install openssl
system-wide. Using apt-get, this is done with

    sudo apt-get install openssh0server openssh0client

Other package managers will have similar commands.


### Troubleshooting: Let sage build its own ssl

Alternatively, it is possible to have sage build its own ssl
capabilities. (This is particularly helpful for OSX installations). To
do this, open a terminal and type

    sage -i openssl
    sage -f python2  # This should take a bit of time
    sage -i pyopenssl
    sage -pip install --upgrade pip

What these commands do is have sage install ssl, then rebuild its
internal python with ssl (which takes a lot of time, since it checks the
signatures and hashes of its various components), then installs a python
library for ssl, and finally upgrades its internal python package
manager to now use ssl.

It should now be possible to open sage and import ssl.


## Install additional dependencies and run the LMFDB

You should now have sage with ssl installed and ready. The remaining
steps are very straightforward. These steps are

- Get git
- Copy the LMFDB code using git
- Have sage install LMFDB dependencies
- Test run the LMFDB


### Get git

First, check if you have git. To do so, open a terminal and type

    git --version

If a meaningful version comes out (such as `git version 2.7.4`), then
congratulations, you already have git and can move on to the next step.

Otherwise, you'll need to install git. Git is ubiquitous, and is
certainly available from your package manager. Thus a command like `sudo
apt-get install git` or `sudo brew install git` will probably work.

Git is good, git is great --- but for the moment we treat it like a black box
and don't worry about it. We'll revisit git later.


### Acquire the LMFDB code

The code for the LMFDB can be gound at https://github.com/LMFDB/lmfdb. This is
the code that runs the site (though not the database that the site reads from).
To get this code on your machine, you're going to clone (or download a copy of)
this code.

For development, the LMFDB follows a git-fork model. This roughly means that
each contributor has their own copy of the code and can make changes to it ---
and when they want to give these changes to the LMFDB itself, they use git.
Thus in order to contribute to the lmfdb, one must make a github account
and make a copy (fork) of the lmfdb.



#### Making a Github account and setting up a fork

Go to github.com and make a free account if you don't have one already.
Login to your account, and then go to https://github.com/LMFDB/lmfdb. In
the upper right hand corner, hit `Fork`.

Congratulations, you now have a copy of the LMFDB's code associated to
your github account!

Now you will make a local copy of your LMFDB code. Make a new directory
(which I will again suppose you call `lmfdb`).

    cd            # alternately, cd to wherever you want your code
    git clone https://github.com:<your_github_id>/lmfdb.git lmfdb

You should now have a copy of the LMFDB on your machine, but this time
it's associated to your fork of the code (instead of the main
repository). Git will refer to your github repo as `origin`.

Add the main LMFDB repository and call it `upstream` by typing

    git remote add upstream git@github.com:LMFDB/lmfdb.git

You can check that repositories called `origin` and `upstream` are known
to your local git repo by typing

    git remote -v

**Note that it is not very important to understand what git is doing
behind the scenes, or even how to use git particularly well.** It is a
good idea to take git as a black box if you are unfamiliar with it right
now, and revisit it later only if you are interested.


#### Running your local copy of the LMFDB

We now need to install the prerequisites to running the LMFDB locally.
Enter the `lmfdb` directory and type

    sage -i gap_packages
    sage -i database_gap
    sage -i pip          # May not be necessary
    sage -b              # May take a bit of time
    sage -pip install -r requirements.txt

The last command reads the `requirements.txt` file in the `lmfdb`
directory, which contains a list of sage dependencies that sage will
read and install. It may be necessary to run

    sage -pip install -r requirements.txt --upgrade

You can now run your local copy of the LMFDB by typing

    sage -python start-lmfdb.py --debug

Point your browser to `localhost://37777` to see the LMFDB.

If you see errors, either revisit the documentation or ask for help.


### Congratulations

Congratulations, you now have all the prerequisites out of the
way. Initial setup is the most tedious part of the process. Now that
it's done, the rest is much more straightforward.
