# Source Management
This document discusses details of how to manage source code stored in Git. A visual diagram of this process can be found [here](./SourceManagement.pdf).

## Table Of Contents 
* [Quick Start](#user-content-)
* [Prerequisites](#user-content-)
* Commands
	* [Clone](#user-content-clone)
	* [Create Local Branch](#user-content-create-local-branch)
	* [Set Local Branch To Track Origin](#user-content-set-local-branch-to-track-origin)
	* [Add New Files To Branch](#user-content-add-new-files-to-branch)
	* [Commit Files To Branch](#user-content-commit-files-to-branch)
	* [Push Local Changes To Origin](#user-content-push-local-changes-to-origin)
	* [Merge Master Into Branch](#user-content-merge-master-into-branch)
		* [Resolve Merge Confilicts](#user-content-resolve-merge-confilicts)
	* [Squash-Push](#user-content-squash-push)
	* [Create PR](#user-content-creat-pr)
	* [Request Code Review](#user-content-request-code-review)  

## Quick Start
Assuming that you have already [cloned](#user-content-clone) your target repository, the general source code management process is as follows:

1. [Pull origin/master](#user-content-pull-origin-changes-to-local)
2. [Branch master](#user-content-create-local-branch)
3. [Set origin/\<branch\>](#user-content-set-local-branch-to-track-origin)
4. Develop
	* [Add](#user-content-Add New Files To Branch)
	* [Commit](#user-content-Commit Files To Branch)
	* [Push](#user-content-Push Local Changes To Origin)
	* [Merge](#user-content-Merge Master Into Branch)
* [Squash-Push](#user-content-squash-push)
* [Create PR](#user-content-create-pr)
* [Request Code Review](#user-content-request-code-review)

## Prerequisites
This process assumes basic knowledge of Git and the following concepts

* **origin**: The source code repository location that resides on a remote server.
* **local**: The source code repository location that resides on your local machine. Wenever origin is not specified local will be assumed.


## Commands
The following are a list of common Git commands that can be run from a *nix or Cygwin terminal. Note that these commands contain \<VARIABLES\> where you are expected to supply specific values for your use case. Unless otherwise specified, these commands assume that you are running them from the root of the given local source code location.

### Clone
Before starting development on any project you will need to clone origin/master to your local. This command should be run from a location that will be "one level up" from the target local repository location.

```sh
cd <PATH> && \
git clone <YOUR_REPO@TARGET_LOCATION.git>

```

### Create Local Branch

```sh
git checkout -b <BRANCH_NAME>

```

### Set Local Branch To Track Origin

```sh
git push --set-upstream origin <BRANCH_NAME>

```

### Add New Files To Branch

```sh
git add <FILE>

```

### Commit Files To Branch

```sh
git commit -m'<MESSAGE>'

```

### Push Local Changes To Origin

```sh
git push

```

### Pull Origin Changes To Local

```sh
git pull

```

### Merge Master Into Branch
You will want to keep up with the changes that will be being pushed into master so that your feature branch does not fall too far out of date with the latest code base. By keeping up with the source of truth you will be preventing your having to deal with extensive merge conflicts in the future.

```sh
git checkout master && \
git pull && \
git checkout <BRANCH> && \
git pull && \
git merge master

```

#### Resolve Merge Confilicts
On occasion you will have merge confilicts due to parallel development that makes it into `master` before your changes. You will need to resolve these conflicts before your merge will complete. You can resolve this issues using any mergetool that you like. One suggestion is `opendiff`.

```sh
git mergetool --tool <opendiff>

```

### Squash-Push
You should squash your branch before requesting a PR to keep the `master` history clean.

```sh
git checkout <YOUR_BRANCH> && \                        
git reset $(git merge-base origin/master <YOUR_BRANCH>) && \
git add -A && \                                           
git commit -m '<YOUR_MESSAGE>' && \
git push -f

```

### Create PR
Information on creating a PR in GitHub can be found [here](https://help.github.com/articles/about-pull-requests/)

### Request Code Review
Information on requesting a code review can be found [here](https://github.com/blog/2291-introducing-review-requests)