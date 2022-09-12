# Pisound Documentation

If your are looking for Pisound documentation, head straight to the [Pisound Docs page](https://blokas.io/pisound/docs/)!


# Contribution Guide

We love your input! We want to make contributing to this project as easy and transparent as possible, whether it's:

- Reporting an issue
- Discussing the current state of the docs and the code
- Submitting a fix
- Proposing new features

## Reporting Issues
We use public GitHub issues to track bugs and requests. Just open [a new issue](https://github.com/BlokasLabs/pisound-docs/issues) - it's that easy!

## Submit Changes
[GitHub Pull requests](https://docs.github.com/en/pull-requests) are the best way to propose changes to the docs and the codebase. We actively welcome your pull requests! You can find an extensive contribution guide [here](https://github.com/firstcontributions/first-contributions), but in short:

1. Fork this repo and create your branch from master.
1. Make your changes.
1. Issue that pull request!


# Serving Docs via `mkdocs`

```
$ python --version
Python 3.5+
```

1. `pip install mkdocs==1.1.2 pymdown-extensions==8.0 mkdocs-redirects==1.0.1 mkdocs-material==5.5.12`
1. `cd pisound-docs`
1. `git submodule init`
1. `git submodule update`
1. `mkdocs serve`