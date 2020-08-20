
<!-- README.md is generated from README.Rmd. Please edit that file -->

# VISCtemplates

The goal of VISCtemplates is to:

  - automate project setup
  - provide a common, easy-to-understand directory structure across
    analyses
  - provide consistency across reports in the same project/protocol
  - provide consistency across reports analyzing data from the same
    assay

There are two main features of VISCtemplates:

1.  A function to automate directory/project set-up.
2.  A Rmarkdown template and functions to write Protocol Team (PT)
    reports.

## Installation

The package is available on the Fred Hutch organization GitHub page.

``` r
remotes::install_github("FredHutch/VISCtemplates")

# Use the build_vignettes parameter to access the vignette:
remotes::install_github("FredHutch/VISCtemplates", build_vignettes = TRUE)
vignette("using_pdf_and_word_template")
```

## Requirements

  - R (version \>= 3.0)
  - RStudio (version \>= 1.2)
      - Includes Pandoc (version \>= 2.0), which is needed for Word
        reports.
  - TinyTeX (or MiKTeX), which is needed for PDF reports.
      - `install.packages(“tinytex”)`
      - `tinytex::install_tinytex()`

## Setting up a VISC Analysis Project

### Step one: Create project locally

Use `create_visc_project()` to specify where to save your analysis
project. The function will set up a basic directory structure for your
analysis that follow the directory structure WI (WI-xxxx).

The directory name should match the CAVD name with “Analysis” at the
end: `VDCnnnAnalysis`. For example:

``` r
create_visc_project("H:/visc-projects/Gallo477Analysis")
```

This function will produce the following structure and templates:

    [VDCnnn]Analysis
    |- .github/
    |  +- pull_request_template.md   # template for GitHub PRs
    |
    |- .gitignore                     # File types that VISC ignores
    |- .Rbuildignore                  # Auto-generated for package development (ignore)
    |- DESCRIPTION                    # Project metadata and dependencies 
    |
    |- docs/
    |  +- background.Rmd              # Backround section of PT report
    |  +- group-colors.R              # Used to assign group colors across projects
    |  +- objectives.Rmd              # Objectives section of PT report
    |  +- presentations/              # Protocol presentations
    |  +- study-schema.R              # Used to generate table with study design
    |
    |- inst/WORDLIST                  # A lists of words that will be ignored during spell check
    |
    |- man/                           # Documentation of R functions generated by Roxygen
    |
    |- NAMESPACE                      # Auto-generated for package development tools (ignore)
    |
    |- R/                             # R functions
    |
    |- README.Rmd                     # top-level description of content and guide to users
    |
    |- .VDCnnnAnalysis.Rproj          # R project file

### Step Two: GitHub

Once you’ve created the project locally, initialize Git and push to
github.com/FredHutch:

``` r
usethis::use_git()

# this will create a project on GitHub, push your work, and open the webpage!
Sys.getenv("GITHUB_PAT")
usethis::use_github(organisation = "FredHutch", private = TRUE, protocol = "https")
```

Note that you must have a [personal access token
(PAT)](https://happygitwithr.com/github-pat.html) set up to do this.

You still need to set the permissions to the project to vidd-visc:

Settings → Manage access → Invite teams or people (button) → vidd-visc
(shows up as FredHutch/vidd-visc)

### Step Three: Documentation

Now you can begin editing the Documentation.

1.  README.Rmd: Open README template and add things like documentation
    of SST.
2.  docs/: Begin filling in group colors, background, and objectives.
3.  sap/: Add statistical analysis plan.

### Step Four: Start Analysis Work

The above setup steps are done on the `master` branch. When you’re ready
to begin work on the analysis, create a branch.

Then start working from the PT report template. You can use the template
from this package:

``` r
# create a top-level directory if >1 report per assay
usethis::use_directory("BAMA")

rmarkdown::draft(
  "BAMA/BAMA_IgG_pt_report.Rmd", 
  template = "visc_report", 
  package = "VISCtemplates", 
  edit = FALSE
  )
```

Alternatively, you could set this up using the RStudio menus:

New File → R markdown → From Template → Visc Report (PDF & Word Output)

To knit the report, click the small down arrow to the right of the
“Knit” button:

  - For PDF, select “visc\_pdf\_document”.
  - For Word (.docx), select “visc\_word\_document”.

## Spell Check

When the template is generated, it creates a list of custom words that
are frequently ignored in VISC reports. This is saved in
`inst/WORDLIST`. You can check spelling of files and update `WORDLIST`
using the [`spelling` package](https://docs.ropensci.org/spelling/). The
`spelling` package only spell checks text blocks, not code chunks.

``` r
install.packages("spelling")

library(spelling)

spell_check_files("BAMA/pt_report/BAMA_pt_report.Rmd")

spell_check_files("BAMA/pt_report/BAMA_pt_report.pdf") # this is helpful for spell checking captions
```

## Why use a R package structure?

The R package structure is used for analysis projects because it
provides an easily recognizable format for file organization and allows
for the use of packages like devtools and roxygen2. For more
information, see:

1.  Ben Marwick, Carl Boettiger, and Lincoln Mullen. Paper: [Packaging
    data analytical work reproducibily using R (and
    friends)](https://peerj.com/preprints/3192/)
2.  Karthik Ram. Rstudio::conf 2019 talk: [How To Make Your Data
    Analysis Notebooks More
    Reproducible](https://github.com/karthik/rstudio2019)
3.  rOpenSci Community Call: [Reproducible Research with
    R](https://ropensci.org/commcalls/2019-07-30/)
