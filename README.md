## This repo has been forked from [danilo-delbusso/pr-review-labeller](https://github.com/danilo-delbusso/pr-review-labeller) and all of the hard work was already done. Much credit to the original author(s). 

### This fork simply adds an extra label for 'needs review'. 

It's something I needed for a project of mine and is largely untested (it will probably fall over and die).
I'm currently using it in said project and will make any fixes etc as necessary.

As a ~filthy degenerate~ badass gangsta, I've been committing to master, so bare that in mind if you plan on using this madness. 
For this reason, I highly recommend using [the original](https://github.com/danilo-delbusso/pr-review-labeller) unless you really need the 'needs review' status.

Once I'm confident this works how I want it to, I'll create a release, or open a PR to the original if the author is willing to accept it.

Until then, 'here be dragons'.

(The rest of the readme is from the original repo, updated to include the added field and the event types to trigger it).

---

### Attention! 🚨

If you're using `GITHUB_TOKEN`, you'll only be able to use this action for PRs opened within the same repository.

If you're planning to support PRs opened from forks, you can enable the option to  [Send write tokens to workflows from pull requests](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository#enabling-workflows-for-private-repository-forks). However, this is a **high risk option**, as described in the docs.

See this comment for more information: https://github.com/xenserver/xenadmin/pull/2875#issuecomment-930281214

You can also use this [workaround](https://stackoverflow.com/a/67249854), which would avoid using such sensitive permissions. See https://github.com/danilo-delbusso/pr-review-labeller/issues/1 for more.

# Update PR Status Labels Action

This action updates the labels of a PR after a review has been added.

It currently supports four labels:

- One Approval label
- Two Approvals label
- Changes Requested label
- Updated PR Label

These labels have to exist in the repository already.

You will simply need to specify their names in the `.yml` configuration.

## Inputs

### `needs-review-label-name`

**Required** Name of the label to show when the PR has zero approvals. Defaults to `needs review`.

### `one-approval-label-name`

**Required** Name of the label to show when the PR has one approval. Defaults to `1 approval`.

### `two-approvals-label-name`

**Required** Name of the label to show when the PR has two approvals. Defaults to `2 approvals`.

### `changes-requested-label-name`

**Required** Name of the label to show when the PR needs changes. Defaults to `needs updating`.

### `updated-pr-label-name`

**Required** Name of the label to show when the PR has been updated (new commits). Defaults to `updated`.

## Environment Variables

## `GITHUB_TOKEN`

The token needs to be added to the yml description in order for the action to call GitHub's API.

## Example usage with default label names

You can replace `pull_request` with `pull_request_target` if you need to.

```yml
name: Update PR Labels
on:
  pull_request_review:
    types:
      - submitted
      - edited
      - dismissed
  pull_request:
    types:
      - opened
      - reopened
      - synchronize

jobs:
  update-pr-labels:
    runs-on: ubuntu-latest
    name: Update PR Labels
    steps:
      - name: Update Labels
        uses: danilo-delbusso/pr-review-labeller@v1.2.3
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Example usage with custom label names

```yml
name: Update PR Labels
on:
  pull_request_review:
    types:
      - submitted
      - edited
      - dismissed
  pull_request:
    types:
      - opened
      - reopened
      - synchronize

jobs:
  update-pr-labels:
    runs-on: ubuntu-latest
    name: Update PR Labels
    steps:
      - name: Update Labels
        uses: danilo-delbusso/pr-review-labeller@v1.2.3
        with:
          one-approval-label-name: "1 approval"
          two-approvals-label-name: "2 approvals"
          changes-requested-label-name: "needs updating"
          updated-pr-label-name: "updated"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
