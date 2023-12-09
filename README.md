# certification-github-workflows

This repository is the home to reusable GitHub workflows and
[composite actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)
useful across the different pieces of the Certification ops stack.

- `.github/actions/microk8s-setup`: Sets up a juju managed microk8s cluster in a
  ready-to-be-tested state, with docker.io image registry mirroring configured
  for Canonical self-hosted runners in mind.
