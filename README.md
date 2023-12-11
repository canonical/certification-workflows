# certification-github-workflows

This repository is the home to reusable GitHub
[workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
and
[composite actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)
useful across the different pieces of the Certification ops stack.

- `.github/actions/microk8s-setup`: Sets up a juju managed microk8s cluster in a
  ready-to-be-tested state, with docker.io image registry mirroring configured
  for Canonical self-hosted runners in mind.
- `.github/actions/archive-charm-testing-artifacts`: Archives a charm build,
  logs and [juju crashdump](https://github.com/juju/juju-crashdump) output in
  case of a failed run.

## Usage examples

### k8s charm integration testing

Taking [github.com/canonical/test_observer] as an example, integration tests for
the frontend charm under `./frontend/charm` can be constructed using the actions
`microk8s-setup` and `archive-charm-testing-artifacts` as follows:

```yaml
jobs:
  integration-test:
    permissions:
      contents: read
      packages: read

    name: Integration tests
    runs-on: [self-hosted, jammy, xlarge]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up microk8s
        uses: canonical/certification-github-workflows/.github/actions/microk8s-setup

      - name: Run integration tests
        run: tox -e integration

      - name: Archive charm testing outputs
        uses: canonical/certification-github-workflows/.github/actions/archive-charm-testing-artifacts

    defaults:
      run:
        working-directory: ./frontend/charm
```
