epeltools
=========

Various scripts and hacks for working with EPEL


What is EPEL?
-------------

EPEL == Extra Packages for Enterprise Linux
https://fedoraproject.org/wiki/EPEL

These are RPM packages that can be installed on Red Hat Enterprise Linux or
CentOS systems.  Possibly other RHEL-derived systems too like Scientific
Linux.


What are epeltools?
-------------------

Assorted things I've written to make working with and developing for EPEL
easier.  Here's what I have currently:


* inepel

  This is a script that can be used to report if a list of binary packages
  would conflict with a given EL repo, its updates repo, and the currently
  published repo.  Status is reported by exit code:

      0      Package is ok to add to EPEL.
      1      General error in the script.
      2      Package is already provided by an existing repo.

  Usage is simple.  Use the command line options to specify the EPEL repo
  to test against, the EL repo, and [optionally] the EL updates repo.  There
  is no effort made to verify you have selected the correct versions or
  architecture.  After the command line options, list all of the packages
  you want to check.

  EXAMPLES

  a) Build 'bash' and check to see if it can be added to EPEL-7:

     $ ./inepel -o http://mirror.centos.org/centos/7/os/x86_64/ \
                -u http://mirror.centos.org/centos/7/updates/x86_64/ \
                -e http://dl.fedoraproject.org/pub/epel/7/x86_64/ \
                $HOME/rpmbuild/RPMS/x86_64/bash-4.3.24-1.el7.dlc.x86_64.rpm
     FAIL: 'bash' is provided by multiple repos:
     http://mirror.centos.org/centos/7/os/x86_64/Packages/bash-4.2.45-5.el7.x86_64.rpm
     file:///tmp/tmp.MlDrPKXhIz/epeltest/bash-4.3.24-1.el7.dlc.x86_64.rpm

     The exit code here is 2, indicating bash is not safe for EPEL-7.

  b) Build 'clpbar' and check to see if it can be added to EPEL-7:

     $ ./inepel -o http://mirror.centos.org/centos/7/os/x86_64/ \
                -u http://mirror.centos.org/centos/7/updates/x86_64/ \
                -e http://dl.fedoraproject.org/pub/epel/7/x86_64/ \
                $HOME/rpmbuild/RPMS/x86_64/clpbar-1.10.9-11.el7.dlc.x86_64.rpm
     GOOD: 'clpbar'

     The exit code here is 0, indicating clpbar can be added to EPEL-7.

  The script itself could be rounded out more, but the idea is there.
  Patches welcome.
