# Contributing

Welcome! Thanks for considering contributing to QuCumber, it's people like
you that make Open Source awesome!

Please take the time to read the following guidelines, in order to streamline
the contribution process.
Also note that this project is released with a Contributor Code of Conduct,
which can be found in this repository's [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)
file. By participating in this project you agree to abide by its terms.

## Bug Reports

First, you should search the current issues to check if someone else has
run into a similar issue, and chime in there with info about your case.
If you believe the bug to be new, open up an issue with a descriptive title
starting with the `[bug]` tag and some system information (eg. OS,
Python/PyTorch version, CUDA version if applicable, etc.) as well as an example
code snippet that reproduces the error. The example code is very important
since, if we can't reproduce the error, we can't debug it, and we won't be able
to fix it.

## Feature Requests

If you'd like to request a feature, open up an issue with a `[feature-request]`
tag in the title, and some details about why it may be useful.

## Contributing Code

If you want to work on an existing issue, make sure nobody is currently assigned
to it, and comment on the issue to let us know that you'd like to work on it.

If you'd like to implement a new feature yourself, open up a Feature Request
first and mention that you'd like to work on it. This is to avoid duplicate
work being done, as PIQuIL team members may be working on features locally
that they haven't put online. In addition, the feature itself may be outside
the intended scope of this project, and therefore may be rejected.

Before you start writing your code, there are a few things you need to set up:

### Fork and Branch

Fork the QuCumber repo on GitHub and create a branch from the `develop` branch
on your fork. Be sure to give it a descriptive name (ideally referencing
the relevant issue number). For example, after cloning your fork to a local
directory, you could create a branch to solve an issue with the documentation
of say, the `PositiveWavefunction` class:

```bash
git checkout -b 42-fix-positive-wavefunction-documentation
```

where issue #42 is the issue you're working on.

### Setting Up Your Development Environment

We recommend developing under at least Python 3.6, in order to fully utilise
available development tools (like [`black`'s](https://github.com/ambv/black)).
In most cases it should be sufficient to run the following:

```bash
pip install -e .[dev]
```

However, if you get an error with PyTorch not being found (which may happen
if you're on Windows) go to [PyTorch](https://pytorch.org) and follow their
installation instructions. Once you've done that, run the above command again.
If you're still having trouble, and have given your best attempt at solving
the problem on your own, open up an issue.

### Running Tests

To run all of the unit tests, run `pytest` from the root of the repository.

In addition, the following sets of arguments may be useful:

```bash
# To explicitly skip tests on the gpu (by default, gpu tests will
# only run if there is a gpu available)
pytest -m 'not gpu'

# To include Negative Log-Likelihood gradient tests which are skipped by default
# These tests tend to be fairly slow and are also be a bit inconsistent
# as they involve random sampling.
pytest --nll

# To skip tests marked as 'slow', these include tests which train models
# for a few epochs, as well as the NLL tests (may give confusing results if
# run together with --nll)
pytest -m 'not slow'

pytest -m 'not slow and not gpu'  # exclude slow AND gpu tests

# Run tests along with any notebooks in the current directory (also searches
# subdirectories) and check if they produce any errors
pytest --nbval-lax

pytest --nbval-lax ./examples # check only the example notebooks

pytest --cov=qucumber  # to show test coverage

pytest --durations=N  # show test durations for the N slowest tests (use N=0 to show all)
```

### Building Documentation

If you're working on code documentation, you can build a copy locally like so:

```bash
cd ./docs  # go into the docs folder if you aren't already there
make html  # build the docs in HTML format
```

This will build the HTML files for the documentation and put them in the
`docs/_build/html` folder. Alternatively, you can use `sphinx-autobuild` to
automatically rebuild the docs whenever a file is changed using:

```bash
make livehtml  # again, run this from inside the docs directory
```

then open your web-browser to `localhost:8000` to view the automatically
updated documentation. Occasionally, the autobuilder may get stuck in an
infinite loop. In this case, it suffices to exit it with `Ctrl+C` and running
`make livehtml` again.

### Code Style

QuCumber's Python code follows [`black`'s](https://github.com/ambv/black) styling
conventions + `flake8`. These should have been installed when you installed the
development dependencies. Our custom `flake8` config can be found in the repository's
`setup.cfg` file.

To run code style checks, run the following from the repo's root directory:

```bash
flake8
```

and to run the `black` autoformatter (be aware that this will modify your code)

```bash
black ./
```

For convenience, you can add both of these as `git` pre-commit hooks, meaning
that everytime you commit your code, these checks will be run and your commit
will be cancelled if they fail.

First, install the `flake8` hook:

```bash
flake8 --install-hook git
```

and then the `black` hook using the `pre-commit` python tool:

```bash
pre-commit install
```

Note that the hooks have to be installed in this order as the `flake8` hook
installation will fail if the `pre-commit` hook has already been installed.

#### Jupyter Notebook style

We also require that Jupyter notebooks on this repo follow the same style
conventions as the main package source code. Unfortunately, the
git pre-commit hooks won't work on Jupyter notebooks. To get around this,
we've written some custom [PyInvoke](https://www.pyinvoke.org/) commands to
check style guide adherence of notebooks in the repo's `./examples/` folder.

You can check that they are formatted properly by running the following
commands from the repo's root directory:

```bash
inv lint-examples -l flake8
inv lint-examples -l black
```

Unfortunately, `black` does not support autoformatting of Jupyter notebooks,
but the above command will provide you with the changes that it would have made.
However, there is an easier way, you can use the
[blackcellmagic](https://github.com/csurfer/blackcellmagic) Jupyter extension
to format notebook cells.

### License Headers

All significant Python source files should contain this repo's license header at
the top of the file. The header text itself can be found in the
[LICENSE_HEADER](LICENSE_HEADER) file.

You can also run the following to check if all Python files of length > 15
contain the license header:

```bash
inv license-check
```

## Submitting a Pull Request

Once you've finished writing your code (and testing it sufficiently!), you can
open up a pull request against the `develop` branch (don't PR to `master`, as
we want it to be as close to the last stable release as possible).

Your code will be run through all of the checks listed above (except the
Jupyter Notebook style checks) automatically,
and the checks *must* pass in order for your code to be merged. A PIQuIL
team member will then review your code when they get the chance, and if
everything looks good, will merge your code.
