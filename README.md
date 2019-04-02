GenericMakefile
===============

A generic makefile for use with C projects. Allows for easy project setup without the need to create tedious build rules or
dependency lists. Release builds produce size-optimized binaries.

## Features
* Automatically finds and compiles all source files within the source directory.
* Compiles most recently modified files first, to hopefully find problems earlier.
* Automatically generates dependencies as files are compiled, ensuring that files are correctly recompiled when dependencies have updated.
* Includes configurations for normal (release) build and debug build suitable for GDB debugging.
* Times the compilation of each file and the entire build.
* Generates version numbers based on git tags (see below), which are passed the compiler as preprocessor macros (in debug mode).
* By default, builds in a "quiet" mode that only lists the actions being performed. By passing V=true to make, you can compile in verbose mode to see the full compiler commands being issued.
* 8 default targets:
    * 64-bit, dynamic-linked, release
    * 64-bit, dynamic-linked, debug
    * 64-bit, static-linked, release
    * 64-bit, static-linked, debug
    * 32-bit, dynamic-linked, release
    * 32-bit, dynamic-linked, debug
    * 32-bit, static-linked, release
    * 32-bit, static-linked, debug
* Customizable compiler/linker flags for each of the build targets.  Sensible defaults baked in optimizing for size.
* Post-build step to strip symbols from release binaries.  Also available as `strip` target.
* Incorporates calls to `cppcheck` as a build prerequisite.  Also available as `check` target.
* Optional `indent` target for style/whitespace conformity.  Defaults to K&R, but with spaces instead of tabs.

### Versioning
Tags should be made in the format `vMAJOR.MINOR.PATCH[-description]`, where `MAJOR`, `MINOR`, and `PATCH` are numeric. The `v` prefix is optional. The following macros will be generated and passed to the preprocessor:
* `VERSION_MAJOR (int)` - The major version number from the most recent tag.
* `VERSION_MINOR (int)` - The minor version number from the most recent tag.
* `VERSION_PATCH (int)` - The patch version number from the most recent tag.
* `VERSION_REVISION (int)` - The number of commits since the most recent tag.
* `VERSION_HASH (string)` - The SHA of the current commit. Includes the "-dirty" suffix if there are uncommitted changes.

If the makefile is not used in a git repository, or is in a repository with no tags, the version macros are not created.

## Limitations
* Assumes GNU make.
* Timing does not work correctly with MacOS. See [issue #6](https://github.com/mbcrawfo/GenericMakefile/issues/6) for a workaround patch that allows timing in seconds on Mac.
* Doesn't really support multiple types of source files in the same project.
* No easy way to exclude files from the build. You can either change the
  extension of files to be excluded, or use preprocessor flags for
  conditional compilation.

## Thanks to
* My friend Jay and people on Stack Overflow for help with regex to parse the version info.
* The residents of [/r/programming](https://www.reddit.com/r/programming/) for suggesting numerous tweaks and improvements.
