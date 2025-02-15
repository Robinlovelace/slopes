
<!-- README.md is generated from README.Rmd. Please edit that file -->

# slopes package

<!-- badges: start -->

[![R-CMD-check](https://github.com/ropensci/slopes/workflows/R-CMD-check/badge.svg)](https://github.com/ropensci/slopes/actions)
[![Codecov test
coverage](https://codecov.io/gh/ropensci/slopes/branch/master/graph/badge.svg)](https://codecov.io/gh/ropensci/slopes?branch=master)
[![Status at rOpenSci Software Peer
Review](https://badges.ropensci.org/420_status.svg)](https://github.com/ropensci/software-review/issues/420)
<!-- badges: end -->

The **slopes** R package calculates the slope (longitudinal steepness,
also known as gradient) of roads, rivers and other linear (simple)
features, based on two main inputs:

-   [vector](https://geocompr.robinlovelace.net/spatial-class.html#vector-data)
    linestring geometries defined by classes in the
    [`sf`](https://r-spatial.github.io/sf/) package
-   [raster](https://geocompr.robinlovelace.net/spatial-class.html#raster-data)
    objects with pixel values reporting average height, commonly known
    as digital elevation model (**DEM**) datasets, defined by classes in
    the [`raster`](https://cran.r-project.org/package=raster) or more
    recent [`terra`](https://rspatial.org/terra) packages

Data on slopes are useful in many fields of research, including
[hydrology](https://en.wikipedia.org/wiki/Stream_gradient), natural
hazards (including
[flooding](https://www.humanitarianresponse.info/fr/operations/afghanistan/infographic/afg-river-gradient-and-flood-hazard)
and [landslide risk
management](https://assets.publishing.service.gov.uk/media/57a08d0740f0b652dd0016f4/R7815-ADD017_col.pdf)),
recreational and competitive sports such as
[cycling](http://theclimbingcyclist.com/gradients-and-cycling-an-introduction/),
[hiking](https://trailism.com/trail-grades/), and
[skiing](https://www.snowplaza.co.uk/blog/16682-skiing-steeps-what-does-gradient-mean-ski-piste/).
Slopes are also also important in some branches of [transport and
emissions
modelling](https://www.sciencedirect.com/science/article/pii/S2352146516302642)
and [ecology](https://doi.org/10.1016/j.ncon.2016.10.001). See the
[`intro-to-slopes`
vignette](https://itsleeds.github.io/slopes/articles/intro-to-slopes.html)
for details on fields using slope data and the need for this package.

This README covers installation and basic usage. For more information
about slopes and how to use the package to calculate them, see the [get
started](https://itsleeds.github.io/slopes/) and the [introducion to
slopes](https://itsleeds.github.io/intro-to-slopes/) vignette.

## How it works

The package takes two main types of input data for slope calculation: -
vector geographic objects representing **linear features**, and -
**elevation values** from a digital elevation model representing a
continuous terrain surface or which can be downloaded using
functionality in the package

The package can be used with two sources of elevation data: - openly
available elevation data via an interface to the [ceramic
package](https://github.com/hypertidy/ceramic), enabling estimation of
hilliness for routes anywhere worldwide even when local elevation data
is lacking. The package takes geographic lines objects and returns
elevation data per vertex (providing the output as a 3D point geometry
in the sf package by default) and per line feature (providing average
gradient by default). - an elevation model, available on your machine.

## Getting started

### Installation

<!-- You can install the released version of slopes from [CRAN](https://CRAN.R-project.org) with: -->
<!-- ``` r -->
<!-- install.packages("slopes") -->
<!-- ``` -->

Install the development version from [GitHub](https://github.com/) with:

``` r
# install.packages("remotes")
remotes::install_github("ropensci/slopes")
```

#### Installation for DEM downloads

If you do not already have DEM data and want to make use of the
package’s ability to download them using the `ceramic` package, install
the package with suggested dependencies, as follows:

``` r
# install.packages("remotes")
remotes::install_github("ropensci/slopes", dependencies = "Suggests")
```

Furthermore, you will need to add a MapBox API key to be able to get DEM
datasets, by signing up and registering for a key at
<https://account.mapbox.com/access-tokens/> and then following these
steps:

``` r
usethis::edit_r_environ()
MAPBOX_API_KEY=xxxxx # replace XXX with your api key
```

## Basic examples

Load the package in the usual way. We will also load the `sf` library:

``` r
library(slopes)
library(sf)
```

The minimum input data requirement for using the package is an `sf`
object containing LINESTRING geometries, as illustrated below (requires
a MapBox API key):

``` r
sf_linestring = lisbon_route # import or load a linestring object
```

``` r
sf_linestring_xyz = elevation_add(sf_linestring)  # dem = NULL
#> Loading required namespace: ceramic
#> Preparing to download: 12 tiles at zoom = 12 from 
#> https://api.mapbox.com/v4/mapbox.terrain-rgb/
```

With the default argument `dem = NULL`, the function downloads the
necessary elevation information from Mapbox. You can also this use a
local digital elevation model (`dem = ...`), as shown in the example
below:

``` r
sf_linestring_xyz_local = elevation_add(sf_linestring, dem = dem_lisbon_raster)
```

In both cases you can obtain the average gradient of the linestring with
`slope_xyz()` and plot the elevation profile with `plot_slope()` as
follows:

``` r
slope_xyz(sf_linestring_xyz_local)
#>          1 
#> 0.07817098
plot_slope(sf_linestring_xyz_local)
```

<img src="man/figures/README-elevationprofile-1.png" width="100%" />

*See more functions in [Get
started](https://itsleeds.github.io/slopes/articles/slopes.html)
vignette.*

## See more in vignettes

-   [Get
    started](https://itsleeds.github.io/slopes/articles/slopes.html)
-   [An introduction to
    slopes](https://itsleeds.github.io/slopes/articles/intro-to-slopes.html)
-   [Reproducible example: gradients of a road network for a given
    city](https://itsleeds.github.io/slopes/articles/roadnetworkcycling.html)
-   [Verification of
    slopes](https://itsleeds.github.io/slopes/articles/verification.html)
-   [Benchmarking slopes
    calculation](https://itsleeds.github.io/slopes/articles/benchmark.html)

## Code of Conduct

Please note that the slopes project is released with a [Contributor Code
of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.
