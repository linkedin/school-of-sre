We realise that the initial content we created is just a starting point and our hope is that the community can help in the journey refining and extending the contents.

### Reporting issues

We use GitHub issues to track bug reports, feature requests, and submitting pull requests.

If you find a bug:

1. Use the GitHub issue search to check whether the bug has already been reported.

1. If the issue has been fixed, try to reproduce the issue using the latest master branch of the repository.

1. If the issue still reproduces or has not yet been reported, try to isolate the problem before opening an issue.

### Contributing content
As a contributor, you represent that the content you submit is not plagiarised. By submitting the content, you (and, if applicable, your employer) are licensing the submitted content to LinkedIn and the open source community subject to the Creative Commons Attribution 4.0 International Public License.

Ensure that you adhere to the following guidelines:

* Should be about principles and concepts that can be applied in any company or individual project. Do not focus on particular tools or tech stack(which usually change over time).
* Adhere to the [Code of Conduct](/school-of-sre/CODE_OF_CONDUCT/).
* Should be relevant to the roles and responsibilities of an SRE.
* Should be [locally tested](#building-and-testing-locally) and well formatted.
* It is good practice to open an issue first and discuss your changes before submitting a pull request. This way, you can incorporate ideas from others before you even start.

### Building and testing locally
Run the following commands from the root of the repo to build and view the site locally before opening a PR.

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
mkdocs build
mkdocs serve
```

### Submitting a Pull Request (PR)

Before you submit your Pull Request (PR), consider the following guidelines:

* Search this repo for an open or closed PR that relates to your submission. You don't want to duplicate effort.
* Follow the [standard GitHub approach](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork) to create the PR. Please also follow our [commit message format](#commit-message-format).
* That's it! Thank you for your contribution!

### Commit Message Format

Please follow the [Conventional Commits](https://www.conventionalcommits.org/) specification for the commit message format. In summary, each commit message consists of a *header*, a *body* and a *footer*, separated by a single blank line.

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

Any line of the commit message cannot be longer than 88 characters! This allows the message to be easier to read on GitHub as well as in various Git tools.

### Type

Must be one of the following (based on the [Angular convention](https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines)):

* *feat*: A new feature
* *fix*: A bug fix
* *docs*: Documentation only changes
* *style*: Changes that do not affect the meaning of the code (whitespace, formatting, missing semicolons, etc.)
* *build*: Changes that affect the build system or external dependencies

A scope may be provided to a commit’s type, to provide additional contextual information and is contained within parenthesis, e.g., 
```
feat(parser): add ability to parse arrays
```

### Description

Each commit must contain a succinct description of the change:

* use the imperative, present tense: "change" not "changed" nor "changes"
* don't capitalize the first letter
* no dot(.) at the end

### Body
Just as in the description, use the imperative, present tense: "change" not "changed" nor "changes". The body should include the motivation for the change and contrast this with previous behavior.