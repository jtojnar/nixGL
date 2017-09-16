This tool tries to solve the "OpenGL" problem on nix.

# Motivation

You use Nix on any distribution, and any GL application installed fails with this error:

```
$ program
libGL error: unable to load driver: i965_dri.so
libGL error: driver pointer missing
libGL error: failed to load driver: i965
libGL error: unable to load driver: i965_dri.so
libGL error: driver pointer missing
libGL error: failed to load driver: i965
libGL error: unable to load driver: swrast_dri.so
libGL error: failed to load driver: swrast
```

This library contains a wrapper which is able to launch GL application:

```
nixGL program
```

# Installation / Usage

Clone this git repository:

```
git clone https://github.com/guibou/nixGL
```

(Optional) installation:

```
cd nixGL
nix-build
nix-env -i ./result
```

Usage:

```
nixGL program args
```

# Limitations

The idea is really simple and should work reliably in most cases.

However there is still two configurations variables hardcoded in the wrapper.

- `ignored`: the list of all nix packages which may contain a wrong `libGL.so`.
- `systemLibs`: the list of where on the host system the `libGL.so` can be found.

Open a bug / pull request if this does not work on your distribution / driver.

It works with `primus`, but there is some artifacts.

## Fundamental issue

If your program libraries depends on different version of the same library, for example, this dependency tree:

```
program
   libFoo-1.2
      libBar-1.4
   libTurtle-1.6
      libBar-1.2
```

One version or the other of `libBar` may be used. In practice this does not happen a lot.