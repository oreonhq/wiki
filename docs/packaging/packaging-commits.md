---
sidebar_position: 3
---

# Commits

If a commit touches a package, it touches one package, and the subject says which one and what you did.

For example:

```
sed: update to 4.10
llvm: drop gcc spec flags clang ignores
ngtcp2: keep werror, fix the addr copy warning
```

:::note

The name is the directory. `release/python3.15` is `python3.15` in the subject even when one of its subpackages is called `python3-libs`. A subject that starts with `python3-libs` will miss the spec `commit-check` is looking at.

:::

The spec, its patches, and a signature for that same package go in the same commit. That is one package with its files. The same one-line fix copied into sed and grep is two commits, because the next person reading `git log` on `release/sed` should see sed by itself.

## Check before you push

`commit-check` is how you see that before it is pushed.

```
orbscli commit-check
orbscli commit-check --cached
orbscli commit-check --range origin/main..HEAD
```

With a dirty checkout, the first one looks at what you have not committed yet. With a clean checkout it looks at HEAD. `--cached` is the index. `--range` walks every commit in something like `origin/main..HEAD`, which is the one to run before you push a branch.

:::tip

Groups can have a normal sentence as the subject, since none of those are a package. The moment the commit also contains a spec, or a patch next to a spec, the subject has to be the package name and what changed, and the other package directories have to be in other commits.

:::

## Bump commits and manual fixes

`orbscli spec-bump` writes these subjects for you, name and update to the version, lists every package it changed, and asks once whether to commit the batch. Saying no keeps the list. `orbscli commit` asks again, and `orbscli push` sends the branch up. From main that push starts on `github_username_epoch`. `orbscli sync` puts the checkout back on the remote's main when you are done.

```
orbscli spec-bump -p <name> --release <release>
orbscli commit
orbscli commit --yes
orbscli push
orbscli sync
```

A fix you made yourself uses the same command. The subject is the package and what changed, and only that package directory is committed. Other dirty packages stay dirty.

```
orbscli commit -m "<name>: what changed"
orbscli commit -p <name> -m "what changed"
orbscli commit --yes -m "<name>: what changed"
```

`fast-fix` asks the same question after it writes a spec. The subject is short, like the package name and package installed files. `--yes` commits that without asking. `--dry-run` only reports what would change.

```
orbscli fast-fix --chain <chain> --release <release>
orbscli fast-fix --chain <chain> --release <release> --yes
orbscli fast-fix --chain <chain> --release <release> --dry-run
```

:::info

`fast-fix` only applies obvious repairs. Dependency chain problems and `%prep` patch or checksum failures will need manual fixing.

:::
