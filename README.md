
- [1. `tools::CRAN_package_db` that are `^gg|^GG|gg$|GG$` w/ ggplot2
  depend or
  import](#1-toolscran_package_db-that-are-gggggggg-w-ggplot2-depend-or-import)
- [2. `yaml::read_yaml` and `httr2` to parse extension gallery
  `gallery _config.yml`
  file](#2-yamlread_yaml-and-httr2-to-parse-extension-gallery-gallery-_configyml-file)
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
  extenders’](https://evamaerey.github.io/ggplot2-extension-ecosystem/)

# 1. `tools::CRAN_package_db` that are `^gg|^GG|gg$|GG$` w/ ggplot2 depend or import

Code/ideas: June, Joyce, Pepijn, Gina

``` r
df <- tools::CRAN_package_db() |> 
  dplyr::filter(
    stringr::str_detect(Package, "^gg|^GG|gg$|GG$"),
    stringr::str_detect(Depends, "ggplot2") | 
    stringr::str_detect(Imports, "ggplot2")
  ) 

df |> tibble::tibble()
#> # A tibble: 215 × 69
#>    Package  Version Priority Depends Imports LinkingTo Suggests Enhances License
#>    <chr>    <chr>   <chr>    <chr>   <chr>   <chr>     <chr>    <chr>    <chr>  
#>  1 egg      0.4.5   <NA>     gridEx… "gtabl… <NA>      "knitr,… <NA>     GPL-3  
#>  2 gg.gap   1.3     <NA>     <NA>    "ggplo… <NA>       <NA>    <NA>     GPL-3  
#>  3 gg1d     0.1.0   <NA>     R (>= … "asser… <NA>      "covr, … <NA>     MIT + …
#>  4 ggalign  1.0.0   <NA>     ggplot… "cli, … <NA>      "ggrast… <NA>     MIT + …
#>  5 ggalign… 0.1     <NA>     <NA>    "ggplo… <NA>       <NA>    <NA>     GPL-3  
#>  6 ggalign… 1.0.2   <NA>     R (>= … "dplyr… <NA>      "rmarkd… <NA>     MIT + …
#>  7 ggallin  0.1.1   <NA>     ggplot… "scale… <NA>      "knitr,… <NA>     LGPL-3 
#>  8 ggalluv… 0.12.5  <NA>     R (>= … "stats… <NA>      "grid, … <NA>     GPL-3  
#>  9 GGally   2.2.1   <NA>     R (>= … "dplyr… <NA>      "broom … <NA>     GPL (>…
#> 10 ggalt    0.4.0   <NA>     R (>= … "utils… <NA>      "testth… <NA>     AGPL +…
#> # ℹ 205 more rows
#> # ℹ 60 more variables: License_is_FOSS <chr>, License_restricts_use <chr>,
#> #   OS_type <chr>, Archs <chr>, MD5sum <chr>, NeedsCompilation <chr>,
#> #   Additional_repositories <chr>, Author <chr>, `Authors@R` <chr>,
#> #   Biarch <chr>, BugReports <chr>, BuildKeepEmpty <chr>, BuildManual <chr>,
#> #   BuildResaveData <chr>, BuildVignettes <chr>, Built <chr>,
#> #   ByteCompile <chr>, `Classification/ACM` <chr>, …

df$Package
#>   [1] "egg"               "gg.gap"            "gg1d"             
#>   [4] "ggalign"           "ggaligner"         "ggalignment"      
#>   [7] "ggallin"           "ggalluvial"        "GGally"           
#>  [10] "ggalt"             "gganimate"         "ggarchery"        
#>  [13] "ggarrow"           "ggautomap"         "ggbeeswarm"       
#>  [16] "ggbiplot"          "ggblanket"         "ggblend"          
#>  [19] "ggborderline"      "ggbrace"           "ggbrain"          
#>  [22] "ggbreak"           "ggbrick"           "ggBubbles"        
#>  [25] "ggbuildr"          "ggbump"            "ggchangepoint"    
#>  [28] "ggcharts"          "ggChernoff"        "ggcleveland"      
#>  [31] "ggcompare"         "ggcorrplot"        "ggcorset"         
#>  [34] "ggdag"             "ggdark"            "ggdaynight"       
#>  [37] "ggdemetra"         "ggdendro"          "ggdensity"        
#>  [40] "ggdist"            "ggdmc"             "ggDoE"            
#>  [43] "ggDoubleHeat"      "ggeasy"            "GGEBiplots"       
#>  [46] "ggedit"            "ggenealogy"        "ggESDA"           
#>  [49] "ggetho"            "ggExtra"           "ggfacto"          
#>  [52] "ggfields"          "ggfigdone"         "ggFishPlots"      
#>  [55] "ggfittext"         "ggfixest"          "ggflowchart"      
#>  [58] "ggfocus"           "ggfootball"        "ggforce"          
#>  [61] "ggformula"         "ggfortify"         "ggfoundry"        
#>  [64] "ggfun"             "ggfx"              "gggap"            
#>  [67] "gggenes"           "gggenomes"         "ggghost"          
#>  [70] "gggibbous"         "gggrid"            "ggh4x"            
#>  [73] "gghalfnorm"        "gghalves"          "gghdx"            
#>  [76] "ggheatmap"         "gghighlight"       "gghilbertstrings" 
#>  [79] "gghist"            "ggHoriPlot"        "gghourglass"      
#>  [82] "ggimage"           "ggimg"             "gginference"      
#>  [85] "gginnards"         "ggip"              "ggiraph"          
#>  [88] "ggiraphExtra"      "ggisotonic"        "gglgbtq"          
#>  [91] "gglm"              "gglogger"          "gglorenz"         
#>  [94] "ggmap"             "ggmapcn"           "ggmapinset"       
#>  [97] "ggmatplot"         "ggmcmc"            "ggmice"           
#> [100] "GGMncv"            "GGMnonreg"         "ggmosaic"         
#> [103] "ggmugs"            "ggmuller"          "ggmulti"          
#> [106] "ggnetwork"         "ggnewscale"        "ggnormalviolin"   
#> [109] "ggnuplot"          "ggOceanMaps"       "ggokabeito"       
#> [112] "ggordiplots"       "GGoutlieR"         "ggpackets"        
#> [115] "ggpage"            "ggparallel"        "ggparty"          
#> [118] "ggpath"            "ggpattern"         "ggpca"            
#> [121] "ggpcp"             "ggperiodic"        "ggpie"            
#> [124] "ggplate"           "ggplot.multistats" "ggplot2.utils"    
#> [127] "ggplotAssist"      "ggplotgui"         "ggplotify"        
#> [130] "ggpmisc"           "ggPMX"             "ggpointdensity"   
#> [133] "ggpointless"       "ggpol"             "ggpolar"          
#> [136] "ggpolypath"        "ggpp"              "ggprism"          
#> [139] "ggpubr"            "ggpval"            "ggQC"             
#> [142] "ggQQunif"          "ggquickeda"        "ggquiver"         
#> [145] "ggragged"          "ggrain"            "ggRandomForests"  
#> [148] "ggraph"            "ggraptR"           "ggrasp"           
#> [151] "ggrastr"           "ggrcs"             "ggredist"         
#> [154] "ggrepel"           "ggResidpanel"      "ggreveal"         
#> [157] "ggridges"          "ggrisk"            "ggrounded"        
#> [160] "ggRtsy"            "ggsankeyfier"      "ggScatRidges"     
#> [163] "ggsci"             "ggscidca"          "ggseas"           
#> [166] "ggsector"          "ggseg"             "ggsem"            
#> [169] "ggseqlogo"         "ggseqplot"         "ggshadow"         
#> [172] "ggside"            "ggsignif"          "ggsmc"            
#> [175] "ggsoccer"          "ggsolvencyii"      "ggsom"            
#> [178] "ggspark"           "ggspatial"         "ggspectra"        
#> [181] "ggstackplot"       "ggstance"          "ggstar"           
#> [184] "ggstats"           "ggstatsplot"       "ggstream"         
#> [187] "ggstudent"         "ggsurveillance"    "ggsurvey"         
#> [190] "ggsurvfit"         "ggswissmaps"       "ggtangle"         
#> [193] "ggtaxplot"         "ggtea"             "ggtern"           
#> [196] "ggtext"            "ggThemeAssist"     "ggthemes"         
#> [199] "ggthemeUL"         "ggtibble"          "ggtikz"           
#> [202] "ggTimeSeries"      "ggtrace"           "ggtrendline"      
#> [205] "ggtricks"          "ggupset"           "ggvenn"           
#> [208] "ggVennDiagram"     "ggvfields"         "ggview"           
#> [211] "ggvolcano"         "ggwordcloud"       "ggx"              
#> [214] "granovaGG"         "tablesgg"

readr::write_csv(df, "gg_w_ggplot2_depends_or_imports_cran.csv")
```

# 2. `yaml::read_yaml` and `httr2` to parse extension gallery `gallery _config.yml` file

Code/ideas: Pepijn, Joyce, Gina, (Probably others)

``` r
df <-
  "https://raw.githubusercontent.com/ggplot2-exts/gallery/refs/heads/gh-pages/_config.yml" |>
  httr2::request() |>
  httr2::req_perform() |>
  httr2::resp_body_string() |>
  (\(x) yaml::read_yaml(text = x))() |>
  _$widgets |>
  dplyr::bind_rows()

df$name
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
#> [145] "ggfootball"

df
#> # A tibble: 145 × 10
#>    name     thumbnail url   ghuser ghrepo tags  cran  ghauthor short description
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
#> # ℹ 135 more rows

readr::write_csv(df, "gg_extension_pkgs_gallery.csv")
```

# 3. `gh::gh` Keep in mind in terms: github contributors

“Re finding GitHub users from packages, the repo owner is not always the
only/main contributor, especially if it’s an org”

Code/ideas: Carl Suster @arcresu

<https://github.com/ggplot2-extenders/ggplot-extension-club/discussions/82#discussioncomment-12469510>

``` r
gh_contributors <- function(repo) {
  resp <- gh::gh("GET /repos/{repo}/contributors", repo = repo)
  total_contributions <- sum(sapply(resp, \(x) x$contributions))
  resp <- Filter(\(x) x$type == "User", resp) # exclude bots
  resp <- Filter(\(x) x$contributions > total_contributions/5, resp) # with at least 20% of contributions
  sapply(resp, \(x) stringr::str_trim(x$login))
}

gh_contributors("cidm-ph/ggmapinset")
#> [1] "arcresu"
gh_contributors("YuLab-SMU/ggfun")
#> [1] "GuangchuangYu" "xiangpin"
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

write.csv(gg_pkgs_data.df, file = "gg-pkgs-data.csv")
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
