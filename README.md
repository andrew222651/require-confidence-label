# Require PR Confidence Label

A GitHub Action that requires a pull request to carry exactly one
confidence-level label, e.g.:

* `confidence:low` — I would be surprised if this code doesn't have problems
* `confidence:medium` — I don't have a strong belief either way
* `confidence:high` — I would be surprised if this code has problems

Having the author state their confidence up front tells reviewers how
carefully they need to check the patch.

This action is a thin wrapper around
[`mheap/github-action-required-labels`](https://github.com/mheap/github-action-required-labels)
preconfigured to require exactly one label from a confidence-level set.

## Usage

Create labels named `confidence:low`, `confidence:medium`, and
`confidence:high` in your repository, then add a workflow:

```yaml
name: Require PR confidence level label
on:
  pull_request:
    types: [opened, labeled, unlabeled, synchronize]

jobs:
  confidence-label:
    runs-on: ubuntu-latest
    steps:
      - uses: andrew222651/require-confidence-label@v1
```

See [`examples/require-confidence-label.yml`](examples/require-confidence-label.yml).

## Inputs

| Name     | Description                                                | Default                                                    |
|----------|--------------------------------------------------------------|-------------------------------------------------------------|
| `labels` | Comma-separated list of confidence labels; exactly one must be present | `confidence:low, confidence:medium, confidence:high` |
| `mode`   | Matching mode (`exactly`, `minimum`, `maximum`)               | `exactly`                                                    |
| `count`  | Number of matching labels required                           | `1`                                                          |
| `message`| Message shown when the check fails                           | see [`action.yml`](action.yml)                               |
| `token`  | Token used to read PR labels                                  | `${{ github.token }}`                                        |

## Outputs

| Name     | Description                                      |
|----------|---------------------------------------------------|
| `labels` | The labels that were matched, comma-separated      |
| `status` | The status of the check                            |

## Known limitation

This only supports a single confidence label for the whole PR. If an author
is confident about part of a patch but not another part, this scheme can't
express that.
