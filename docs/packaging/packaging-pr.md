---
sidebar_position: 6
---

# Pull requests

ohqcibot watches PRs on [rpm-specfiles](https://github.com/oreonhq/rpm-specfiles). Write, maintain, or admin on the repo is enough to run its commands. Other people can comment, but the bot will refuse them.

## Commit check

On every open and sync, ohqcibot posts check `ohqcibot/commit-format`. Only commits that touch a `.spec` need a colon in the subject. Same rule as [Commits](packaging-commits.md). Docs-only or groups-only commits are left alone.

## Chain from a PR

Do not ask the bot to invent the chain. Run generate-chain locally for the release you care about, then paste that string into the PR.

```
orbscli generate-chain --release oreon-11-rp1
```

The last line is the chain. Comment it with the release name first.

```
/chainbuild oreon-11-rp1 patch : xz : cmake-srpm-macros : libarchive : ...
```

Both pieces are required. Bare `/chainbuild` and a chain with no release are rejected. The release has to be an `oreon-*` folder this PR actually touches. If the PR changed both oreon-10 and oreon-11-rp1, pick one and run a second command for the other.

The bot creates side tag `ohqcibot/pr-<n>` once per PR (or reuses it on later `/chainbuild`s), triggers the chain on that tag, and leaves a kickoff comment with the chain link. While the chain runs it edits one status comment instead of spamming new ones. When packages fail, dig in with [Reading a failure](packaging-failures.md) the same way you would for a local chain-build.

## Resume

If a chain stopped and you fixed whatever blocked it, resume from the PR.

```
/resume <chain>
```

## Move a fork PR

Someone opened the PR from their fork and you need a branch on the main repo so other people can push fixes. **Run this before building please.**

```
/move
```

That only works when the PR head is a fork. It creates (or updates) branch named after the PR number, opens a new PR from that branch onto main, and closes the fork PR.

## Merge

When the chain looks good and the PR is ready.

```
/merge
/m
/merge testing
/merge dev
```

That promotes the PR's side tag into `stable` by default (or testing / dev if you said so), then rebase-merges the PR onto `main`. Same Oreon promote you would do by hand, then the GitHub merge.

## What the bot does not do

It does not run generate-chain for you. Local orbscli is still the planner. It does not build every release the PR touched unless you ask for each one. It does not push main. `/merge` is rebase only.
