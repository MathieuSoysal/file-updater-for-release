# Files updater for release
[![Test Actions](https://github.com/MathieuSoysal/file-updater-for-release/actions/workflows/test-action-final.yml/badge.svg)](https://github.com/MathieuSoysal/file-updater-for-release/actions/workflows/test-action-final.yml)*(Tested on Ubuntu, Macos, Windows)*


Update automatically the old version in your readme (or other files) with your new tag version.

## Usage

### With automatic commit

The workflow, usually declared in `.github/workflows/post-release.yml`, looks like:

<details open>

<summary>.github/workflows/post-release.yml</summary>

```YAML
name: Update files

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Update files
        uses: MathieuSoysal/file-updater-for-release@v1.0.1
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          prefix: "MathieuSoysal/file-updater-for-release@" # Prefix before the version, default is: ""
          suffix: ""
          files: README.md # List of files to update
          commit-message: "Update version in README.md" # Commit message, default is: "Update version in files"
```
</details>

### Without giving GITHUB_TOKEN

The workflow, usually declared in `.github/workflows/post-release.yml`, looks like:

<details open>
<summary>.github/workflows/post-release-gradle.yml</summary>



```YAML
name: Update files

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - name: Update files
        uses: MathieuSoysal/file-updater-for-release@v1.0.1
        with:
          files: README.md # List of files to update
          with-commit: false # If you don't want to commit the changes
      
      - name: Commit changes
        run: |
          git config --local user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git config --local user.name "github-actions[bot]"
          git add README.md
          git commit -m "Update version in README.md"
          git push
```
</details>

## Contributing

Want to contribute to File Updater? Awesome! Check out [the contributing guidelines](CONTRIBUTING.md) to get involved.

### Requirements to your environment to test in locally

- Install [nektos/act](https://github.com/nektos/act) & clone the repo `git clone git@github.com:MathieuSoysal/file-updater-for-release.git`  
OR
- Use the devcontainer of the repo: with [GitHub Codespaces](https://github.com/codespaces/new?hide_repo_select=true&ref=main&repo=600417768)

### Command to test your changes

```bash
act -W .github/workflows/test-action-local.yml
```

### Stars 🎇

If you like or use this project, please don't forget to give it a star ⭐️. Thanks!

## License
The Dockerfile and associated scripts and documentation in this project are released under the [Apache 2.0 License](https://github.com/MathieuSoysal/file-updater-for-release/blob/main/LICENSE).