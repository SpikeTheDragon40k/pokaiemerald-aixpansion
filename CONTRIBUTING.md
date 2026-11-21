# Contributing to this pokeemerald-expansion fork

First off, thanks for helping improve `pokeemerald-expansion`! ❤️

All contributions are encouraged and valued. Please make sure to read the relevant section before making your contribution! It will make it a lot easier for you and the maintainers. We're excited to see your contributions. 🎉

## AI-Assisted Contributions Policy

This is an **independent project** that started as a fork of RHH's `pokeemerald-expansion` and no longer syncs with it. **This project welcomes contributions written with help from large language models (LLMs)**, such as Claude, ChatGPT, Copilot or local models. Use these tools to build features that would take much longer to write by hand.

AI does not lower the bar. Every PR, however it was written, must meet the same rules:

1. **It builds.** `make -j$(nproc)` must succeed for Emerald, and `make firered` and `make leafgreen` must not break.
2. **The tests pass.** `make check -j$(nproc)` must pass with no new failures. CI runs both on every PR.
3. **New behaviour has tests.** Battle mechanics, items, abilities and moves need tests in `test/`. See the [testing system guide](docs/tutorials/how_to_testing_system.md). Do not delete, skip or weaken tests to get CI green.
4. **It follows the [style guide](docs/STYLEGUIDE.md)** and its principles: minimally invasive, behind a config where it makes sense, and no save-format changes without discussion.
5. **You disclose it.** Fill in the "AI assistance" section of the PR template: which tools you used and what you checked by hand.
6. **You are accountable.** You must understand the code you submit and be able to answer review questions about it. "The AI wrote it" is not an answer. Answers that are wrong, made up, or only pasted from a model will get a PR closed.
7. **Keep PRs focused.** One feature or fix per PR. Huge, sweeping generated diffs are hard to review and will be asked to be split.

LLM-generated assets (graphics, music) are accepted only if you have the right to use them and you say so in the PR.

> ⚠️ Open AI-assisted PRs **here**. The original RHH project does not accept LLM-assisted contributions.

Agents and their users should read [`AGENTS.md`](AGENTS.md) first: it explains how to build, test and navigate this codebase.

## Bug Reports

We use [GitHub](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues?q=sort%3Aupdated-desc+is%3Aissue+is%3Aopen+label%3Abug) issues to track bugs.

### What should I do before making a bug report?

- Does your bug occur on the latest unmodified (clean) version of the [`master`](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/tree/master) branch? If not, please do not submit a report - the issue is most likely one introduced by your game.
- Has somebody else already found this issue? This is best done by searching the [bug tracker](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues?q=label%3Abug) to see if anybody else reported it. If there is already an issue, replying to the exsting issue with more information can help solve the problem.

### How do I submit a bug report?

If you run into an issue with the project, open an [issue](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues/new).

The best bug reports have enough information that we won't have to contact you for more information. We welcome all efforts to improve pokeemerald-expansion, but would be very grateful if you completed as much of the checklist as possible in your bug report. This will help other contributiors fix your issue.

### What happens after I submit a bug report?

- A maintainer will [label](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/labels) the bug report.
- A maintainer will try to reproduce the bug with your provided steps.
    - If there are no reproduction steps or no obvious way to reproduce the issue, somebody will ask you for those steps. Until the bug can be reproduced, the bug will retain the `bug:unconfirmed` label. Unconfirmed bugs are less likely to get fixed.
