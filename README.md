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

## Note on OS

This is written for someone developing on either a Linux-based system or a Mac
OSX system --- for Windows or other systems, it might be easiest to run a linux
virtual machine. Unfortunately we don't go into that here.

## Guide Overview

In this guide to contributing to the LMFDB, we'll examine how to

1. install sage and the dependencies to run the LMFDB,
2. get a local copy of the LMFDB and set up a development
   environment,
3. inspect pages of the website and determine where the relevant code is
   in the LMFDB itself,
4. make changes to the local copy of the LMFDB and check them for
   errors, and how to
5. submit changes to the LMFDB, and how the review process works.
