---
sidebar_position: 1
---

# Packaging

:::warning

Please note: `orbscli` is still in early production and may have bugs. Report them if you find any, or fix it yourself and open a PR.

:::

A package in [this repo](https://github.com/oreonhq/rpm-specfiles) is one directory under a release, like release/sed/sed.spec. Patches, signatures, and config snippets sit next to that spec. The tarball comes from upstream at build time. Source0 is that URL, and source0_hash with oreon_sha256 checks that the bytes you downloaded are the bytes the spec was written against.

A new maintainer gets the tree with orbscli setup, which clones https://github.com/oreonhq/rpm-specfiles. Pass --dest and a path if you want it somewhere else. If that checkout is already on disk, the command prints the path it found.

Login with the commands below. You will get logged out after a while for security, so log back in.

```
orbscli login -u <name> --url https://build.oreonhq.com
orbscli whoami
```

The password is typed at the prompt and is not echoed, and it is not a command argument. orbscli keeps the access token in the credentials file and drops the password. A script can pipe it with --password-stdin, or skip the password entirely with --token-stdin if you already have a token.

The rest of this section is the stuff you end up needing once a spec is wrong. [Specs](packaging-specs.md) is how a spec is supposed to be written, [Commits](packaging-commits.md) is the one-package commit rule, [Groups](packaging-groups.md) is who owns which packages, and [Reading a failure](packaging-failures.md) is what to do with a failed chain, including an example with a complex package.

## New package

spec-add starts from the upstream tarball URL.

```
orbscli spec-add <name> --url https://example.com/name-1.2.3.tar.gz
```

If the tarball name has no version in it, pass --version yourself.

spec-add can fill BuildRequires and Requires for Rust, Python, Go, Ruby, Perl, R, and Haskell, because those come out of rust2rpm, pyp2spec, go2rpm, gem2rpm, and the other 2rpm tools. A C library has no tool like that, so spec-add drops this comment in the spec and you replace it with real dependencies before you run generate-chain.

:::note[For example]

```
# !!!ADD DEPS!!! !!!ADD DEPS!!! !!!ADD DEPS!!!
```

:::

One BuildRequires per line, and the name has to be something this tree actually builds.

:::note[For example]

BuildRequires: zlib-devel

:::

zlib-devel is the subpackage from zlib.spec, so the chain knows to build zlib first. pkgconfig(zlib) works for the same reason, because that spec has Provides pkgconfig(zlib). pkgconfig(liblzma) works because xz.spec provides it. pkgconfig(xz) matches nothing we build, so the chain will leave xz where it is and the build root's copy is what you get.

If the .pc name and the spec name disagree, add Provides pkgconfig(the-name) on the subpackage that ships the file. generate-chain only draws edges between packages this tree builds. Everything else is already supposed to be in the build root.

A devel subpackage has to require the library package at the same version, or you can install headers that disagree with the .so you linked.

:::note[For example]

```
Requires: %{name}%{?_isa} = %{version}-%{release}
```

:::

## Chain

generate-chain prints the build order for that release.

```
orbscli generate-chain --release <release>
```

You get a string like pkg pkg : pkg. Packages written together in one group build at the same time, and the group after the colon waits until they are published.

```
orbscli chain-build 'pkg pkg : pkg' --release <release> --side-tag <tag>
```

You can paste that same string into https://build.oreonhq.com if you prefer a web GUI.

For a single package, this is enough.

```
orbscli build <name> --release <release> --wait --log /tmp/<name>-build
```

After it is submitted, builds list, builds show, builds retry, and logs job are how you poke at a job. Pass the id those commands print.

When a chain job has failed, run this before you start scrolling a multi-megabyte log.

```
orbscli why <chain>
```

## Bump

spec-bump takes one package name, or several.

```
orbscli spec-bump -p <name>
```

You can pass several names at once. For rust, python, go, ruby, perl, R, and haskell it also refreshes the dependency lists from the 2rpm tool, same as spec-add.

After the files are written, orbscli prints a commit subject for every package that actually changed.

:::note[For example]
sed: update to 4.10  
grep: update to 3.12
:::

y creates one commit per package. N leaves the files dirty and remembers the subjects. orbscli commit prints that same list and asks again. orbscli commit --yes commits it without the question.

A spec you edited by hand is the same command with a subject. It commits that one package, patches included, and leaves every other dirty package alone.

```
orbscli commit -m "<name>: what changed"
```

push sends the branch. The second line is the same command with the flags written out.

```
orbscli push
orbscli push --branch <branch> --remote origin --user <github_username>
```

From main or master, orbscli push makes a branch named github_username_epoch and pushes that. If you are already on some other branch, it pushes the one you are on. It will never push main.

When the work is on the remote and you want your checkout back on an updated main, orbscli sync fetches, checks out the remote's main branch, and resets your local main to match it. The branch you were on is still in the repo. A dirty tree stops the command, and so does a local main that has commits the remote does not.

## Before you push

```
orbscli lint <path>
orbscli groups check
orbscli commit-check
orbscli commit-check --range origin/main..HEAD
```

lint looks for the copyright header, a Name that matches the filename, and source URLs that still point at a lookaside cache. groups check complains if a spec is missing from groups/ or shows up twice. commit-check complains if one commit touches two packages, or if the subject is missing the package name and what changed.
