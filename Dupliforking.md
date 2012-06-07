# Dupliforking

This document describes a way to "fork" a project multiple times, which we call "dupliforking".

## Why Duplifork?

We invented this process because GitHub will only allow one fork per project, per organization.

Since a person might want to make more than one app using the [Bootplate](Bootplate)
template, we needed a way to enable this.

## Instructions

The following command-line instructions should work on Mac OS X, Linux, and Windows
if the [official Git client](http://git-scm.com/download/win) is installed.

Let's say you want to create a new app called "MillionDollars".

1. Clone the bootplate repository and enter the directory.

        git clone https://github.com/enyojs/bootplate.git MillionDollars

        cd MillionDollars

2. Initialize the subrepositories.

        git submodule update --init

3. Create a new GitHub repository for your app.

    [Click Here](https://github.com/repositories/new)

4. Point your clone of bootplate at your new GitHub repository.  (This step changes where the code is pushed to and pulled from.)

        git remote set-url origin git@github.com:<your user name>/MillionDollars.git

    Users of other tools may just edit the config file, `MillionDollars/.git/config`:

        [remote "origin"]
            url = git@github.com:<your user name>/MillionDollars.git
            ...

5. Push to your new repository.

You're all set!