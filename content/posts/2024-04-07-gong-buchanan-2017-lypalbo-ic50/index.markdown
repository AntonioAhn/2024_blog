---
title: LY Palbo IC50 analysis of pan cancer cell lines (Gong Buchanan 2017)
author: ''
date: '2024-04-07'
slug: gong-buchanan-2017-lypalbo-ic50
categories: ["R"]
tags: ["project"]
---

The data is from this paper:
Gong X, Litchfield LM, Webster Y, Chio LC, Wong SS, Stewart TR, Dowless M, Dempsey J, Zeng Y, Torres R, Boehnke K, Mur C, Marugán C, Baquero C, Yu C, Bray SM, Wulur IH, Bi C, Chu S, Qian HR, Iversen PW, Merzoug FF, Ye XS, Reinhard C, De Dios A, Du J, Caldwell CW, Lallena MJ, Beckmann RP, Buchanan SG. Genomic Aberrations that Activate D-type Cyclins Are Associated with Enhanced Sensitivity to the CDK4 and CDK6 Inhibitor Abemaciclib. Cancer Cell. 2017 Dec 11;32(6):761-776.e6. doi: 10.1016/j.ccell.2017.11.006. PMID: 29232554.




```r
options(ggrepel.max.overlaps = Inf)
```

# read data




```r
LY_data <- read_csv(file.path(file_dir, "Buchanan_AbsIC50Data_Abema.csv"))
```

```
## New names:
## Rows: 560 Columns: 31
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (16): Cell Line, Cancer Type, DCAF, KHSV cyclin (k-cyclin), CCND 3'UTR l... dbl
## (12): Abs IC50 (µM) Abemaciclib, StdErr(Abs IC50), CCND1 cn, CCND2 cn, C... lgl
## (3): ...29, ...30, ...31
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...29`
## • `` -> `...30`
## • `` -> `...31`
```

```r
Palbo_data <- read_csv(file.path(file_dir, "Buchanan_AbsIC50Data_Palbo.csv"))
```

```
## New names:
## Rows: 492 Columns: 24
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (13): Cell Line, Cancer Type, DCAF, KHSV cyclin (k-cyclin), CCND1 3UTR l... dbl
## (10): Abs IC50 (µM) Palbociclib, CCND1 cn, CCND2 cn, CCND3 cn, CCNE1 cn,... lgl
## (1): ...23
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...23`
## • `` -> `...24`
```

# summary 


```r
LY_data %>% dim
```

```
## [1] 560  31
```

```r
Palbo_data %>% dim
```

```
## [1] 492  24
```

```r
LY_data %>% head
```

