### Installing and using Python packages.
We often have a problem when working on different projects in the local system
we might need different python versions for different projects (less common) or
we might need python packages compatible with particular versions (more likely).
Virtual environments for different projects for easy deployments
After stumbling with this problem, I found a perfect solution using two awesome libraries — Pyenv and Pipenv.
Pyenv is to manage Python versions, and Pipenv is to create virtual environments required for each project and manage python packages and their dependencies for each project.
#### Installing Pyenv

Installing Pipenv
```
brew install pyenv
```

Use pip to install Pipenv:
```bash
$ pip install --user pipenv

```
