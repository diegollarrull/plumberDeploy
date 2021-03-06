
<!-- README.md is generated from README.Rmd. Please edit that file -->

# plumberDeploy

<!-- badges: start -->

[![Travis build
status](https://travis-ci.com/muschellij2/plumberDeploy.svg?branch=master)](https://travis-ci.com/muschellij2/plumberDeploy)
[![AppVeyor build
status](https://ci.appveyor.com/api/projects/status/github/muschellij2/plumberDeploy?branch=master&svg=true)](https://ci.appveyor.com/project/muschellij2/plumberDeploy)
[![R build
status](https://github.com/muschellij2/plumberDeploy/workflows/R-CMD-check/badge.svg)](https://github.com/muschellij2/plumberDeploy/actions)
[![R build
status](https://github.com/meztez/plumberDeploy/workflows/R-CMD-check/badge.svg)](https://github.com/meztez/plumberDeploy/actions)
<!-- badges: end -->

A package to deploy plumber APIs.

## How to use

First create a Digital Ocean account. Validate using
`analogsea::account()`.

Then configure an ssh key for the Digital Ocean account before using
methods included in this package. Use `analogsea::key_create` method or
see
<https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/to-account/>.

Same ssh key needs to be available on the local machine too. Check
availability with `ssh::ssh_key_info()`. Validate that one of the public
keys can be found in `lapply(analogsea::keys(), '[[', "public_key")`.

## Deploy an api to a new droplet

`.api/plumber.R`

``` r
#* @get /
function() {
  Sys.Date()
}
```

Then run this code

``` r
id <- plumberDeploy::do_provision(example = FALSE)
# About 10 minutes
# STOP Make sure every packages the api depends on is available on the droplet, see below for other commands.
plumberDeploy::do_deploy_api(id, "date", "./api/", 8000, docs = TRUE)
```

Navigate to: `[[IPADDRESS]]/date/__docs__/`

## Other useful commands

``` r
# Install package to your droplet
analogsea::install_r_package(droplet, c("readr", "remotes"))
# Install system dependencies to your droplet
analogsea::debian_apt_get_install(droplet, "libssl-dev","libsodium-dev", "libcurl4-openssl-dev")
```

R Packages installed on linux systems sometimes require system packages.
Check console output carefully to see if a package was installed
successfully.

Otherwise read the doc on functions, ask questions in the [RStudio
community](https://community.rstudio.com/) or report issues to our
github issue tracker.
