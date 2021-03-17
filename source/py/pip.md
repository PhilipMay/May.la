# PIP

## Install Packages
- install GIT development code: `pip install git+<https_git_clone_link>`
- install GIT development code from branch: `pip install git+<https_git_clone_link>@<branch_name>`
- install editable local Projects:
  - `pip install -e .`
  - also see <https://pip.pypa.io/en/stable/reference/pip_install/#editable-installs>

## List Packages
- list outdated packages: `pip list -o`
- list packages in requirements.txt format: `pip list --format freeze`

## Other Commands
- delete package cache: `pip cache purge`

## Install and update Packages from a File
For pip you can create so called [requirements
files](https://pip.pypa.io/en/stable/user_guide/#requirements-files).
These files just list one package per line. Packages from this file can
be installes with `pip install -r <requirements.txt>` and updatet
with `pip install -r <requirements.txt> -U`. The update only makes
sence when you do not specify a version number with the package.

Since pip does not support an “update all” mechanism this is a good way
to install and update the needed packages.

To add a package from GIT just add `git+<https_git_clone_link>` instead of the normal package name.