- If the team is able to reproduce the bug, it will be labeled `bug:confirmed`, and the bug will be left to be [fixed by someone](#Pull-Requests).
    - If the issue is particularly game-breaking, a maintainer will add it to a future version's [milestone](), meaning that version will not be released until the problem is solved.

## Feature Requests

This section guides you through submitting a feature request for pokeemerald-expansion, **including completely new features and minor improvements to existing functionality**. Following these guidelines will help maintainers and the community to understand your suggestion and find related suggestions.

- We use [GitHub](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues?q=sort%3Aupdated-desc+is%3Aissue+is%3Aopen+label%3Afeature-request) issues to track feature requests.

### What should I do before making a feature request?

- Make sure your request is in [pokeemerald-expansion's scope](docs/team_procedures/scope.md) - if it is not clear if something is in scope, open an [issue](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues).

### What should I do before making a feature request?

- Read the [documentation](docs/) to find out if the functionality is already covered, maybe by an individual configuration.
- Perform a [search](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues) to see if the feature has already been requested. If it has, add a comment to the existing issue instead of opening a new one.

### How do I submit a feature request?

To request a feature to be added to the project, open a [feature request](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues/new).

### What happens after I submit a feature request?

- A maintainer will [label](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/labels) the issue.
- If the feature request is out of [scope](docs/team_procedures/scope.md), it will be closed.
- if the request is in scope, any other contributor can volunteer to [fufill it via a pull request](#Pull-Requests). When the request is filled, the request will be closed.

## Pull Requests

If you have read all of this and still need help, feel free to open an [issue](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues).

### What should I do before starting a pull request?

- If you're new to git and GitHub, [Team Aqua's Asset Repo](https://github.com/Pawkkie/Team-Aquas-Asset-Repo/) has a [guide on forking and cloning the repository](https://github.com/Pawkkie/Team-Aquas-Asset-Repo/wiki/The-Basics-of-GitHub). Make sure you have a [local copy](INSTALL.md) of `pokeemerald-expansion`.
- Make sure your contribution is in [scope](docs/team_procedures/scope.md) - if it is not clear if something is in scope, open an [issue](https://github.com/SpikeTheDragon40k/porkeemerald-explansion/issues).
- All pull requests target `master`.
- Create a new branch from the most recent `master`.
- If your contribution introduces, removes, or changes a lot of existing code, we reccomend getting a maintainer to agree to review it before you start on the work! We have a table that lists all [current maintainers and their areas of expertise](#maintainers).

### How do I submit a pull request?

#### 1. Get a working local copy
If you haven't already, follow [INSTALL.md](INSTALL.md) to get a working local copy of `pokeemerald-expansion`.

#### 2. Set this fork as a remote
This will designate this repository as a remote.
```bash
git remote add fork https://github.com/SpikeTheDragon40k/porkeemerald-explansion # You can replace fork with anything you want.
```

#### 3. Create a new branch
This will create a new branch and switch to it.
```bash
git switch -c newFeature # the name newFeature can be anything you want. This tutorial assumes you used newFeature.
```

#### 4. Start from the latest master
This will change your new branch to match the latest `master`.
```bash
git fetch fork && git reset --hard fork/master
```

#### 5. Implement your code
All of your work should go on this new, clean branch. If you already started work on a different branch, you can [cherry-pick](https://git-scm.com/docs/git-cherry-pick) you old commits onto this new branch, or just copy and paste the changes from the original files.

##### Popular Features / Feature Branches

If you are implementing functionality from a known community feature branch, it is **strongly** recommended that you open an issue _before_ starting. There are some situations where maintainers would ask you to use the existing feature branch as a base, and others where maintainers would want a feature to be written from scratch.

This changes on a case by case basis.

#### 6. Push your changes
When you push your first commit, you'll need to push the new branch to the remote repo.
```bash
git push --set-upstream origin newFeature
```

#### 7. Open Pull Request
Once your work is complete and pushed to the branch on Github, you can open a [pull request from your branch](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork), targeting `master` in this repository. Please fill out the pull request description as completely as possible.

### What happens after I submit a pull request?

A maintainer will then assign themselves as a reviewer of your pull request, and may provide feedback in the form of a PR review.

Contributors are responsible for responding to and updating their branch by addressing the feedback in the review. Contributors are also responsible for making sure the branch passes the checklist at all times.

Once a maintainer has begun reviewing your PR, **please** do not force-push new changes - normal pushes are fine. Do not worry about git history - we squash most incoming changes.

Maintainers will measure the submitted pull request against a [merge checklist](docs/team_procedures/merge_checklist.md).

Once all items on the merge checklist are true, the branch will be merged in.


## Maintainers

This project is maintained by [SpikeTheDragon40k](https://github.com/SpikeTheDragon40k). The maintainers of RHH's `pokeemerald-expansion` are not involved in this project, so please do not contact them about it.

## Attribution
This guide is based on the [contributing.md](https://contributing.md/generator)!
