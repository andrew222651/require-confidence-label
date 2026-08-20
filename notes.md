# Expressing confidence within a merge request

When submitting a merge request and requesting a code review, authors can
communicate their confidence in the patch on a three-point scale:

* low = I would be surprised if this code doesn't have problems
* medium = I don’t have a strong belief either way
* high = I would be surprised if this code has problems

This way, reviewers know how careful they need to be.

For example, in GitHub there can be PR labels for the different levels, whose
presence is enforced with the following GitHub Actions workflow:
```yaml
name: Require PR confidence level label
on: pull_request
jobs:
  label_test:
    runs-on: ubuntu-latest
    steps:
      - uses: mheap/github-action-required-labels@v3
        with:
          mode: exactly
          count: 1
          labels: "confidence:low, confidence:medium, confidence:high"
          message: "Label error. Requires exactly one of: [{{ provided }}]. Fix labels and re-run this check."
```

Perhaps there would ideally be a way of tracking which PRs did indeed have issues so people's
beliefs can be evaluated.

Sometimes the author is confident in one part of a patch and not confident in another in
which case this system is too simple.

