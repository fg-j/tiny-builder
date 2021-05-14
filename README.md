# Automating builder release with buildpackless builder
There are two mainline branches in this repo:
- `main`
- `buildpackless`

The `buildpackless` branch is an orphan branch and holds the `buildpackless-builder.toml` file and some necessary workflows/config.

This repo uses [Release Drafter](https://github.com/release-drafter/release-drafter) instead of the Paketo Create Draft Release action because it's configurable to handle automatically publishing releases for two separate version lines.
- [This config file](https://github.com/fg-j/tiny-builder/blob/3779e5158f1f0c7398f41f5227595446aa5059e2/.github/release-drafter.yml) determines the release versioning, tagging, and formatting for the standard builder. It's the input to [this workflow](https://github.com/fg-j/tiny-builder/blob/3779e5158f1f0c7398f41f5227595446aa5059e2/.github/workflows/release-drafter.yml).
    - Note `filter-by-commitish: true` and `commitish: main`. This allows the release drafter to determine the next release version only by considering previous releases tagged on the `main` branch
- [This config file](https://github.com/fg-j/tiny-builder/blob/3779e5158f1f0c7398f41f5227595446aa5059e2/.github/release-drafter-buildpackless.yml) determines the release versioning, tagging, and formatting for the buildpackless builder. It's the input to [this workflow](https://github.com/fg-j/tiny-builder/blob/67924a7be6a3fe0f7e9da12155b6ff3275a9834a/.github/workflows/release-drafter.yml), which must be **on the `buildpackless` branch**.
    - Note `filter-by-commitish: true` and `commitish: buildpackless`. This allows the release drafter to determine the next release version only by considering previous releases tagged on the `buildpackless` branch.

Note also that this repo has different `push-image.yml` workflows on the `main` and `buildpackless` branches. These trigger on releases to the respective target branches.

One thing to notice is that the sort order of the [Releases](https://github.com/fg-j/tiny-builder/releases) page always shows the standard builder releases first. [Github sorts releases](https://github.community/t/release-page-not-sorting-correctly/121002/2) based on tag time, then by SemVer of tags, and then alphabetically by tag name. To get a better Release page ordering, one could tag the `buildpackless` branch with `v1.2.3-buildpackless` or `v1.2.3/buildpackless` instead of `buildpackless/v1.2.3`. The tag template is [determined by the release drafter config](https://github.com/fg-j/tiny-builder/blob/3779e5158f1f0c7398f41f5227595446aa5059e2/.github/release-drafter-buildpackless.yml#L3). 
