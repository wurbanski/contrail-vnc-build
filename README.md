# contrail-vnc-build

## Purpose

This is a PoC of tracking and tagging code used for nightly builds/releases.

This repository contains archived manifests for each of the nightly builds. Manifests were downlaoded from the [log server](http://logs.opencontrail.org/periodic-nightly/review.opencontrail.org/).

Branches correspond to product's release lines (`master` and `R5.0`).

## Problem and solution

For a complex product using [git-repo](https://gerrit.googlesource.com/git-repo/ ), it is difficult to track code used for specific build. Typical manifest from [contrail-vnc](https://github.com/Juniper/contrail-vnc/blob/master/default.xml) contains references to the tops of specific branches (in this case `master` or `R5.0`), which move, so it isn't a good way to track the code. 

CI system prepares a manifest directing to specific revisions in all of the repositories used.  This way, it is possible to initialize a sandbox with exactly the same code as used in a nightly build.

This repository contains a part of the history of builds and allows to sync the sandbox to specific version of the code.

## Branching and tagging scheme

1. Each release branch corresponds with a branch in this repository.
2. Each nightly build is represented by a commit in specific branch.
3. Each commit contains a manifest with snapshot of all the code used in the build.
4. Each commit is tagged with `<branch>-<build_number>`.
5. If a specific build becomes release build, selected commit should be tagged with correct label, e.g. `v1.2.3` on tag `master-150`.

## Usage in sandbox

Just use it while initializing the repository, e.g. `repo init -u https://github.com/wurbanski/contrail-vnc-build -b refs/tags/master-150; repo sync`.

## Usage in CI

Currently, [Zuul](http://zuulv3.opencontrail.org) snapshots the commits and saves the manifest. In future, zuul could use this repository to schedule a nightly build whenever new commit gets to the repository. This should be automated separately, but allows for a good control of both contents and timing of the builds.