```
## # A tibble: 6 × 31
##   Cell L…¹ Cance…² Abs I…³ StdEr…⁴ DCAF  CCND1…⁵ CCND2…⁶ CCND3…⁷ CCNE1…⁸ KHSV …⁹
##   <chr>    <chr>     <dbl>   <dbl> <chr>   <dbl>   <dbl>   <dbl>   <dbl> <chr>  
## 1 JeKo-1   mantle…    0.06    0.02 y           4       4       5       2 <NA>   
## 2 Kasumi-6 acute …    0.12    0.05 <NA>        2       1       2       2 <NA>   
## 3 LS-513   colon …    0.13    0.04 <NA>        2       2       2       2 <NA>   
## 4 Z-138    mantle…    0.13    0.06 y          NA      NA      NA      NA <NA>   
## 5 DMS-53   lung S…    0.17    0.06 y           3      12       3       2 <NA>   
## 6 SK-N-FI  neurob…    0.21   NA    <NA>        2       3       2       2 <NA>   
## # … with 21 more variables: `CCND 3'UTR loss` <chr>, `CCND1 t(11;14)` <chr>,
## #   `CCND1 mut` <chr>, `CCND2 mut` <chr>, `CCND3 mut` <chr>,
## #   `CDKN2A mut` <chr>, `CDKN2A cn` <dbl>, `FBXO31 cn` <dbl>, `FBXO4 cn` <dbl>,
## #   `FBXW8 cn` <dbl>, `PARK2 cn` <dbl>, `RB1 mut` <chr>, `FBXO4 mut` <chr>,
## #   `mRNA Expression Source` <chr>, `Copy Number Source` <chr>,
## #   `Mutation Data Source` <chr>, `Doubling Times (Hours)` <dbl>,
## #   `Seeding Density (cells/well)` <chr>, ...29 <lgl>, ...30 <lgl>, …
```

```r
Palbo_data %>% head
```

```
## # A tibble: 6 × 24
##   Cell L…¹ Cance…² Abs I…³ DCAF  CCND1…⁴ CCND2…⁵ CCND3…⁶ CCNE1…⁷ KHSV …⁸ CCND1…⁹
##   <chr>    <chr>     <dbl> <chr>   <dbl>   <dbl>   <dbl>   <dbl> <chr>   <chr>  
## 1 A704     kidney…    9.86 y           2       3       2       2 <NA>    <NA>   
## 2 BCP-1    body c…    0.32 y           2       3       2       2 y       <NA>   
## 3 CCF-STT… glioma    20    y           2       2      14       2 <NA>    <NA>   
## 4 CHP-212  neurob…    1.07 y           2       2       2       2 <NA>    <NA>   
## 5 COLO-205 colon …    1.98 y           3       3      12       3 <NA>    <NA>   
## 6 D-283MED medull…    2.83 y           2       2       2       2 <NA>    <NA>   
## # … with 14 more variables: `CCND1 t(11;14)` <chr>, `CCND1 mut` <chr>,
## #   `CCND2 mut` <chr>, `CCND3 mut` <chr>, `CDKN2A mut` <chr>,
## #   `CDKN2A cn` <dbl>, `FBXO31 cn` <dbl>, `FBXO4 cn` <dbl>, `FBXW8 cn` <dbl>,
## #   `PARK2 cn` <dbl>, `RB1 mut` <chr>, `FBXO4 mut` <chr>, ...23 <lgl>,
## #   ...24 <chr>, and abbreviated variable names ¹​`Cell Line`, ²​`Cancer Type`,
## #   ³​`Abs IC50 (µM) Palbociclib`, ⁴​`CCND1 cn`, ⁵​`CCND2 cn`, ⁶​`CCND3 cn`,
## #   ⁷​`CCNE1 cn`, ⁸​`KHSV cyclin (k-cyclin)`, ⁹​`CCND1 3UTR loss`
```


# tidy up 


```r
LY_data <- LY_data %>% dplyr::rename("cell_line" = "Cell Line",
                                     "cancer_type" = "Cancer Type",
                                     "IC50" = "Abs IC50 (µM) Abemaciclib")
#                                     "RB1_status" = "RB1 mut")

Palbo_data <- Palbo_data %>% dplyr::rename("cell_line" = "Cell Line",
                                           "cancer_type" = "Cancer Type",
                                           "IC50" = "Abs IC50 (µM) Palbociclib")
#                                           "RB1_status" = "RB1 mut")
```


```r
LY_data$RB1_status = ifelse(is.na(LY_data$"RB1 mut"), "WT", "MUT")
Palbo_data$RB1_status = ifelse(is.na(Palbo_data$"RB1 mut"), "WT", "MUT")
```


```r
combined_data <- left_join(LY_data %>% dplyr::select("cell_line", "cancer_type", "IC50", "RB1_status"), 
                           Palbo_data %>% dplyr::select("cell_line", "cancer_type", "IC50", "RB1_status"), 
                           by = "cell_line")

colnames(combined_data) = combined_data %>% 
  colnames %>% 
  gsub("\\.x", "_LY", .) %>%
  gsub("\\.y", "_Palbo", .)

combined_data$cancer_type_Palbo %>% is.na %>% table
```

```
## .
## FALSE  TRUE 
##   492    68
```


```r
# Theres less IC50 values for Palbo than LY. these are changed to 0. was finding an error when plotting in ggplot, so decided to make it 0.... myabe there is a better way?
combined_data$IC50_Palbo[is.na(combined_data$IC50_Palbo)] = 0
```

# overlap


```r
list_i <- list(LY_data = LY_data$cell_line, 
               Palbo_data = Palbo_data$cell_line
)
     
LY_Palbo_overlap <- calculate.overlap(x = list_i)
```


```r
ggvenn(
  list_i, 
  fill_color = c("#0073C2FF", "#EFC000FF", "#868686FF"),
  stroke_size = 0.5, set_name_size = 7, text_size = 7
  )
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />


# cancer type summary


