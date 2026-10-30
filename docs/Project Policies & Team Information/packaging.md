# Oreon Packaging Guidelines

Specs are in `oreon-11-rp1/<name>/<name>.spec`. (via rpm-specfiles on our GitHub)

Builds go through Oreon Build Service. Use `orbscli`, or the site at https://build.oreonhq.com.

```
orbscli login -u NAME -p PASSWORD --url https://build.oreonhq.com
orbscli whoami
```

## New package

```
orbscli spec-add NAME --url https://example.com/name-1.2.3.tar.gz
```

`--version` if the tarball name has no version.

spec-add does not guess C library deps. It leaves this line:

```
# !!!ADD DEPS!!! !!!ADD DEPS!!! !!!ADD DEPS!!!
```

Replace it with real `BuildRequires` before `generate-chain`. Remove the comment when done.

Rust, Python, Go, Ruby, Perl, R, and Haskell use `rust2rpm`, `pyp2spec`, `go2rpm`, `gem2rpm`, and the other `*2rpm` tools. spec-add and spec-bump take `BuildRequires` and `Requires` from those. C packages stay on the comment.

One `BuildRequires` per line. Use the package this tree actually builds.

```
BuildRequires: zlib-devel
```

`zlib-devel` matches the `zlib` spec. `pkgconfig(zlib)` matches it only because `zlib.spec` has `Provides: pkgconfig(zlib)`. `pkgconfig(liblzma)` matches `xz` for the same reason. `pkgconfig(xz)` matches nothing we build, so xz is not ordered first.

If the `.pc` name is not the spec name, add `Provides: pkgconfig(the-name)` on the subpackage that provides the file. `%oreon_filelist` is not in the chain index.

A name we do not build is not a chain edge. The build root uses whatever else provides it.

Same upstream version already in the build root at a higher release wins over ours. Pin ours. Use `%{?dist}`.

```
BuildRequires: libXau-devel = 1.0.12-1%{?dist}
```

`-devel` requires the library at the same version.

```
Requires: %{name}%{?_isa} = %{version}-%{release}
```

If `-devel` requires a package this tree does not build, `pkgconfig()`, `cmake()`, and `python3dist()` look uninstallable. The chain picks another provider.

## Chain and builds

```
orbscli generate-chain --release oreon-11-rp1
```

That prints `pkg pkg : pkg`. Packages in one group build together. The next group waits.

```
orbscli chain-build 'pkg pkg : pkg' --release oreon-11-rp1 --side-tag TAG
```

Same chain string can go on https://build.oreonhq.com.

One package:

```
orbscli build NAME --release oreon-11-rp1 --wait --log /tmp/NAME-build
```

`orbscli builds list`, `orbscli builds show ID`, `orbscli builds retry ID`, `orbscli logs job ID`.

## Bump

```
orbscli spec-bump -p NAME
```

On a rust, python, go, ruby, perl, R, or haskell package this also refreshes deps from the `*2rpm` tool.

## Expectations

`BuildRequires: oreon-rpm-macros` on every spec except `oreon-rpm-macros`.

`%global source0_hash` and `%oreon_sha256` in `%prep`. spec-add writes both.

`%oreon_cmake`, `%oreon_meson`, `%oreon_configure`, `%oreon_make`, `%oreon_py_build`, `%oreon_cargo_build`. Not `%cmake` or `%meson`.

Patches and gpg/pgp stuff should be located next to the spec itself in the same dir.

`%dist` is `.or11` on the builders. changing it is not needed.