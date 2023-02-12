# Files updater for release
[![Test Actions](https://github.com/MathieuSoysal/file-updater-for-release/actions/workflows/test-action-final.yml/badge.svg)](https://github.com/MathieuSoysal/file-updater-for-release/actions/workflows/test-action-final.yml)*(Tested on Ubuntu, Macos, Windows)*


Update automatically the old version in your readme (or other files) with your new tag version.

## Requirements
- You need to give permission to your GitHub Actions to create a pull request in your GitHub repo settings *(Settings -> Actions -> General)*.   

OR


- Instead of use `${{ secrets.GITHUB_TOKEN }}` use a GitHub [Personnal Acces Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#creating-a-fine-grained-personal-access-token) like : `${{ secrets.PAT }}`.

## Usage

### With pull request

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
          files: README.md # List of files to update
          prefix: "file-updater-for-release@" # Prefix before the version, default is: ""

      - name: Create Pull Request
        uses: peter-evans/create-pull-request@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }} # You need to create your own token with pull request rights
          commit-message: update readme
          title: Update readme
          body: Update readme to reflect release changes
          branch: update-readme
          base: main
```
</details>

### With directly commit

The workflow, usually declared in `.github/workflows/post-release.yml`, looks like:

<details open>
<summary>.github/workflows/post-release-gradle.yml</summary>



```YAML
name: Update files with commit

on:
  release:
    types: [published]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:

      - name: Checkout
        uses: actions/checkout@v3
        with:
          fetch-depth: 0
          token: ${{ secrets.GITHUB_TOKEN }} # You need to create your own token with commit rights
          ref: main # The branch you want to commit to


      - name: Update files
        uses: MathieuSoysal/file-updater-for-release@v1.0.1
        with:
          files: README.md # List of files to update
          prefix: "file-updater-for-release@" # Prefix before the version, default is: ""
          with-checkout: false # If you don't want to checkout the repo, default is: true
      
      - name: Push changes
        uses: EndBug/add-and-commit@v9
        with:
          committer_name: GitHub Actions
          committer_email: actions@github.com
          add: .
          message: 'update files'
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