```r
cancer_type_df <- combined_data$cancer_type_LY %>% table %>% sort(decreasing = TRUE) %>% data.frame
colnames(cancer_type_df) <- c("cancer_type", "frequency")

cancer_type_df %>% ggplot(aes(x = cancer_type, y = frequency, fill = cancer_type)) + 
  geom_bar(stat = "identity") +
  geom_text(aes(label=frequency), position=position_dodge(width=0.9), vjust=-0.25, size = 2) +
  theme_bw() + 
  theme(legend.position = "none", legend.title = element_blank() ,axis.text.x = element_text(angle = 45, hjust = 1))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="960" />

```r
cancer_order <- cancer_type_df$cancer_type %>% levels
```

# IC50 summary


```r
combined_data$cancer_type_LY %>% table %>% sort(decreasing = TRUE) %>% data.frame
```

```
##                                       . Freq
## 1                            lung NSCLC   62
## 2                         breast cancer   43
## 3                          liver cancer   37
## 4                              melanoma   36
## 5                          colon cancer   35
## 6                        gastric cancer   32
## 7                             lung SCLC   30
## 8                      head/neck cancer   28
## 9                     esophageal cancer   21
## 10                       ovarian cancer   21
## 11                               glioma   18
## 12        acute lymphoblastic leukaemia   14
## 13                        kidney cancer   14
## 14                    pancreatic cancer   13
## 15                        lung Squamous   11
## 16                        neuroblastoma   11
## 17                         osteosarcoma   10
## 18                  soft tissue sarcoma   10
## 19              acute myeloid leukaemia    9
## 20                       bladder cancer    9
## 21                   Burkitt's lymphoma    8
## 22                      cervical cancer    8
## 23                   endometrial cancer    6
## 24                       Ewings sarcoma    6
## 25                     multiple myeloma    6
## 26                        non-cancerous    6
## 27                      prostate cancer    5
## 28                       thyroid cancer    5
## 29                 mantle cell lymphoma    4
## 30                      medulloblastoma    4
## 31                         mesothelioma    4
## 32                        biliary tract    3
## 33         chronic myelogenous leukemia    3
## 34        diffuse large B cell lymphoma    3
## 35                        adrenal gland    2
## 36          B cell lymphoma unspecified    2
## 37                         vulva cancer    2
## 38 acute leukaemia of ambiguous lineage    1
## 39             artificially transformed    1
## 40           body cavity based lymphoma    1
## 41                       chondrosarcoma    1
## 42              duodenal adenocarcinoma    1
## 43                  epithelioid sarcoma    1
## 44                    giant cell tumour    1
## 45                  hairy cell leukemia    1
## 46                   Hodgkin's lymphoma    1
## 47    mycosis fungoides-Sezary syndrome    1
## 48             myelodysplastic syndrome    1
## 49             ovarian granulosa cancer    1
## 50           prostate small cell cancer    1
## 51                       retinoblastoma    1
## 52                      somatostatinoma    1
## 53        T cell lymphoblastic lymphoma    1
## 54                             teratoma    1
## 55                    testicular cancer    1
## 56                         Wilm's tumor    1
```

```r
cellline_i <- c(combined_data %>% dplyr::arrange(IC50_LY) %>% .$cell_line %>% head(n=40), 
                combined_data %>% dplyr::arrange(IC50_Palbo) %>% .$cell_line %>% head(n=40)) %>% unique

combined_data <- combined_data %>% unite(plot_name, cell_line, cancer_type_LY, remove = FALSE)
combined_data$plot_name <- combined_data$plot_name %>% gsub(" ", "_", .)

combined_data %>% ggplot(aes(x = -log10(IC50_LY), y = -log10(IC50_Palbo), col = cancer_type_LY)) + 
  geom_point() +
  geom_text_repel(data = combined_data %>% filter(cell_line %in% cellline_i), aes(label = plot_name), size = 2.5) + 
  theme_bw() + 
  theme(legend.position = "bottom", legend.title = element_blank())
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="960" />

# RB mutatant status by cancer type


```r
RB_mutant_df <- combined_data %>% 
  dplyr::group_by(cancer_type_LY, RB1_status_LY) %>% 
  summarise(count = n())
```

```
## `summarise()` has grouped output by 'cancer_type_LY'. You can override using
## the `.groups` argument.
```

```r
RB_mutant_df$cancer_type_LY <- factor(RB_mutant_df$cancer_type_LY, levels = cancer_order)
```


```r
#RB_mutant_df %>% head
RB_mutant_df$count %>% max()

RB_mutant_df %>% 
  ggplot(aes(x = RB1_status_LY, y = count)) + 
  geom_bar(stat= "identity") + 
  ylim(c(0, 65)) +
  facet_wrap( . ~ cancer_type_LY) +
  geom_text(aes(label=count), position=position_dodge(width=0.9), vjust=-0.25, size = 2) +
  theme_bw()
```


