---
sidebar_position: 4
---

# Groups

groups/release lists every package under that release directory. The filename is the release, so the sections inside the file have no separate release field. Putting llvm in the toolchain group leaves the spec at release/llvm.

generate-chain also prints groups, and those mean build these together, then build the next chunk. The files in groups/ are who you ask when a package is on fire. If llvm fails, orbscli groups owner llvm is the question, and the chain string is a different question.

These read the files.

```
orbscli groups list
orbscli groups list <release>
orbscli groups show toolchain
orbscli groups owner llvm
orbscli groups check
```

Edit the files through orbscli so the comma-separated package line stays straight. Pass --release with the release name. Whatever comment you put at the top of the file is left alone.

```
orbscli groups package add sed build-tools
orbscli groups package move llvm toolchain
orbscli groups package remove sed
orbscli groups create math --summary "gmp and friends" --maintainer "Brandon Lester"
orbscli groups delete math
orbscli groups maintainer add toolchain "Brandon Lester"
orbscli groups maintainer remove toolchain "Brandon Lester"
orbscli groups release new <release>
```

package add needs a spec on disk, and it will refuse if that package is already listed somewhere in the same release. Use package move to take it out of the old group. package remove drops it from the file entirely, which makes groups check fail until you add it again. Use that when you are about to delete the package. delete only works once the group has no packages left. maintainer remove will leave the last person on a group. release new creates groups/release when the spec directory for that release already exists and the file does not.

:::note[For example]

```
[toolchain]
summary: compilers, libc, binutils, debuggers. these are slow and they fill disks
maintainer: Brandon Lester
maintainer: Maintainer2
packages: gcc, binutils, glibc, gdb, llvm
```

:::

maintainer is a person. Add another line for a second person, or put both names on one line.

:::note[For example]

maintainer: Brandon Lester, Maintainer2

:::

groups list, groups show, and groups owner print every name on the group. Maintainer2 in the example is a placeholder so you can see the shape. Brandon Lester is the person actually on every group right now, until someone else takes one or gets added beside him. The group file is the list. If you write the owner down somewhere else and forget to change the file, groups owner will keep answering with the old name.

groups check fails if a spec is missing, if a package is listed twice, if a name in the file has no directory, if a group has nobody on it, or if you put a release line in the file that disagrees with the filename. A new spec should be added to exactly one group in that same commit. commit-check counts package directories, so the groups/ edit can ride along with the package commit and it is still one package.

libraries is the big group on purpose, for shared libraries that do not have enough going on to need their own maintainer conversation. Once one of them is failing every chain, or it has grown a pile of patches, move it out into a group of its own.

Oreon 10 desktop packages are oreon-10-desktop inside groups/oreon-10. A new release is a new file, groups/release, with the same name as the spec directory.
