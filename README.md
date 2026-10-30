# spork fonts drawer

Fonts for [spork](https://github.com/spork-pm/spork), the macOS package manager that needs no
admin rights. They're installed into `~/Library/Fonts`, where macOS picks them up for your user
only.

This drawer isn't added by spork's installer. To add it:

```bash
spork drawer add fonts
spork install jetbrainsmono-nf
```

Restart an app that's already running to see a new font in it.

## Packages

| Package | Font |
|---|---|
| `cascadiacode-nf` | Cascadia Code (as *CaskaydiaCove Nerd Font*) |
| `firacode-nf` | Fira Code |
| `hack-nf` | Hack |
| `jetbrainsmono-nf` | JetBrains Mono (with and without ligatures) |
| `meslo-nf` | Meslo LG, in its S, M and L line spacings, with and without a dotted zero |

The `-nf` packages are [Nerd Fonts](https://www.nerdfonts.com): the original font patched with
thousands of icons used by shell prompts, editors and terminal tools. Each one installs three
variants:

- **Nerd Font**: icons are a little wider than one cell. Best for most apps.
- **Nerd Font Mono**: icons are exactly one cell wide. Use it if icons overlap in your terminal.
- **Nerd Font Propo**: a proportional version, for user interfaces rather than code.

Browse [drawer/](drawer/) for the full list, or run `spork search nerd`.

## Adding a font

Add one JSON manifest per package in [drawer/](drawer/), named after the package
(`drawer/<package>.json`). The `font` field lists the font files to install: paths inside the
download, with wildcards allowed. The rest of the format is described in
[spork's README](https://github.com/spork-pm/spork#manifest-format).

For a Nerd Font, copy one of the existing `-nf` manifests and change the archive name, the
`description` and the `license`. The license is the original font's, found in the archive.
Name the package after the archive, lowercased, with `-nf` appended.

```json
{
    "version": "3.5.1",
    "description": "Fira Code Nerd Font: monospaced font with programming ligatures, patched with icons",
    "homepage": "https://www.nerdfonts.com",
    "license": "OFL-1.1",
    "url": "https://github.com/ryanoasis/nerd-fonts/releases/download/v3.5.1/FiraCode.tar.xz",
    "hash": "sha256:…",
    "font": "*.ttf",
    "checkver": {"github": "ryanoasis/nerd-fonts"},
    "autoupdate": {
        "url": "https://github.com/ryanoasis/nerd-fonts/releases/download/v$version/FiraCode.tar.xz",
        "hash": {"url": "$baseurl/SHA-256.txt"}
    }
}
```

With `checkver` and `autoupdate`, a workflow updates the manifests every 6 hours when Nerd Fonts
publishes a new release. Hashes come from the release's `SHA-256.txt`.

Before opening a pull request:

```bash
spork hash <url>                          # get a checksum
spork install ./drawer/<package>.json     # test the install
```