```r
RB_mutant_df %>% 
  ggplot(aes(x = cancer_type_LY, y = count, fill = RB1_status_LY)) + 
  #geom_bar(stat=stat_count) +
  geom_bar(stat='identity') + 
  #ylim(c(0, 65)) +
  #facet_wrap( . ~ cancer_type_LY) +
  #geom_text(aes(label=count), position=position_dodge(width=0.9), vjust=-0.25, size = 2) +
  theme_bw() +
  theme(legend.position = "none", axis.title.x = element_blank() ,axis.text.x = element_text(angle = 45, hjust = 1))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />



# which cancer type responds the best 


```r
combined_data %>% 
  ggplot(aes(y = IC50_LY,  x = fct_reorder(cancer_type_LY, IC50_LY, .desc	
 = TRUE))) + 
  geom_boxplot() +
  theme_bw() + 
  theme(legend.position = "none", axis.title.x = element_blank() ,axis.text.x = element_text(angle = 45, hjust = 1))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="1152" />

```r
combined_data %>% 
  ggplot(aes(y = IC50_Palbo,  x = fct_reorder(cancer_type_LY, IC50_Palbo, .desc	
 = TRUE))) + 
  geom_boxplot() +
  theme_bw() + 
  theme(legend.position = "none", axis.title.x = element_blank() ,axis.text.x = element_text(angle = 45, hjust = 1))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-2.png" width="1152" />


# plot IC50 according to cancer type


```r
combined_data$RB1_status_LY
```

```
##   [1] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
##  [13] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
##  [25] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
##  [37] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
##  [49] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT" 
##  [61] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT" 
##  [73] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
##  [85] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
##  [97] "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "MUT" "WT" 
## [109] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT" 
## [121] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT" 
## [133] "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT"
## [145] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [157] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT" 
## [169] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT"
## [181] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [193] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [205] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [217] "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [229] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT" 
## [241] "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [253] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [265] "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT" 
## [277] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT" 
## [289] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT" 
## [301] "MUT" "WT"  "WT"  "MUT" "WT"  "MUT" "WT"  "WT"  "MUT" "WT"  "MUT" "MUT"
## [313] "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT"
## [325] "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [337] "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "MUT" "WT"  "WT" 
## [349] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [361] "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [373] "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [385] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "MUT" "MUT" "WT" 
## [397] "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "MUT" "WT"  "WT"  "WT"  "WT"  "WT" 
## [409] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [421] "WT"  "MUT" "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT"
## [433] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [445] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "MUT" "WT"  "WT" 
## [457] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [469] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT" 
## [481] "WT"  "MUT" "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [493] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [505] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "MUT"
## [517] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [529] "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [541] "WT"  "MUT" "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT"  "WT" 
## [553] "WT"  "WT"  "WT"  "WT"  "WT"  "MUT" "WT"  "WT"
```

```r
RB1_col = c("WT" = "Dodgerblue3", "MUT" = "orange")

plot_IC50 <- function(type){
  temp_df <- combined_data %>% filter(cancer_type_LY == type)

  max_x = temp_df$IC50_LY %>% max %>% round(digits = 1) + 1
  max_y = temp_df$IC50_Palbo %>%  max(na.rm= TRUE) %>% round(digits = 1) +1

  temp_df %>% 
    ggplot(aes(x = IC50_LY, y = IC50_Palbo, col = RB1_status_LY)) + 
    geom_point() +
    geom_text_repel(data = temp_df, aes(label = plot_name), size = 2.5) + 
    scale_color_manual(values  = RB1_col) +
    ggtitle(type) +
    labs(x = "IC50 LY (uM)", y = "IC50 Palbo (uM)") +
    ylim(max_y, 0) +
    xlim(max_x, 0) +
    theme_bw() + 
    theme(legend.position = "top", legend.title = element_blank(), plot.title = element_text(hjust = 0.5, size=25))
  }
```


```r
plot_IC50("breast cancer")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="960" />

```r
plot_IC50("lung NSCLC")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-2.png" width="960" />

```r
plot_IC50("melanoma")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-3.png" width="960" />

```r
plot_IC50("prostate cancer")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-4.png" width="960" />

```r
plot_IC50("pancreatic cancer")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-5.png" width="960" />

```r
plot_IC50("colon cancer")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-6.png" width="960" />



```r
plot_IC50("ovarian cancer")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="960" />

```r
plot_IC50("liver cancer")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-2.png" width="960" />

```r
plot_IC50("ovarian cancer")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-3.png" width="960" />


