---
sidebar_position: 2
---

# Specs

The license id on the packaging is GPL-2.0-or-later. The upstream code keeps whatever is in the License tag.

For example:

```
# Copyright (C) 2026 Oreon HQ
# SPDX-License-Identifier: GPL-2.0-or-later
```

`orbscli spec-add` writes those two lines for you. `orbscli lint` fails the spec if somebody deletes them.

```
orbscli spec-add <name> --url https://example.com/name-1.2.3.tar.gz --release <release>
orbscli lint <path>
```

## What the build worker runs

The build worker runs prep, then build, then install, then check, then files.

`Source0` should be the URL of the upstream tarball. Patches and key files stay in the package directory, because those are ours. The tarball stays on the upstream server, so a version bump is a URL change and a new hash.

For example:

```
%global source0_hash <sha256 of that exact file>
```

:::warning

prep checks that hash with `oreon_sha256`. When you change the URL, download the file and hash the bytes you actually got. A hash copied off a web page for a different tarball will fail on the build worker, and it should.

:::

Every spec needs `BuildRequires: oreon-rpm-macros`. That is where `oreon_sha256` and the rest of those helpers come from. The `oreon-rpm-macros` spec itself is the package that provides them, so that one spec does not depend on itself.

:::danger

The build workers set `dist` for the release you are building. Setting it again in the spec fights the build root.

:::

## Dependencies

One BuildRequires per line, and the name has to be something rpm-specfiles builds, or a Provides that spec really has. The pkgconfig cases are on the main [packaging](packaging.md) page, because getting them wrong is how a chain ends up building packages in the wrong order.

:::note

If a macro was never defined in the build root, rpm leaves the text sitting in the file list. install then dies with `File must begin with "/"`. Your laptop having the macro means the laptop has the package. The build root still needs it. BuildRequires the package that defines the macro, compute the path from the python that is installed, or walk the files you just installed and write the list yourself.

:::

`find-debuginfo` runs `gdb-add-index` while packaging debug info. If gdb is missing you get `gdb: command not found`, then `GDB exited with exit status 127`. The fix is `BuildRequires: gdb`.

## Tests and optimizations

:::warning

The test suite, LTO, PGO, and `-Werror` are part of the package. A build that reports success after you deleted the check section, passed `-fno-lto`, turned `LLVM_ENABLE_LTO` off on the build we provide, or demoted `-Werror` to a warning still has the bug. The next thing that links it will hit the same warning with less context.

:::

If the warning is real, fix the function. If gcc 16 with LTO is misreading one helper, change that helper, or stop that one function from being inlined, and leave `-Werror` on for everything else. A package-wide `-Wno-error` will also hide the next bug that is real.

:::tip

When the tests race each other, run them with `-j1`. It is slower but it still runs the tests.

:::

A temporary object you delete before the end of build can skip LTO if you are throwing it away anyway. That is what the llvm instrumented build is. The package that gets installed still has LTO.

:::info

Changelogs should stay short. Add a line when Version or Release changes.

:::

## Read the spec the build worker will see

This is the script after the macros expand. Replace `<dist>` and `<name>.spec` with yours.

```
rpmspec -P --define "dist <dist>" --define "_sourcedir $PWD" <name>.spec | less
```

That is the script after macros expand. If you still see `python3_sitearch` or `buildsubdir` sitting there as text, the build root will see the same text unless some BuildRequires defines it later. A `%global` copied from the debug install script at the top of the spec freezes `buildsubdir` while it is still empty. `%define` waits until the line runs.

```
rpmspec -q --srpm <name>.spec
```

:::warning

`rpmspec -q --srpm` only tells you the spec parsed. It will accept a spec whose build section cannot compile.

:::
