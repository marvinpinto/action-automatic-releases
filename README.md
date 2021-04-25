# GitHub Automatic Releases

This action simplifies the GitHub release process by automatically uploading assets, generating changelogs, handling pre-releases, and so on.

## Contents

1. [Usage Examples](#usage-examples)
1. [Supported Parameters](#supported-parameters)
1. [Event Triggers](#event-triggers)
1. [Versioning](#versioning)
1. [How to get help](#how-to-get-help)
1. [License](#license)

> **NOTE**: The `marvinpinto/action-automatic-releases` repository is an automatically generated mirror of the [marvinpinto/actions](https://github.com/marvinpinto/actions) monorepo containing this and other actions. Please file issues and pull requests over there.

## Usage Examples

### Automatically generate a pre-release when changes land on master

This example workflow will kick in as soon as changes land on `master`. After running the steps to build and test your project:

1. It will create (or replace) a git tag called `latest`.
1. Generate a changelog from all the commits between this, and the previous `latest` tag.
1. Generate a new release associated with the `latest` tag (removing any previous associated releases).
1. Update this new release with the specified title (e.g. `Development Build`).
1. Upload `LICENSE.txt` and any `jar` files as release assets.
1. Mark this release as a `pre-release`.

You can see a working example of this workflow over at [marvinpinto/actions](https://github.com/marvinpinto/actions/releases/tag/latest).

```yaml
---
name: "pre-release"

on:
  push:
    branches:
      - "master"

jobs:
  pre-release:
    name: "Pre Release"
    runs-on: "ubuntu-latest"

    steps:
      # ...
      - name: "Build & test"
        run: |
          echo "done!"

      - uses: "marvinpinto/action-automatic-releases@latest"
        with:
          repo_token: "${{ secrets.GITHUB_TOKEN }}"
          automatic_release_tag: "latest"
          prerelease: true
          title: "Development Build"
          files: |
            LICENSE.txt
            *.jar
```

### Create a new GitHub release when tags are pushed to the repository

Similar to the previous example, this workflow will kick in as soon as new tags are pushed to GitHub. After building & testing your project:

1. Generate a changelog from all the commits between this and the previous [semver-looking](https://semver.org/) tag.
1. Generate a new release and associate it with this tag.
1. Upload `LICENSE.txt` and any `jar` files as release assets.

Once again there's an example of this over at [marvinpinto/actions](https://github.com/marvinpinto/actions/releases/latest).

```yaml
---
name: "tagged-release"

on:
  push:
    tags:
      - "v*"

jobs:
  tagged-release:
    name: "Tagged Release"
    runs-on: "ubuntu-latest"

    steps:
      # ...
      - name: "Build & test"
        run: |
          echo "done!"

      - uses: "marvinpinto/action-automatic-releases@latest"
        with:
          repo_token: "${{ secrets.GITHUB_TOKEN }}"
          prerelease: false
          files: |
            LICENSE.txt
            *.jar
```

## Supported Parameters

| Parameter               | Description                                                | Default  |
| ----------------------- | ---------------------------------------------------------- | -------- |
| `repo_token`\*\*        | GitHub Action token, e.g. `"${{ secrets.GITHUB_TOKEN }}"`. | `null`   |
| `draft`                 | Mark this release as a draft?                              | `false`  |
| `prerelease`            | Mark this release as a pre-release?                        | `true`   |
| `automatic_release_tag` | Tag name to use for automatic releases, e.g `latest`.      | `null`   |
| `title`                 | Release title; defaults to the tag name if none specified. | Tag Name |
| `files`                 | Files to upload as part of the release assets.             | `null`   |

## Outputs

The following output values can be accessed via `${{ steps.<step-id>.outputs.<output-name> }}`:

| Name                     | Description                                            | Type   |
| ------------------------ | ------------------------------------------------------ | ------ |
| `automatic_releases_tag` | The release tag this action just processed             | string |
| `upload_url`             | The URL for uploading additional assets to the release | string |

### Notes:

- Parameters denoted with `**` are required.
- The `files` parameter supports multi-line [glob](https://github.com/isaacs/node-glob) patterns, see repository examples.

## Event Triggers

The GitHub Actions framework allows you to trigger this (and other) actions on _many combinations_ of events. For example, you could create specific pre-releases for release candidate tags (e.g `*-rc*`), generate releases as changes land on master (example above), nightly releases, and much more. Read through [Workflow syntax for GitHub Actions](https://help.github.com/en/articles/workflow-syntax-for-github-actions) for ideas and advanced examples.

## Versioning

Every commit that lands on master for this project triggers an automatic build as well as a tagged release called `latest`. If you don't wish to live on the bleeding edge you may use a stable release instead. See [releases](../../releases/latest) for the available versions.

```yaml
- uses: "marvinpinto/action-automatic-releases@<VERSION>"
```

## How to get help

The main [README](https://github.com/marvinpinto/actions/blob/master/README.md) for this project has a bunch of information related to debugging & submitting issues. If you're still stuck, try and get a hold of me on [keybase](https://keybase.io/marvinpinto) and I will do my best to help you out.

## License

The source code for this project is released under the [MIT License](/LICENSE). This project is not associated with GitHub.
