0.15.1.4

- Improved error reporting during venv metadata inspection.
- [bugfix] Fixed incompatibility with pypy as venv interpreter (#372).
- [bugfix] Replaced implicit dependency on setuptools with an explicit dependency on packaging (#339).
- [refactor] Moved all commands to separate files within the commands module (#255).

0.15.1.3

- [bugfix] On Windows, pipx now lists correct Windows apps (#217)
- [bugfix] Fixed a `pipx install` bug causing incorrect python binary to be used when using the optional --python argument in certain situations, such as running pipx from a Framework python on macOS and specifying a non-Framework python.

0.15.1.2

- [bugfix] Fix recursive search of dependencies' apps so no apps are missed.
- `upgrade-all` now skips editable packages, because pip disallows upgrading editable packages.

0.15.1.1

- [bugfix] fix regression that caused installing with --editable flag to fail package name determination.

0.15.1.0

- Add Python 3.8 to PyPI classifier and travis test matrix
- [feature] auto-upgrade shared libraries, including pip, if older than one month. Hide all pip warnings that a new version is available. (#264)
- [bugfix] pass pip arguments to pip when determining package name (#320)

0.15.0.0

Upgrade instructions: When upgrading to 0.15.0.0 or above from a pre-0.15.0.0 version, you must re-install all packages to take advantage of the new persistent pipx metadata files introduced in this release. These metadata files store pip specification values, injected packages, any custom pip arguments, and more in each main package's venv. You can do this by running `pipx reinstall-all` or `pipx uninstall-all`, then reinstalling manually.

- `install` now has no `--spec` option. You may specify any valid pip specification for `install`'s main argument.
- `inject` will now accept pip specifications for dependency arguments
- Metadata is now stored for each application installed, including install options like `--spec`, and injected packages. This information allows upgrade, upgrade-all and reinstall-all to work properly even with non-pypi installed packages. (#222)
- `upgrade` options `--spec` and `--include-deps` were removed. Pipx now uses the original options used to install each application instead. (#222)
- `upgrade-all` options `--include-deps`, `--system-site-packages`, `--index-url`, `--editable`, and `--pip-args` were removed. Pipx now uses the original options used to install each application instead. (#222)
- `reinstall-all` options `--include-deps`, `--system-site-packages`, `--index-url`, `--editable`, and `--pip-args` were removed. Pipx now uses the original options used to install each application instead. (#222)
- Handle missing interpreters more gracefully (#146)
- Change `reinstall-all` to use system python by default for apps. Now use `--python` option to specify a different python version.
- Remove the PYTHONPATH environment variable when executing any command to prevent conflicts between pipx dependencies and package dependencies when pipx is installed via homebrew. Homebrew can use pythonpath manipulation instead of virtual environments. (#233)
- Add printed summary after successful call to `pipx inject`
- Support associating apps with Python 3.5
- Improvements to animation status text
- Make `--python` argument in `reinstall-all` command optional
- Use threads on OS's without support for semaphores
- Stricter parsing when passing `--` argument as delimeter

0.14.0.0

- Speed up operations by using shared venv for `pip`, `setuptools`, and `wheel`. You can see more detail in the 'how pipx works' section of the documentation. (#164, @pfmoore)
- Breaking change: for the `inject` command, change `--include-binaries` to `--include-apps`
- Change all terminology from `binary` to `app` or `application`
- Improve argument parsing for `pipx run` and `pipx runpip`
- If `--force` is passed, remove existing files in PIPX_BIN_DIR
- Move animation to start of line, hide cursor when animating

0.13.2.3

- Fix regression when installing a package that doesn't have any entry points

0.13.2.2

- Remove unneccesary and sometimes incorrect check after `pipx inject` (#195)
- Make status text/animation reliably disappear before continuing
- Update animation symbols

0.13.2.1

- Remove virtual environment if installation did not complete. For example, if it was interrupted by ctrl+c or if an exception occurred for any reason. (#193)

0.13.2.0

- Add shell autocompletions. Also add `pipx completions` command to print instructions on how to add pipx completions to your shell.
- Un-deprecate `ensurepath`. Use `userpath` internally instead of instructing users to run the `userpath` cli command.
- Improve detection of PIPX_BIN_DIR not being on PATH
- Improve error message when an existing symlink exists in PIPX_BIN_DIR and points to the wrong location
- Improve handling of unexpected files in PIPX_HOME (@uranusjr)
- swap out of order logic in order to correctly recommend --include-deps (@joshuarli)
- [dev] Migrate from tox to nox

0.13.1.1

- Do not raise bare exception if no binaries found (#150)
- Update pipsi migration script

0.13.1.0

- Deprecate `ensurepath` command. Use `userpath append ~/.local/bin`
- Support redirects and proxies when downloading python files (i.e. `pipx run http://url/file.py`)
- Use tox for document generation and CI testing (CI tests are now functional rather than static tests on style and formatting!)
- Use mkdocs for documentation
- Change default cache duration for `pipx run` from 2 to 14 days

0.13.0.1

- Fix upgrade-all and reinstall-all regression

0.13.0.0

- Add `runpip` command to run arbitrary pip commands in pipx-managed virtual environments
- Do not raise error when running `pipx install PACKAGE` and the package has already been installed by pipx (#125). This is the cause of the major version change from 0.12 to 0.13.
- Add `--skip` argument to `upgrade-all` and `reinstall-all` commands, to let the user skip particular packages

0.12.3.3

- Update logic in determining a package's binaries during installation. This removes spurious binaries from the installation. (#104)
- Improve compatibility with Debian distributions by using `shutil.which` instead of `distutils.spawn.find_executable` (#102)

0.12.3.2

- Fix infinite recursion error when installing package such as `cloudtoken==0.1.84` (#103)
- Fix windows type errors (#96, #98)

0.12.3.1

- Fix "WindowsPath is not iterable" bug

0.12.3.0

- Add `--include-deps` argument to include binaries of dependent packages when installing with pipx. This improves compatibility with packages that depend on other installed packages, such as `jupyter`.
- Speed up `pipx list` output (by running multiple processes in parallel) and by collecting all metadata in a single subprocess call
- More aggressive cache directory removal when `--no-cache` is passed to `pipx run`
- [dev] Move inline text passed to subprocess calls to their own files to enable autoformating, linting, unit testing

0.12.2.0

- Add support for PEP 582's `__pypackages__` (experimental). `pipx run BINARY` will first search in `__pypackages__` for binary, then fallback to installing from PyPI. `pipx run --pypackages BINARY` will raise an error if the binary is not found in `__pypackages__`.
- Fix regression when installing with `--editable` flag (#93)
- [dev] improve unit tests

0.12.1.0

- Cache and reuse temporary Virtual Environments created with `pipx run` (#61)
- Update binary discovery logic to find "scripts" like awscli (#91)
- Forward `--pip-args` to the pip upgrade command (previously the args were forwarded to install/upgrade commands for packages) (#77)
- When using environment variable PIPX_HOME, Virtual Environments will now be created at `$PIPX_HOME/venvs` rather than at `$PIPX_HOME`.
- [dev] refactor into multiple files, add more unit tests

0.12.0.4

- Fix parsing bug in pipx run

0.12.0.3

- list python2 as supported language so that pip installs with python2 will no longer install the pipx on PyPI from the original pipx owner. Running pipx with python2 will fail, but at least it will not be as confusing as running the pipx package from the original owner.

0.12.0.2

- forward arguments to run command correctly #90

0.12.0.1

- stop using unverified context #89

0.12.0.0

- Change installation instructions to use `pipx` PyPI name
- Add `ensurepath` command

0.11.0.2

- add version argument parsing back in (fixes regression)

0.11.0.1

- add version check, command check, fix printed version update installation instructions

0.11.0.0

- Replace `pipx BINARY` with `pipx run BINARY` to run a binary in an ephemeral environment. This is a breaking API change so the major version has been incremented. (Issue #69)
- upgrade pip when upgrading packages (Issue #72)
- support --system-site-packages flag (Issue #64)

0.10.4.1

- Fix version printed when `pipx --version` is run

0.10.4.0

- Add --index-url, --editable, and --pip-args flags
- Updated README with pipsi migration instructions

0.10.3.0

- Display python version in list
- Do not reinstall package if already installed (added `--force` flag to override)
- When upgrading all packages, print message only when package is updated
- Avoid accidental execution of pipx.**main**
