---
sidebar_position: 5
---

# Reading a failure

The chain page saying Failed 9 means nine jobs failed, usually fewer packages than that because x86_64 and aarch64 are separate jobs. What you need from that page is the line that stopped each build.

```
orbscli why <chain>
```

That asks the API for the failed jobs, then the newest attempt of each one, then that attempt's log. It prints the lines that look like a real stop. Two things in these logs look like failures and are noise, and they have eaten a lot of time.

CMake probes the compiler with a pile of tiny test programs. When the flag is missing it logs this and carries on.

:::note[For example]

-- Performing Test C_SUPPORTS_FPIC - Failed

:::

orbscli why drops those probe lines. Configure failed when configure itself exited. The chain page still searches the same log for the word Failed and can list the probes there. Use the API output.

Clang on these builders is handed a few gcc flags it does not understand. You will see this, and then the next object will start compiling.

:::note[For example]

```
clang++: warning: argument unused during compilation: '-specs=/usr/lib/rpm/oreon/oreon-hardened-cc1'
warning: unknown warning option '-Wno-complain-wrong-lang'
1 warning generated.
[1262/6303] Building CXX object ...
```

:::

The 1262/6303 line is the proof it kept going. The llvm spec strips those flags now because one warning per object was filling the log before the real error could show up. If some other spec is passing them to clang, take them out of the flags you hand clang. The failure you are looking for is further down.

## The log gets cut off

The builder stores about the first 5MB of the build log and then a short footer. llvm prints enough per object that the capture dies somewhere around 1200/6303, and whatever actually killed the build is absent from the file you downloaded.

When orbscli why tells you there is no compiler error in the captured log, believe it. You do not have the error yet. Change the spec so successful compiles go to a file, and on failure print the last 200 lines of that file. Build again. Until that shorter log contains error:, you are guessing, and a guessed patch on a 6300-file tree is how you spend a week.

## The compiler already finished

gdb: command not found in install, followed by GDB exited with exit status 127, is find-debuginfo trying to run gdb-add-index. gdb was absent from the build root. BuildRequires gdb.

File must begin with "/" and then python3_sitearch means rpm never expanded the macro, so it tried to package a path that is still that text. python3_sitelib does the same thing. aarch64 sometimes has the macro from another package and x86_64 does not, which is why one arch is a success and the other dies in files.

Could not open the files list debugsourcefiles.list is find-debuginfo writing the list and rpm looking for it in a different directory. If you copied the debug install script with global, buildsubdir was expanded at the top of the spec while it was still empty, so the tool ran in the parent directory. Use define and the path is filled in when install actually runs.

No space left on device, or error opening a .o.d file in the middle of a huge compile, is the disk. llvm's final tree is the usual offender. -gz=zstd on the debug info shrinks the objects and you still have the debug info. The instrumented tree is already deleted once result.profdata has been copied. LTO stays on. Turning it off would ship an llvm we do not want.

No module named pip during pyproject_wheel is a spec that builds a wheel and never asked for pip. BuildRequires python3-pip.

If dnf refuses to start the build because two packages own the same file, one of your BuildRequires pulled a second provider of a library the build root already has. See which two packages own the file and drop the extra dependency. Another version pin on top of that usually makes the solver worse.

## A warning that fails the link

gcc 16 with LTO and -Werror can be quiet for the whole compile and then fail at the link, once a function has been inlined into a test.

:::note[For example]
error: 'path' may be used uninitialized [-Werror=maybe-uninitialized]  
lto1: all warnings being treated as errors
:::

The inlined from notes above that line name the function that matters. Fix that function. If the write is obviously there and gcc still cannot see it through LTO, stop that one function from being inlined. --enable-werror stays. A package-wide -Wno-error will swallow the next one too.

A compile line that merely contains -Werror=format-security is the flag, which has the word error in it. orbscli why tries to keep lines that say error: and skip the flag-only ones.

## Fixing it yourself

1. Run orbscli why and the chain id. Write down the first real error: line, the file, and whether you were in build, install, or check.
2. Open the spec and run rpmspec -P the way the [specs](packaging-specs.md) page shows. The line you think is running has to be the line that printout shows.
3. If the log dies mid-compile with no error:, make the spec quieter and build again. Stop there until you can see the stop.
4. Change the smallest thing that makes that line true. A missing tool is a BuildRequires. A path that is still a macro is a path. A warning on one function is a change to that function.
5. orbscli lint the spec, then orbscli commit-check, and the subject is the package name and what changed.

For llvm, step 3 is the whole job until the captured log actually contains the stop. The cmake Failed list and the unused -specs warnings are the noise that filled the previous log.
