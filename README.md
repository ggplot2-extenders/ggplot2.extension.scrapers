
- [2. `yaml::read_yaml` and `httr2` to parse extension gallery
  `gallery _config.yml` file 148
  packages](#2-yamlread_yaml-and-httr2-to-parse-extension-gallery-gallery-_configyml-file-148-packages)
- [1. `tools::CRAN_package_db` that are `^gg|^GG|gg$|GG$` or found in
  gallery w/ ggplot2 depend or import (242
  packages)](#1-toolscran_package_db-that-are-gggggggg-or-found-in-gallery-w-ggplot2-depend-or-import-242-packages)
- [3. `gh::gh` Keep in mind in terms: github
  contributors](#3-ghgh-keep-in-mind-in-terms-github-contributors)
- [4. `universe::global_search` with exported function pattern
  identification](#4-universeglobal_search-with-exported-function-pattern-identification)
- [5. `pkgdiff` to look at patterns
  also?](#5-pkgdiff-to-look-at-patterns-also)

<!-- README.md is generated from README.Rmd. Please edit that file -->

1.  [universe_ggplot2_depends_function_exports.csv](https://raw.githubusercontent.com/ggplot2-extenders/ggplot2-extension-scrapers/refs/heads/main/universe_ggplot2_depends_function_exports.csv)
2.  [gg_w_ggplot2_depends_or_imports_cran.csv](https://raw.githubusercontent.com/ggplot2-extenders/ggplot2-extension-scrapers/refs/heads/main/gg_w_ggplot2_depends_or_imports_cran.csv)
3.  [gg_extension_pkgs_gallery.csv](https://raw.githubusercontent.com/ggplot2-extenders/ggplot2-extension-scrapers/refs/heads/main/gg_extension_pkgs_gallery.csv)

The repo contains code to characterize the ggplot2 extension ecosystem.
A couple of projects motivate this:

- [CRAN task views grammar of graphics (or ggplot2
  extension)](https://github.com/ggplot2-extenders/ggplot-extension-club/discussions/82)
- [JSM ‘Who are the ggplot2
  extenders?’](https://evamaerey.github.io/ggplot2-extension-ecosystem/)

``` r
devtools::create(".")
```

# 2. `yaml::read_yaml` and `httr2` to parse extension gallery `gallery _config.yml` file 148 packages

Code/ideas: Pepijn, Joyce, Gina, (Probably others)

``` r
gg_gallery_pkgs <-
  "https://raw.githubusercontent.com/ggplot2-exts/gallery/refs/heads/gh-pages/_config.yml" |>
  httr2::request() |>
  httr2::req_perform() |>
  httr2::resp_body_string() |>
  (\(x) yaml::read_yaml(text = x))() |>
  _$widgets |>
  dplyr::bind_rows() |>
  dplyr::rename(package = name)

gg_gallery_pkgs |> tibble::tibble()
#> # A tibble: 148 × 10
#>    package  thumbnail url   ghuser ghrepo tags  cran  ghauthor short description
#>    <chr>    <chr>     <chr> <chr>  <chr>  <chr> <lgl> <chr>    <chr> <chr>      
#>  1 ggQQunif images/g… http… rcorty ggQQu… visu… TRUE  rcorty   "Mak… "For \"big…
#>  2 ggupset  images/g… http… const… ggups… visu… TRUE  const-ae "Com… "Replace t…
#>  3 xmrr     images/x… http… Zanid… xmrr   XmR,… TRUE  Alex Za… "Gen… "Use xmr()…
#>  4 gg3D     images/g… http… Acker… gg3D   3D, … FALSE Daniel … "3D … "gg3D adds…
#>  5 ggQC     images/g… http… kenit… ggQC   QC, … TRUE  Kenith … "Use… "ggQC is u…
#>  6 ggdist   images/g… http… mjskay ggdist visu… TRUE  mjskay   "'gg… "'ggdist' …
#>  7 ggedit   images/g… http… metru… ggedit visu… TRUE  yonicd   "gge… "ggedit is…
#>  8 ggpage   images/g… http… emilh… ggpage visu… TRUE  emilhvi… "Cre… "Facilitat…
#>  9 ggpca    images/g… http… Yaoxi… ggpca  visu… TRUE  Yaoxian… "Pro… "`ggpca` o…
#> 10 ggbreak  images/g… http… YuLab… ggbre… visu… TRUE  YuLab-S… "Set… "An implem…
#> # ℹ 138 more rows
gg_gallery_pkgs$package
#>   [1] "ggQQunif"             "ggupset"              "xmrr"                
#>   [4] "gg3D"                 "ggQC"                 "ggdist"              
#>   [7] "ggedit"               "ggpage"               "ggpca"               
#>  [10] "ggbreak"              "ggimg"                "gganatogram"         
#>  [13] "ggforce"              "ggalt"                "ggiraph"             
#>  [16] "ggmuller"             "ggstance"             "ggrepel"             
#>  [19] "ggraph"               "gginnards"            "ggpp"                
#>  [22] "ggpmisc"              "geomnet"              "ggExtra"             
#>  [25] "ggfortify"            "autoplotly"           "gganimate"           
#>  [28] "ggfx"                 "plotROC"              "ggbump"              
#>  [31] "ggthemes"             "ggspectra"            "ggstatsplot"         
#>  [34] "ggnetwork"            "ggtech"               "ggradar"             
#>  [37] "ggx"                  "ggTimeSeries"         "ggtree"              
#>  [40] "ggseas"               "ggsci"                "ggmosaic"            
#>  [43] "survminer"            "ggeasy"               "ggside"              
#>  [46] "ggcorrplot"           "ggpubr"               "ggthemr"             
#>  [49] "GGally"               "ggseqlogo"            "ggChernoff"          
#>  [52] "ggridges"             "lemon"                "cowplot"             
#>  [55] "qqplotr"              "ggalluvial"           "patchwork"           
#>  [58] "ggquiver"             "ggsignif"             "ggdag"               
#>  [61] "ggformula"            "ggbeeswarm"           "ggperiodic"          
#>  [64] "ggpol"                "ggpirate"             "esquisse"            
#>  [67] "ggdark"               "sugrrants"            "tvthemes"            
#>  [70] "ggfittext"            "ggparty"              "gggenes"             
#>  [73] "gggenomes"            "treemapify"           "lindia"              
#>  [76] "gghalves"             "ggrastr"              "ggpointdensity"      
#>  [79] "ggsom"                "ggnewscale"           "ggh4x"               
#>  [82] "ggarrow"              "legendry"             "ggcharts"            
#>  [85] "humapr"               "ggshadow"             "ggseg"               
#>  [88] "mdthemes"             "ggwordcloud"          "ggasym"              
#>  [91] "gglorenz"             "hrbrthemes"           "ggpattern"           
#>  [94] "ggtext"               "calendR"              "ggip"                
#>  [97] "gglm"                 "econocharts"          "ComplexUpset"        
#> [100] "ggchromatic"          "ggheatmap"            "see"                 
#> [103] "directlabels"         "ggHoriPlot"           "ggtrace"             
#> [106] "ggESDA"               "geomtextpath"         "ggdensity"           
#> [109] "ggtranscript"         "piecepackr"           "oblicubes"           
#> [112] "ggDoubleHeat"         "nflplotR"             "ggbraid"             
#> [115] "ggblanket"            "ggpie"                "ggstar"              
#> [118] "ggarchery"            "tidyterra"            "ggseqplot"           
#> [121] "ggsurvfit"            "ggsector"             "ggterror"            
#> [124] "ggragged"             "ggmapinset"           "ggmagnify"           
#> [127] "ggblend"              "ggflowchart"          "ggrain"              
#> [130] "ggoutlierscatterplot" "ggautothemes"         "AMR"                 
#> [133] "ichimoku"             "eheat"                "ggstats"             
#> [136] "ggfoundry"            "ggalign"              "ggreveal"            
#> [139] "geofacet"             "tidyplots"            "rphylopic"           
#> [142] "deeptime"             "ggpcp"                "ggvolcano"           
#> [145] "ggfootball"           "ggfields"             "ggsankeyfier"        
#> [148] "ggpath"

usethis::use_data(gg_gallery_pkgs, overwrite = T)
```

# 1. `tools::CRAN_package_db` that are `^gg|^GG|gg$|GG$` or found in gallery w/ ggplot2 depend or import (242 packages)

Code/ideas: June, Joyce, Pepijn, Gina

``` r
cran_gg_w_ggplot2_depends_or_imports <- tools::CRAN_package_db() |> 
  dplyr::filter(
    stringr::str_detect(Package, "^gg|^GG|gg$|GG$") | 
      Package %in% gg_gallery_pkgs$package,
    stringr::str_detect(Depends, "ggplot2") | 
      stringr::str_detect(Imports, "ggplot2")
  ) |>
  dplyr::mutate(ind_gallery = Package %in% gg_gallery_pkgs$package) |>
  janitor::clean_names()

cran_gg_w_ggplot2_depends_or_imports |> tibble::tibble() 
#> # A tibble: 243 × 70
#>    package version priority depends imports linking_to suggests enhances license
#>    <chr>   <chr>   <chr>    <chr>   <chr>   <chr>      <chr>    <chr>    <chr>  
#>  1 autopl… 0.1.4   <NA>     R (>= … "metho… <NA>       "testth… <NA>     GPL-2  
#>  2 calendR 1.2     <NA>     <NA>    "ggplo… <NA>        <NA>    <NA>     GPL-2  
#>  3 Comple… 1.3.3   <NA>     <NA>    "ggplo… <NA>       "testth… <NA>     MIT + …
#>  4 cowplot 1.1.3   <NA>     R (>= … "ggplo… <NA>       "Cairo,… <NA>     GPL-2  
#>  5 deepti… 2.1.0   <NA>     R (>= … "deept… <NA>       "geomte… <NA>     GPL (>…
#>  6 egg     0.4.5   <NA>     gridEx… "gtabl… <NA>       "knitr,… <NA>     GPL-3  
#>  7 esquis… 2.1.0   <NA>     <NA>    "bslib… <NA>       "office… <NA>     GPL-3 …
#>  8 geofac… 0.2.1   <NA>     R (>= … "ggplo… <NA>       "sf, te… <NA>     MIT + …
#>  9 geomte… 0.1.5   <NA>     ggplot… "grid,… <NA>       "testth… <NA>     MIT + …
#> 10 gg.gap  1.3     <NA>     <NA>    "ggplo… <NA>        <NA>    <NA>     GPL-3  
#> # ℹ 233 more rows
#> # ℹ 61 more variables: license_is_foss <chr>, license_restricts_use <chr>,
#> #   os_type <chr>, archs <chr>, md5sum <chr>, needs_compilation <chr>,
#> #   additional_repositories <chr>, author <chr>, authors_r <chr>, biarch <chr>,
#> #   bug_reports <chr>, build_keep_empty <chr>, build_manual <chr>,
#> #   build_resave_data <chr>, build_vignettes <chr>, built <chr>,
#> #   byte_compile <chr>, classification_acm <chr>, …
names(cran_gg_w_ggplot2_depends_or_imports) 
#>  [1] "package"                 "version"                
#>  [3] "priority"                "depends"                
#>  [5] "imports"                 "linking_to"             
#>  [7] "suggests"                "enhances"               
#>  [9] "license"                 "license_is_foss"        
#> [11] "license_restricts_use"   "os_type"                
#> [13] "archs"                   "md5sum"                 
#> [15] "needs_compilation"       "additional_repositories"
#> [17] "author"                  "authors_r"              
#> [19] "biarch"                  "bug_reports"            
#> [21] "build_keep_empty"        "build_manual"           
#> [23] "build_resave_data"       "build_vignettes"        
#> [25] "built"                   "byte_compile"           
#> [27] "classification_acm"      "classification_acm_2012"
#> [29] "classification_jel"      "classification_msc"     
#> [31] "classification_msc_2010" "collate"                
#> [33] "collate_unix"            "collate_windows"        
#> [35] "contact"                 "copyright"              
#> [37] "date"                    "date_publication"       
#> [39] "description"             "encoding"               
#> [41] "keep_source"             "language"               
#> [43] "lazy_data"               "lazy_data_compression"  
#> [45] "lazy_load"               "mailing_list"           
#> [47] "maintainer"              "note"                   
#> [49] "packaged"                "rd_macros"              
#> [51] "staged_install"          "sys_data_compression"   
#> [53] "system_requirements"     "title"                  
#> [55] "type"                    "url"                    
#> [57] "use_lto"                 "vignette_builder"       
#> [59] "zip_data"                "path"                   
#> [61] "x_cran_comment"          "published"              
#> [63] "reverse_depends"         "reverse_imports"        
#> [65] "reverse_linking_to"      "reverse_suggests"       
#> [67] "reverse_enhances"        "deadline"               
#> [69] "doi"                     "ind_gallery"
cran_gg_w_ggplot2_depends_or_imports$package |> sort()
#>   [1] "autoplotly"          "calendR"             "ComplexUpset"       
#>   [4] "cowplot"             "deeptime"            "egg"                
#>   [7] "esquisse"            "geofacet"            "geomtextpath"       
#>  [10] "gg.gap"              "gg1d"                "ggalign"            
#>  [13] "ggaligner"           "ggalignment"         "ggallin"            
#>  [16] "ggalluvial"          "GGally"              "ggalt"              
#>  [19] "gganimate"           "ggarchery"           "ggarrow"            
#>  [22] "ggautomap"           "ggbeeswarm"          "ggbiplot"           
#>  [25] "ggblanket"           "ggblend"             "ggborderline"       
#>  [28] "ggbrace"             "ggbrain"             "ggbreak"            
#>  [31] "ggbrick"             "ggBubbles"           "ggbuildr"           
#>  [34] "ggbump"              "ggchangepoint"       "ggcharts"           
#>  [37] "ggChernoff"          "ggcleveland"         "ggcompare"          
#>  [40] "ggcorrplot"          "ggcorset"            "ggdag"              
#>  [43] "ggdark"              "ggdaynight"          "ggdemetra"          
#>  [46] "ggdendro"            "ggdensity"           "ggdist"             
#>  [49] "ggdmc"               "ggDoE"               "ggDoubleHeat"       
#>  [52] "ggeasy"              "GGEBiplots"          "ggedit"             
#>  [55] "ggenealogy"          "ggESDA"              "ggetho"             
#>  [58] "ggExtra"             "ggfacto"             "ggfields"           
#>  [61] "ggfigdone"           "ggFishPlots"         "ggfittext"          
#>  [64] "ggfixest"            "ggflowchart"         "ggfocus"            
#>  [67] "ggfootball"          "ggforce"             "ggformula"          
#>  [70] "ggfortify"           "ggfoundry"           "ggfun"              
#>  [73] "ggfx"                "gggap"               "gggenes"            
#>  [76] "gggenomes"           "ggghost"             "gggibbous"          
#>  [79] "gggrid"              "ggh4x"               "gghalfnorm"         
#>  [82] "gghalves"            "gghdx"               "ggheatmap"          
#>  [85] "gghighlight"         "gghilbertstrings"    "gghist"             
#>  [88] "ggHoriPlot"          "gghourglass"         "ggimage"            
#>  [91] "ggimg"               "gginference"         "gginnards"          
#>  [94] "ggip"                "ggiraph"             "ggiraphExtra"       
#>  [97] "ggisotonic"          "gglgbtq"             "gglm"               
#> [100] "gglogger"            "gglorenz"            "ggmap"              
#> [103] "ggmapcn"             "ggmapinset"          "ggmatplot"          
#> [106] "ggmcmc"              "ggmice"              "GGMncv"             
#> [109] "GGMnonreg"           "ggmosaic"            "ggmugs"             
#> [112] "ggmuller"            "ggmulti"             "ggnetwork"          
#> [115] "ggnewscale"          "ggnormalviolin"      "ggnuplot"           
#> [118] "ggOceanMaps"         "ggokabeito"          "ggordiplots"        
#> [121] "GGoutlieR"           "ggpackets"           "ggpage"             
#> [124] "ggparallel"          "ggparty"             "ggpath"             
#> [127] "ggpattern"           "ggpca"               "ggpcp"              
#> [130] "ggperiodic"          "ggpicrust2"          "ggpie"              
#> [133] "ggplate"             "ggplot.multistats"   "ggplot2.utils"      
#> [136] "ggplotAssist"        "ggplotgui"           "ggplotify"          
#> [139] "ggpmisc"             "ggPMX"               "ggpointdensity"     
#> [142] "ggpointless"         "ggpol"               "ggpolar"            
#> [145] "ggpolypath"          "ggpp"                "ggprism"            
#> [148] "ggpubr"              "ggpval"              "ggQC"               
#> [151] "ggQQunif"            "ggquickeda"          "ggquiver"           
#> [154] "ggragged"            "ggrain"              "ggRandomForests"    
#> [157] "ggraph"              "ggraptR"             "ggrasp"             
#> [160] "ggrastr"             "ggrcs"               "ggredist"           
#> [163] "ggrepel"             "ggResidpanel"        "ggreveal"           
#> [166] "ggridges"            "ggrisk"              "ggrounded"          
#> [169] "ggRtsy"              "ggsankeyfier"        "ggScatRidges"       
#> [172] "ggsci"               "ggscidca"            "ggseas"             
#> [175] "ggsector"            "ggseg"               "ggsegmentedtotalbar"
#> [178] "ggsem"               "ggseqlogo"           "ggseqplot"          
#> [181] "ggshadow"            "ggside"              "ggsignif"           
#> [184] "ggsmc"               "ggsoccer"            "ggsolvencyii"       
#> [187] "ggsom"               "ggspark"             "ggspatial"          
#> [190] "ggspectra"           "ggstackplot"         "ggstance"           
#> [193] "ggstar"              "ggstats"             "ggstatsplot"        
#> [196] "ggstream"            "ggstudent"           "ggsurveillance"     
#> [199] "ggsurvey"            "ggsurvfit"           "ggswissmaps"        
#> [202] "ggtangle"            "ggtaxplot"           "ggtea"              
#> [205] "ggtern"              "ggtext"              "ggThemeAssist"      
#> [208] "ggthemes"            "ggthemeUL"           "ggtibble"           
#> [211] "ggtikz"              "ggTimeSeries"        "ggtrace"            
#> [214] "ggtrendline"         "ggtricks"            "ggupset"            
#> [217] "ggvenn"              "ggVennDiagram"       "ggvfields"          
#> [220] "ggview"              "ggvolcano"           "ggwordcloud"        
#> [223] "ggx"                 "granovaGG"           "hrbrthemes"         
#> [226] "ichimoku"            "legendry"            "lemon"              
#> [229] "lindia"              "nflplotR"            "patchwork"          
#> [232] "plotROC"             "qqplotr"             "rphylopic"          
#> [235] "see"                 "sugrrants"           "survminer"          
#> [238] "tablesgg"            "tidyplots"           "tidyterra"          
#> [241] "treemapify"          "tvthemes"            "xmrr"

usethis::use_data(cran_gg_w_ggplot2_depends_or_imports, overwrite = T)
```

``` r
# devtools::check()
# devtools::install(".", upgrade = "never")
```

``` r
knitr::opts_chunk$set(eval= F)
```

# 3. `gh::gh` Keep in mind in terms: github contributors

“Re finding GitHub users from packages, the repo owner is not always the
only/main contributor, especially if it’s an org”

Code/ideas: Carl Suster @arcresu

<https://github.com/ggplot2-extenders/ggplot-extension-club/discussions/82#discussioncomment-12469510>

``` r
gh_contributors <- function(repo = "cidm-ph/ggmapinset") {
  resp <- gh::gh("GET /repos/{repo}/contributors", repo = repo)
  total_contributions <- sum(sapply(resp, \(x) x$contributions))
  resp <- Filter(\(x) x$type == "User", resp) # exclude bots
  resp <- Filter(\(x) x$contributions > total_contributions/5, resp) # with at least 20% of contributions
  sapply(resp, \(x) stringr::str_trim(x$login))
}

gh_contributors("cidm-ph/ggmapinset")
gh_contributors("YuLab-SMU/ggfun")
gh_contributors("tidyverse/ggplot2")
gh_contributors("AllanCameron/geomtextpath")
```

# 4. `universe::global_search` with exported function pattern identification

Teun
<https://github.com/ggplot2-extenders/ggplot-extension-club/discussions/82#discussioncomment-12479880>

``` r
# install.packages("universe", repos = "https://ropensci.r-universe.dev")

# I'm aware there should be ~7k/8k packages with ggplot2 as dependency.
packages <- universe::global_search(query = 'needs:ggplot2', limit = 10000L)
out_file <- "universe_ggplot2_depends_function_exports.csv"

# Ensure I have a 'data' folder with the file I'll need
if (!fs::file_exists(out_file)) {
    dir <- fs::path_dir(out_file)
    if (!fs::dir_exists(dir)) {
        fs::dir_create(dir)
    }
    fs::file_create(out_file)
}

# Read current data if it is cached
current_data <- data.table::fread(out_file)

data <- lapply(packages$results, function(result) {

    name <- result$Package
    universe <- result$`_user`

    # We're going to skip this package if we've already seen it. Potentially,
    # we'd be skipping packages with duplicate names, but that shouldn't occur
    # too often.
    if (name %in% current_data$name) {
        return()
    }

    # The information we want, the exported functions, is not available in
    # the results we already have. We need to a package specific query
    # to get the `_exports` field
    details <- universe::universe_one_package(universe, package = name)
    exports <- unlist(details$`_exports`) %||% NA_character_

    # Format as data.frame
    df <- data.frame(
        name = name,
        universe = universe,
        export = exports
    )

    # Write to file directly. Combined with the skip mechanism above, we're
    # effectively caching every result
    data.table::fwrite(df, out_file, append = TRUE)
})


library(dplyr)
library(ggplot2)
library(scales)

file <- "universe_ggplot2_depends_function_exports.csv"

data <- data.table::fread(file) |>
    filter(nzchar(export)) |>
    filter(!startsWith(name, "RcmdrPlugin")) |>
    mutate(class = case_when(
        startsWith(export, "geom_")     ~ "geom",
        startsWith(export, "stat_")     ~ "stat",
        startsWith(export, "scale_")    ~ "scale",
        startsWith(export, "coord_")    ~ "coord",
        startsWith(export, "facet_")    ~ "facet",
        startsWith(export, "guide_")    ~ "guide",
        startsWith(export, "position_") ~ "position",
        startsWith(export, "draw_key_") ~ "key",
        startsWith(export, "element_")  ~ "element",
        startsWith(export, "theme_")    ~ "theme",
        startsWith(export, "Geom")      ~ "Geom class",
        startsWith(export, "Stat")      ~ "Stat class",
        startsWith(export, "Scale")     ~ "Scale class",
        startsWith(export, "Coord")     ~ "Coord class",
        startsWith(export, "Facet")     ~ "Facet class",
        startsWith(export, "Guide")     ~ "Guide class",
        startsWith(export, "Position")  ~ "Position class",
        .default = ""
    )) |>
    mutate(pattern = case_when(
        startsWith(name, "gg")   ~ "gg-prefix",
        startsWith(name, "tidy") ~ "tidy-prefix",
        endsWith(name, "themes") ~ "themes-suffix",
        .default = ""
    ))


universe_gg_depends_function_exports <- readr::read_csv("universe_ggplot2_depends_function_exports.csv")

usethis::use_data()
```

# 5. `pkgdiff` to look at patterns also?

Pedro Aphalo

<https://github.com/ggplot2-extenders/ggplot-extension-club/discussions/82#discussioncomment-12582326>

``` r
library(pkgdiff)
library(lubridate)
library(dplyr)



# rm(list = ls(pattern = "*"))

pkg_stability_row <- function(pkg, releases = NULL, months = NULL) {
  temp <- pkg_stability(pkg = pkg, releases = releases, months = months)
  temp.df <- as.data.frame(temp[c(1:7)])
  temp.df$num.funs <- temp$StabilityData$TF[1L]
  temp.df$Size <- temp$StabilityData$Size[1L]
  temp.df
}

pkg_gg_functions <- function(pkg) {
  gg.funs <- list(PackageName = pkg)
  temp <- pkg_info(pkg = pkg, ver = "latest") # latest in CRAN! (ignores local)
  fun.names <- unique(names(temp$Functions))
  gg.funs$num.geoms <- sum(grepl("^geom_", fun.names))
  gg.funs$num.stats <- sum(grepl("^stat_", fun.names))
  gg.funs$num.scales <- sum(grepl("^scale_", fun.names))
  gg.funs$num.positions <- sum(grepl("^position_", fun.names))
  gg.funs$num.coords <- sum(grepl("^coord_", fun.names))
  gg.funs$num.drawkeys <- sum(grepl("^draw_key_", fun.names))
  gg.funs$num.guides <- sum(grepl("^guide_", fun.names))
  gg.funs$num.labellers <- sum(grepl("^label_", fun.names))
  gg.funs$num.themes <- sum(grepl("^theme_", fun.names))
  gg.funs$num.theme.elements <- sum(grepl("^element_", fun.names))
  gg.funs$num.ggplots <- sum(grepl("^ggplot", fun.names))
  gg.funs$num.autoplots <- sum(grepl("^autoplot", fun.names))
  gg.funs$num.autolayers <- sum(grepl("^autolayer", fun.names))
  as.data.frame(gg.funs)
}

csv <- "https://raw.githubusercontent.com/ggplot2-extenders/ggplot2-extensions-cran-task-view/refs/heads/main/gg-pkgs.csv"

# downloaded previously from GitHub
gg_pkgs_list.df <- read.csv(csv)
gg_packages <- gg_pkgs_list.df$Package
length(gg_packages)

# 'pkgdiff' gets package data from CRAN
cran_pkgs <- available.packages(repos = c(CRAN = "https://cran.rstudio.com/"))
cran_pkgs <- cran_pkgs[ , "Package"]

gg_packages <- intersect(gg_packages, cran_pkgs)
length(gg_packages)

## run only if cached
# chached_packages <- pkg_cache()
# gg_packages <- intersect(na.omit(chached_packages$Package), gg_packages)
# length(gg_packages)

# even fewer packages for testing
# gg_packages <- gg_packages[1:5]

# all work lost if functions fail to return a value
# stability.ls <- lapply(gg_packages, pkg_stability_row)
# stability.df <- bind_rows(stability.ls)
# 
# functions.ls <- lapply(gg_packages, pkg_gg_functions)
# functions.df <- bind_rows(functions.ls)

# use a for loop instead so that results are not all lost when the function errors.
# It can take quite a long time to run.

if (!exists("stability.ls")) stability.ls <- list()
if (!exists("functions.ls")) functions.ls <- list()
pkgs_done <- intersect(names(stability.ls), names(functions.ls))
pkgs_to_do <- setdiff(gg_packages, pkgs_done)
length(pkgs_to_do)

for (pkg in pkgs_to_do) {
  temp1 <- pkg_stability_row(pkg = pkg)
  if (nrow(temp1)) {
    stability.ls[[pkg]] <- temp1
  }
  temp2 <- pkg_gg_functions(pkg = pkg)
  if (nrow(temp2)) {
    functions.ls[[pkg]] <- temp2
  }
}

stability.df <- bind_rows(stability.ls)
functions.df <- bind_rows(functions.ls)

stability.df |> tibble::tibble()
functions.df |> tibble::tibble()

gg_pkgs_data.df <- full_join(stability.df, functions.df)
```
