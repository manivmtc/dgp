# Contribution Guidelines


Welcome to `TRI-ML/dgp`! This page details contribution guidelines and GitWorkflow.

## Clone the repository

1. Fork `TRI-ML/dgp` into your Github account.
2. Clone your fork `<your-user-name>/dgp` on your local machine.
3. Add `upstream` remote:
   * `cd dgp`
   * `git remote add upstream git@github.com:TRI-ML/dgp.git`
   * `git remote set-url --push upstream no_push`
4. Enable githooks (linting, auto-formatting) via:

```sh
dgp$ make link-githooks
```

## Getting started

Please follow [Getting Started](GETTING_STARTED.md) to setup dockerized development workflow.

## GitWorkflow

This section assumes you have followed the initial setup instructions and enable the githooks.

### Starting a new branch
1. `git fetch upstream`
2. `git checkout -b my_pr_branch upstream/master`

### Making a commit
There should only be a single commit with all the changes when making a pull request. Please squash the commits before opening a PR.
This reposiotry follows [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) for the commit message convention. All commit messages will be verified by [commitlint](https://github.com/conventional-changelog/commitlint).

The commit message should be structured as follows:
```sh
<type>: <description>

[optional body]

[optional footer(s)]
```

When naming the commit, the first line (commit title) should be a short summary **in ALL lowercase** and starts with a `type`. [Common types](https://github.com/conventional-changelog/commitlint/tree/master/@commitlint/config-conventional#type-enum) can be:

- `feat`: introduce new features
- `fix`: bug fix
- `test`: changes to unit tests
- `refactor`: code refactor
- `doc`: document updates
- `build`: requirement.txt and Dockerfile updates
- `ci`: changes to CI/CD
- `schema`: changes to protobuf schema

For example, adding a new PyTorch DatasetClass:
```sh
feat: add synchronized datasetclass

- add synchronized datasetclass to load time-synchronized samples
- speed up build_item_index
```

Add new protobuf schema:
```sh
schema: add map schema

- add protobuf schema for map
- modify dgp.proto.dataset to hold map message
```

**NOTE:** Any [proto schema](../dgp/proto) changes must be seperated from code changes into an independent commit and PR.

### Pre-commit / Pre-push
DGP runs `isort` and `yapf` to auto-format the files in pre-commit, and perform additional linting using `pylint` in pre-push. One can enable githooks via:
```sh
dgp$ make link-githooks
```

### Making a pull request

Please follow this procedure to open a pull request. Also note, the git hooks require that tools such as isort and yapf are installed on the machine doing the git push. If these are not available on your system, you will first need to install them, ideally in the [docker container](GETTING_STARTED.md#markdown-develop-within-docker) or in a [virtual environment](VIRTUAL_ENV.md).

1. Rebase to master
   * `git fetch upstream`
   * `git rebase upstream/master`
2. Push to your GHE fork
   * `git push origin`
3. Create "Pull Request" (PR)
   * Go to your fork in GHE and create a pull request per [these instructions](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request)
4. CI coverage
   * You can run unittests via `make docker-run-tests`.
   * You can make PRs with no reviewers to get CI coverage.
   * All pull requests must pass certain Jenkins stages in order to be merged:
     * Build
     * Unit tests

### Review
The pull request has a reviewable.io review associated with it. (You will need to reload the pull request page before the reviewable.io button appears). Please add at least one reviewer to review.
Note: Any [proto schema](../dgp/proto) changes require _**at least 2 reviewers.**_

### Merging
Once all reviews are complete, and all Jenkins runs have completed with passing scores, the author can click the button `Squash & Merge` to merge the PR.

### Code Style
Docstrings should follow the [Numpy Docstring Standard](https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard)