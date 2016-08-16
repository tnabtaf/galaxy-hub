INCLUDE(/Develop/LinkBox)
# The source code of Galaxy

Galaxy is an open source software under [AFL 3.0](/Admin/License) license.

The source code is publicly available and hosted at !GitHub. If you need help getting started, the [GitHub Git tutorial](https://try.github.io/) is a good places to start.

Galaxy recently transitioned development from [Bitbucket](https://bitbucket.org/galaxy/galaxy-central/) to [GitHub](https://github.com/galaxyproject/galaxy).

**Galaxy deployers** can continue to pull Galaxy from the [Bitbucket galaxy-dist repository](https://bitbucket.org/galaxy/galaxy-dist/). Alternatively, deployers can switch to the `master` branch in !GitHub.

**Galaxy developers** should use !GitHub.

The technical details regarding the transition can be found on the [the GitHub transition Trello card](https://trello.com/c/iiSBweRQ).

## GitHub

[https://github.com/galaxyproject/galaxy/](https://github.com/galaxyproject/galaxy/) is the source repository.

If you have created piece of code for Galaxy that you would like to share with the community, please follow the instructions contained in https://github.com/galaxyproject/galaxy/blob/dev/CONTRIBUTING.md .

For commits made prior to the switch to git, you can find the former HG commit IDs via git notes. Assuming you have set the [canonical Galaxy GitHub repository](https://github.com/galaxyproject/galaxy/) as the remote `upstream`, it is done like so (if you cloned directly from the canonical source rather than your own !GitHub fork, the remote would be `origin`):

```
#!highlight sh
% git fetch upstream refs/notes/hg2git:refs/notes/hg2git`
% git log --show-notes=hg2git [-1 git-commit-hash]
```


e.g.:

```
#!highlight sh
% cd galaxy.git
% git log --show-notes=hg2git -1 579edf01b1416d52ac99f8675b6f86d677d6ee0e
Author: Carl Eberhard <carlfeberhard@>
Date:   Mon Feb 16 17:51:51 2015 -0500

    UI, History: allow purging, implement purge option in multi-view dropdowns; Managers: fix purgable return value; API, histories: return purged in summary view

Notes (hg2git):
    hg:6ed64b39bcef417a2361e42336c3a46ad10354de
% cd ../galaxy.hg
% hg log -r 6ed64b39bcef417a2361e42336c3a46ad10354de
changeset:   16658:6ed64b39bcef
user:        Carl Eberhard <carlfeberhard@gmail.com>
date:        Mon Feb 16 17:51:51 2015 -0500
summary:     UI, History: allow purging, implement purge option in multi-view dropdowns; Managers: fix purgable return value; API, histories: return purged in summary view
```


### branches

The following branches exist:

* `dev` - primary development, new features, largely untested, should not be run in production
* `master` - always updated to the most recent stable release
* `release_YY.MM` - contains commits of the `YY.MM` stable release

You can get the current "in-development" code (where in-development code is on the `dev` branch) with:

```
#!highlight sh
% git clone https://github.com/galaxyproject/galaxy.git
% cd galaxy
```


And receive updates

```
#!highlight sh
% git checkout dev
% git pull
```


## Bitbucket

We maintain two main repositories for different purposes:

### galaxy-central

[Central](https://bitbucket.org/galaxy/galaxy-central ) was formerly the main development repository. It remains as a buffer for the git to hg conversion process, to ensure that nothing broken is inadvertently pushed to galaxy-dist.

### galaxy-dist
[Dist](https://bitbucket.org/galaxy/galaxy-dist ) is the distribution repository that is updated every release. In between releases, there are critical bugfixes applied to the stable branch.

### Branches

You can get the current stable code with:

```
#!highlight sh
% hg clone https://bitbucket.org/galaxy/galaxy-dist/
% cd galaxy-dist
% hg update stable
```


## How to migrate a Galaxy instance from Mercurial Bitbucket to Git GitHub

1. Backup everything.
1. Find what branch and commit is your Mercurial Galaxy at:
  ```#! highlight sh
  $ hg log -b $(hg branch)
  ```

1. `git clone https://github.com/galaxyproject/galaxy` in a temporary directory.
1. Find the corresponding commit in the cloned Git repository on the corresponding branch (Bitbucket default->!GitHub dev; Bitbucket stable->!GitHub master).
1. Checkout the !GitHub repository at the commit you found in the previous step.
1. Backup your `.hg/` folder.
1. Replace your `.hg/` folder with the `.git/` folder from the new checkout.
1. Your Galaxy should be switched to Git. Unless you have local changes, `git status` should show none.
1. You can now update to the latest Git revision:
  ```#! highlight sh
  $ git pull
  ```
