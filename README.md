# spork drawer template

A starting point for a new [spork](https://github.com/spork-pm/spork) drawer: a repo of
JSON manifests that spork installs packages from.

## Using this template

1. On GitHub, click **Use this template → Create a new repository**.
2. In this README, replace `<owner>/<repo>` and `<name>`, describe what the drawer holds,
   and delete this section.
3. Add your first manifest in [drawer/](drawer/) (see below), then delete `drawer/.gitkeep`.
4. In the new repo's settings:
   - To have autoupdates opened as pull requests instead of pushed (`mode: pr` in
     [autoupdate.yml](.github/workflows/autoupdate.yml)), turn on **Actions → General → Allow
     GitHub Actions to create and approve pull requests**.
   - To block broken manifests, protect `main` and make the `validate / validate` and
     `validate / install` checks required.

The two workflows run spork's own tools, so they need no setup:

- [validate.yml](.github/workflows/validate.yml) checks each pull request: required fields,
  `checkver` and `autoupdate` rules, and a test install on macOS.
- [autoupdate.yml](.github/workflows/autoupdate.yml) looks for new versions every 6 hours and
  commits updated manifests after a test install.

Both are described in [spork's tools README](https://github.com/spork-pm/spork/tree/main/tools#readme).

---

# `<name>` drawer

`<what this drawer holds>`, for [spork](https://github.com/spork-pm/spork), the macOS package
manager that needs no admin rights.

```bash
spork drawer add <name> https://github.com/<owner>/<repo>
spork install <name>/<package>
```

## Packages

Browse [drawer/](drawer/) for the full list: one JSON manifest per package, named after it.
To search from the command line, run `spork search <term>`.

## Adding a package

Add one JSON manifest per package in [drawer/](drawer/), named after the package
(`drawer/<package>.json`). The format is described in
[spork's README](https://github.com/spork-pm/spork#manifest-format). For example:

```json
{
    "version": "1.8.2",
    "description": "Command-line JSON processor",
    "homepage": "https://jqlang.org",
    "license": "MIT",
    "architecture": {
        "arm64": {
            "url": "https://github.com/jqlang/jq/releases/download/jq-1.8.2/jq-macos-arm64#/jq",
            "hash": "sha256:2d75340ba57a4b4b4c8708a21c2dc8e958a48aaa8bba13b27f77f6e4c0eca07e"
        },
        "x86_64": {
            "url": "https://github.com/jqlang/jq/releases/download/jq-1.8.2/jq-macos-amd64#/jq",
            "hash": "sha256:e94b266e3c26690550006abe63152b782280f4e14374accdf04cbde844f00bc0"
        }
    },
    "bin": "jq",
    "checkver": {"github": "jqlang/jq"},
    "autoupdate": {
        "architecture": {
            "arm64": {"url": "https://github.com/jqlang/jq/releases/download/jq-$version/jq-macos-arm64#/jq"},
            "x86_64": {"url": "https://github.com/jqlang/jq/releases/download/jq-$version/jq-macos-amd64#/jq"}
        }
    }
}
```

`checkver` and `autoupdate` are optional. With them, the autoupdate workflow keeps the manifest
current on its own.

Before opening a pull request:

```bash
spork hash <url>                          # get a checksum
spork install ./drawer/<package>.json     # test the install
```
