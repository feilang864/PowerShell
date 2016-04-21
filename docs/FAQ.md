Where can I learn PowerShell's syntax?
======================================

[SS64.com](http://ss64.com/ps/syntax.html) is a good resource.

What are the best practices and style?
======================================

The [PoshCode][] unofficial guide is our reference.

[PoshCode]: https://github.com/PoshCode/PowerShellPracticeAndStyle

What are PowerShell's scoping rules?
====================================

- Variables are created in your current scope unless explicitly indicated.
- Variables are visible in a child scope unless explicitly indicated.
- Variables created in a child scope are not visible to a parent unless
  explicitly indicated.
- Variables may be placed explicitly in a scope.

Things that create a scope:
---------------------------

- [functions](http://ss64.com/ps/syntax-functions.html)
- [call operator](http://ss64.com/ps/call.html) (`& { }`)
- [script invocations](http://ss64.com/ps/syntax-run.html)

Things that operate in the current scope:
-----------------------------------------

- [source operator](http://ss64.com/ps/source.html) (`. { }`)
- [statements](http://ss64.com/ps/statements.html) (`if .. else`, `for`, `switch`, etc.)

Why didn't an error throw an exception?
=======================================

Error handling in PowerShell is a bit weird, as not all errors result in
catchable exceptions by default. Setting `$ErrorActionPreference = 'Stop'` will
likely do what you want; that is, cause non-terminating errors to instead
terminate. Read [An Introduction To Error Handling in PowerShell][error] for
more information.

[error]: https://blogs.msdn.microsoft.com/kebab/2013/06/09/an-introduction-to-error-handling-in-powershell/

Why did `Start-PSBuild` tell me to update `dotnet`?
===================================================

We depend on the latest version of the .NET CLI, as we use the output of `dotnet
--info` to determine the current runtime identifier. Without this information,
our build function can't know where `dotnet` is going to place the build
artifacts.

You can automatically install this using `Start-PSBootstrap`.

**However, you must first manually uninstall other versions of the CLI.**

If you have installed via any of the following means:

- MSI
- `exe`
- `apt-get`
- `pkg`

You *must* manually uninstall it.

Additionally, if you've just unzipped their binary drops (or used their obtain
scripts, which do essentially the same thing), you must manually delete the
folder, as the .NET CLI team re-engineered how their binaries are setup, such
that new packages' binaries get stomped on by old packages' binaries.

Why is my submodule empty?
==========================

If a submodule (such as `src/Modules/Pester`) is empty, that means it is
uninitialized. If you've already cloned, you can do this with:

```sh
git submodule init
git submodule update
```

You can verify that the submodules were initialized properly with:

```sh
git submodule status
```

If they're initialized, it will look like this:

```
 f23641488f8d7bf8630ca3496e61562aa3a64009 src/Modules/Pester (f23641488)
 c99458533a9b4c743ed51537e25989ea55944908 src/libpsl-native/test/googletest (release-1.7.0)
 e6bf85694ae8352d77175c4c7d304946e018808c src/windows-build (monad/cc6afbeb-3/31)
```

If they're not, there will be minuses in front (and the folders will
be empty):

```
-f23641488f8d7bf8630ca3496e61562aa3a64009 src/Modules/Pester (f23641488)
-c99458533a9b4c743ed51537e25989ea55944908 src/libpsl-native/test/googletest (release-1.7.0)
-e6bf85694ae8352d77175c4c7d304946e018808c src/windows-build (monad/cc6afbeb-3/31)
```

Please note that the commit hashes for the submodules have likely changed since
this FAQ was written.

Why does my submodule say "HEAD detached at" some commit?
=========================================================

When a submodule is first initialized and updated, it is not checked out to a
branch, but the very exact commit that the superproject (this PowerShell
repository) has recorded for the submodule. This is the intended behavior.

If you want to check out an actual branch, just do so with `git checkout
<branch>`. A submodule is just a Git repository; it just happens to be nested
inside another repository.

Please read the Git Book chapter on [submodules][].

[submodules]: https://git-scm.com/book/en/v2/Git-Tools-Submodules