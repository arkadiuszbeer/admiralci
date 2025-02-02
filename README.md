<!-- Please do not edit the README.md file as it is auto-generated. Only edit the README.Rmd file -->

# CI/CD Workflows for Admiral R Packages

This repository contains GitHub Actions continuous
integration/continuous delivery (CI/CD) workflows, most of which are
used by [`Admiral`](https://github.com/pharmaverse/admiral) and its
extensions extensions. Workflows defined here are responsible for
assuring high package quality standards without compromising
performance, security, or reproducibility.

Please refer to the [`.github/workflows`](.github/workflows) directory
to view the source code for the GitHub Actions workflows.

## What these workflows do?

Most workflows have a `BEGIN boilderplate steps` and
`END boilderplate steps` section within them which define some standard
steps required for installing system dependencies, R version and R
packages which serve as dependencies for the package.

The underlying mechanisms for installing R and Pandoc are defined in
[`r-lib/actions`](https://github.com/r-lib/actions), while the
installation of system dependencies and R package dependencies is
managed via the [Staged Dependencies GitHub
Action](https://github.com/marketplace/actions/staged-dependencies-action).
The latter is used in conjunction with the
[`staged_dependencies.yaml`](staged_dependencies.yaml) file in order to
install dependencies that are in the *same stage of development* as the
current package. You can read more about how it works
[here](https://github.com/openpharma/staged.dependencies). Note that the
latter is not necessary for this workflow to work and is completely
optional.

Following the installation of system dependencies, R, and package
dependencies, each workflow does something different.

### [`check-templates.yml`](./.github/workflows/check-templates.yml)

[![Check
Templates](https://github.com/pharmaverse/admiralci/actions/workflows/check-templates.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/check-templates.yml)

This workflow checks for issues within template scripts. For example, in
admiral package there are several template scripts with admiral-based
functions showing how to build certain ADaM datasets. As we update the
admiral functions, we want to make sure these template scripts execute
appropriately. Functions in the template scripts that are deprecated or
used inappropriately will cause this workflow to fail.

### [`code-coverage.yml`](./.github/workflows/code-coverage.yml)

[![Code
Coverage](https://github.com/pharmaverse/admiralci/actions/workflows/code-coverage.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/code-coverage.yml)

This workflow measures code coverage for unit tests and reports the code
coverage as a percentage of the *total number of lines covered by unit
tests* vs. the *total number of lines in the codebase*.

The [`covr`](https://covr.r-lib.org/) R package is used to calculate the
coverage.

Report summaries and badges for coverage are generated using a series of
other GitHub Actions.

For this workflow to execute successfully, you will need to create an
orphan branch called `badges` in your GitHub repository. You can do that
using the following steps:

    # Create orphan branch
    git checkout --orphan badges
    # Back up files
    mv .git /tmp/.git-backup
    # Remove everything else
    rm -rf * .*
    # Restore git files
    mv /tmp/.git-backup .git
    # Create a README file
    echo "# Badges" > README.md
    # Add, commit and push your new branch
    git add . && git commit -m "Init badges" && git push origin badges

### [`links.yml`](./.github/workflows/links.yml)

[![Check
URLs](https://github.com/pharmaverse/admiralci/actions/workflows/links.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/links.yml)

This workflow checks whether URLs embedded in code and documentation are
valid. Invalid URLs result in workflow failures. This workflow uses
[`lychee`](https://github.com/lycheeverse/lychee) to detect broken
links. Occasionally this check will detect false positives of urls that
look like urls. To remedy, please add this false positive to the
`.lycheeignore` file.

### [`lintr.yml`](./.github/workflows/lintr.yml)

[![Lint](https://github.com/pharmaverse/admiralci/actions/workflows/lintr.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/lintr.yml)

Static code analysis is performed by this workflow, which in turn uses
the [`lintr`](https://lintr.r-lib.org/) R package.

Any [`.lintr`](.lintr) configurations in the repository will be by this
workflow.

### [`man-pages.yml`](./.github/workflows/man-pages.yml)

[![Man
Pages](https://github.com/pharmaverse/admiralci/actions/workflows/man-pages.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/man-pages.yml)

This workflow checks if the manual pages in the `man/` directory of the
package are up-to-date with ROxygen comments in the code.

Workflow failures indicate that the manual pages are not up-to-date with
ROxygen comments, and corrective actions are provided in the workflow
log.

### [`pkgdown.yml`](./.github/workflows/pkgdown.yml)

[![Documentation](https://github.com/pharmaverse/admiralci/actions/workflows/pkgdown.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/pkgdown.yml)

Documentation for the R package is generated via this workflow. This
workflow uses the [`pkgdown`](https://pkgdown.r-lib.org/) framework to
generate documentation in HTML, and the HTML pages are deployed to the
`gh-pages` branch.

Moreover, an additional `Versions` dropdown is generated via the
[](https://github.com/marketplace/actions/r-pkgdown-multi-version-docs)
GitHub Action, so that an end user can view multiple versions of the
documentation for the package.

### [`r-cmd-check.yml`](./.github/workflows/r-cmd-check.yml)

[![R CMD
Check](https://github.com/pharmaverse/admiralci/actions/workflows/r-cmd-check.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/r-cmd-check.yml)

This workflow performs `R CMD check` for the package. Failed workflows
are typically indicative of problems encountered during the check, and
therefore an indication that the package does not meet quality
standards.

### [`r-pkg-validation.yml`](./.github/workflows/r-pkg-validation.yml)

[![R Package Validation
report](https://github.com/pharmaverse/admiralci/actions/workflows/r-pkg-validation.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/r-pkg-validation.yml)

When a new release of the package is made, this workflow executes to
create a validation report via
[theValidatoR](https://github.com/marketplace/actions/r-package-validation-report).
The PDF report is then attached to the release within GitHub.

### [`readme-render.yml`](./.github/workflows/readme-render.yml)

[![Render
README](https://github.com/pharmaverse/admiralci/actions/workflows/readme-render.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/readme-render.yml)

If your codebase uses a [`README.Rmd` file](README.Rmd) (like this
repository), then this workflow will automatically render a `README.md`
and commit it to your branch.

### [`spellcheck.yml`](./.github/workflows/spellcheck.yml)

[![Spelling](https://github.com/pharmaverse/admiralci/actions/workflows/spellcheck.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/spellcheck.yml)

Spellchecks are performed by this workflow, and the
[`spelling`](https://docs.ropensci.org/spelling/) R package is used to
detect spelling mistakes. Failed workflows typically indicate misspelled
words. In the `inst/WORDLIST` file, you can add words and or acronyms
that you want the spell check to ignore, for example *CDISC* is not an
English word but a common acronym used within Pharma. The workflow will
flag this until a user adds it to the `inst/WORDLIST`.

### [`style.yml`](./.github/workflows/style.yml)

[![Style](https://github.com/pharmaverse/admiralci/actions/workflows/style.yml/badge.svg)](https://github.com/pharmaverse/admiralci/actions/workflows/style.yml)

Code style is enforced via the [`styler`](https://styler.r-lib.org/) R
package. Custom style configurations, if any, will be honored by this
workflow.

Failed workflows are indicative of unstyled code.

## How to use these workflows?

### Reuse (recommended)

You could add just *one* file called `.github/workflows/common.yml` to
directly import these workflows while receiving the latest updates and
enhancements, given that the workflows defined in this repository are
reusable via the
[`workflow_call`](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
GitHub Actions event.

The contents of the `.github/workflows/common.yml` file are available in
the [`common.yml.inactive`](./.github/workflows/common.yml.inactive)
file in this repository. Feature flags in the form of `workflow_call`
inputs are available for customization purposes. Feature flags are
documented in the same file - look for the `env:` and `with:` hashes in
the file for feature flags.

### Copy as-is (not recommended)

Alternatively, if you want a high level of customization, you could
simply copy the workflows as-is from this repository to your repository
and modify them to your liking.

<!-- Begin links -->
<!-- End links -->
