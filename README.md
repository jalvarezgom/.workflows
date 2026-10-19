# .workflows

Reusable GitHub Actions workflows.

## python-build.yml

Runs tests on a Python matrix and builds the package with pdm. Uploads `dist/` as the `dist` artifact.
Version comes from the git tag (`pdm-backend` scm source).

PyPI Trusted Publishing does not support reusable workflows, so the publish job lives in each project:

```yaml
on:
  push:
    tags: ["v*.*.*"]
jobs:
  build:
    uses: jalvarezgom/.workflows/.github/workflows/python-build.yml@main
    with:
      python-versions: '["3.11", "3.12", "3.13", "3.14"]'
  publish:
    needs: build
    runs-on: ubuntu-latest
    environment: pypi
    permissions:
      id-token: write
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: dist
          path: dist/
      - uses: pypa/gh-action-pypi-publish@release/v1
```
