# venv-scripts

These are a couple shell scripts related to Python virtual environments. They
may or may not be of any use to anybody but me.

These scripts are not universal. You may have to modify them to run on your
system, especially if you&rsquo;re not running a recent version of Debian or
Ubuntu.

## License

This repository is licensed under [0BSD](https://spdx.org/licenses/0BSD.html).
See [**`LICENSE`**](LICENSE).

## Installation

You can install whichever of these scripts you like by copying them to a
directory in your path. I suggest your per-user bin directory, `~/bin`.

You probably don&rsquo;t want to install [the `check` script](#check), since
its only purpose is to find bugs in these other scripts, and it looks for them
in the current directory.

## The Scripts

### [`check`](check)

Runs [`shellcheck`](https://github.com/koalaman/shellcheck) on these scripts.

This actually runs `shellcheck` on executables under the current directory. So
run it from the top-level of the working tree (where it can be run as
`./check`), if you run it.

### [`delete-all-virtualenvs`](delete-all-virtualenvs)

This deletes all of the current user&rsquo;s Python virtual environments. You
should only ever use this if *all* your venvs are ephemeral&mdash;that is, you
automatically generated them and you can do so again easily.

**This script assumes your virtualenvs are located in the usual places on
recent Debian and Ubuntu systems.** So you may have to modify it for your
system.

This is mainly for troubleshooting, rather than for running regularly.

### [`pipenv3.10`](pipenv3.10)

This runs the system-provided `pipenv`, which it assumes is located in
`/usr/bin`, telling it to use `python3.10` as the interpeter, even if it cannot
identify that command as an interpreter for the version of Python in the
`Pipfile`.

The use case is when the project does use Python 3.10, the `pipfile` specifies
it, and `python3.10` is in `$PATH`, but `pipenv` does not find the installed
`python3.10` as a Python 3.10 interpeter.

**You should only use this when you need to *create* the virtualenv.** That is,
usually you will just use `pipenv`. But if the virtualenv doesn&rsquo;t yet
exist, you may need to tell `pipenv` where it is. (I found the need to do this
repeatedly.)

If you want to tell `pipenv` to use Python 3.10, in general, just use
`pipenv --python 3.10`. This script is different&mdash;it&rsquo;s for when
`pipenv` can&rsquo;t find the interpter, so it tells it explicitly to use
the executable called `python3.10`.

If you use this in place of `pipenv` when the virtualenv already exists,
`pipenv` usually replaces it (or refuses to do anything if you tell it not to).
That causes all packages to need to be reinstalled (including C modules being
rebuilt). So unless you want to initially create, or replace, the venv, just
use `pipenv` instead of this script.

See [Pipenv: Python Dev Workflow for Humans](https://pipenv.pypa.io/en/latest/)
for more information on `pipenv` itself.

I am *not* in any way affiliated with the `pipenv` project itself.

Please note that if you&rsquo;re installing packages for Python 3.10 on a
system that didn&rsquo;t come with it (which is the point of ths script), then
you&rsquo;ll need the header files for that version of Python, so that modules
written in C can be compiled to work with Python 3.10. On a Debian or Ubuntu
system, the `python3.10-dev` package provides those headers.
