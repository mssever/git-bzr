git-bzr: a bidirectional git - bazaar gateway
---------------------------------------------

This is a Python version of the [original Ruby `git-bzr` script](http://github.com/pieter/git-bzr).


There is also [an `sh` version](http://github.com/kfish/git-bzr).

I wrote this python version because I know python well and it was a
rather quick conversion.  The code and this README file are a direct
port of the Ruby original, with thanks.

Does it work?
-------------

Yes, at the moment, it does.  I am using _git_ version 1.7.1, _bzr_ version
2.0.2, and the current development version (revision 268) of
_bzr-fastimport_.  It looks like this setup will allow me to pull in a
bzr repo, and push back git commits to that repo, and fetch a new commit
from the bzr repo.  Here's a test script that should work:

```shell
mkdir test-git
mkdir test-bzr
cd test-bzr
bzr init
echo 'text' > afile
bzr add afile
bzr commit -m 'A file'
cd ../test-git
# make git repo and import
git init
git-bzr add bzr-repo ../test-bzr
git-bzr fetch bzr-repo
# make a git commit and push
git co -b bzr-local bzr/bzr-repo
echo 'more text' >> afile
git commit -am 'More text'
git-bzr push bzr-repo
# Make another one and push
echo 'more text again' >> afile
git commit -am 'More text again'
git-bzr push bzr-repo
# Make another bzr commit
cd ../test-bzr
echo 'even more text' > another_file
bzr add another_file
bzr commit -m 'Another file'
# import it - whoops - crash
cd ../test-git
git-bzr fetch bzr-repo
```

There's a [bug report from March 2009](https://bugs.launchpad.net/bzr-fastimport/+bug/347729) describing the problems that I've tried to hack around.


I'd be happy to hear from y'all.

What does it do?
----------------

This script should allow you to add bazaar repositories as git branches
in your git repository. After that, you can fetch the Bazaar repo, make
some changes, and push it back into Bazaar.

How does it work?
-----------------

An example session goes like this:

```shell
$ git-bzr add upstream ../bzr-branch
$ git-bzr fetch upstream
$ git checkout -b local_branch bzr/upstream
$ # Hack hack, merge merge....
$ git-bzr push upstream
```

An example session with filter goes like this:

```shell
$ git-bzr add upstream ../bzr-branch "-x path/to/exclude"
$ git-bzr fetch upstream
$ git checkout -b local_branch bzr/upstream
$ # Hack hack, merge merge....
$ git-bzr push upstream
```

WARNING:
  Still have issues with git-bzr push when working with git filtered repo

How should I install it?
------------------------

You need a newish Git (v 1.6.0 or higher). 

Furthermore, you need the Bazaar fastimport plugin - `bzr-fastimport`

Finally, you need to install the `git-bzr` script, which is written in
Python, somewhere. 

How is it licensed?
-------------------

The git-bzr script is licensed under the same license as Git.


- _git_: http://git-scm.com/
- _bzr_: http://bazaar.canonical.com/
- _bzr-fastimport_: https://launchpad.net/bzr-fastimport
