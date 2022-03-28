Forest Plots
================

# DEFINITION

Forest Plots are used for 2 main purposes: 1. They are a means of
graphically representing a meta-analysis of the results of randomized
controlled trials. 2. They provide tabular and graphical information
about estimates of comparisons or associations, corresponding precision,
and statistical significance.

The focus of this tutorial is going to be the representation of odds
ratio or standard mean devaiation and their confidence intervals with
respect to one reference group.

# 1\. ODDS RATIO

Odds ratio of mortality of four different factors - Sex, Socioeconomic
condition, Period of the pandemic and Preexisting conditions:

Inputing and formating
data:

``` r
sum <- c(rep(TRUE, 2), rep(FALSE, 25), TRUE, rep(FALSE, 9), TRUE, rep(FALSE, 5),
         TRUE, rep(FALSE, 7))

or.df <- data.frame(
  factors = c("Female (ref)", "Male", 
              "High (ref)", "Low", 
              "Period 1 (05/17/2020-09/30/2020) (ref)", "Period 2 (10/01/2020-01/31/2021)",
         "Period 3 (02/01/2021-05/14/2021)", "Period 4 (05/14/2021-06/14/2021)", 
         "No comorbidity (ref)", "Diabetes", "Hypertension", "Chronic respiratory illness",   "Chronic renal disease", "Chronic liver disease", "Other"), 
  groups = c("Sex", "Sex", 
             "Socio-demographic factors", "Socio-demographic factors", 
             "Period of the pandemic", "Period of the pandemic",
         "Period of the pandemic", "Period of the pandemic", 
         "Preexisting conditions", "Preexisting conditions", "Preexisting conditions",
         "Preexisting conditions", "Preexisting conditions", "Preexisting conditions", "Preexisting conditions"),
  or =  c(NA, 1.15, NA, 1.76, NA, 0.90, 0.84, 9.09, NA, 1.05, 1.13, 1.05, 2.33, 2.08, 1.44),
  ll = c(NA, 1.09, NA, 1.56, NA, 0.85, 0.77, 7.63, NA, 0.97, 1.04, 0.92, 2.04, 1.57, 1.29),
  ul = c(NA, 1.22, NA, 1.98, NA, 0.95, 0.91, 10.83, NA, 1.15, 1.23, 1.20, 2.67, 2.74, 1.61))


#formating confidence intervals as (ll, ul) and adding to dataset
or.df$ci <- ifelse(!is.na(or.df$or), paste("(", as.character(or.df$ll), ", ", as.character(or.df$ul), ")", sep = ""), "")

#adding NA values and factor categories to or, upper limit and lower limit vectors and formated confidence intervals in areas where I want to add blank lines
ci <- c("95% CI", 
        "", or.df$ci[1:2], "", #sex
        "", or.df$ci[3:4], "", #socio-demographic
        "", or.df$ci[5:8], "", #period
        "", or.df$ci[9:15]     #pre-existing condition
        )

or.char <- c("OR", 
        "", as.character(or.df$or[1:2]), "", #sex
        "", as.character(or.df$or[3:4]), "", #socio-demographic
        "", as.character(or.df$or[5:8]), "", #period
        "", as.character(or.df$or[9:15])     #pre-existing condition
        )

or <- c(NA,  
        NA, or.df$or[1:2], NA,#sex 
        NA, or.df$or[3:4], NA, #socio-demographic
        NA, or.df$or[5:8], NA, #period
        NA, or.df$or[9:15]     #pre-existing condition
        )

ll <- c(NA, 
        NA, or.df$ll[1:2], NA, #sex
        NA, or.df$ll[3:4], NA, #socio-demographic
        NA, or.df$ll[5:8], NA, #period
        NA, or.df$ll[9:15]     #pre-existing condition
        )

ul <- c(NA,  
        NA, or.df$ul[1:2], NA, #sex
        NA, or.df$ul[3:4], NA, #socio-economic
        NA, or.df$ul[5:8], NA, #period
        NA, or.df$ul[9:15] #pre-existing condition
        )

factors <- c("", 
        "Sex", or.df$factors[1:2], "", #sex
        "Socioeconomic condition", or.df$factors[3:4], "", #socio-demographic
        "Period of pandemic", or.df$factors[5:8], "", #period
        "Preexisting conditions", or.df$factors[9:15] #pre-existing condition
        )

#specifying bolded text
summary <- c(rep(TRUE, 2), rep(FALSE, 3), TRUE, rep(FALSE, 3), TRUE, rep(FALSE, 5), TRUE, rep(FALSE, 7))

plot.data <- tibble(mean = or,
               lower = ll,
               upper = ul,
               study = factors,
               confint = ci,
               or = or.char)

ticks <- c(0.5, 1, 2, 4, 8, 16)

plot.data %>%
  forestplot(labeltext = c(study, or, confint),
             #clip = c(0.5, 16),
             xticks=ticks,
             xlog = TRUE,
             is.summary = summary,
             new_page = T,
             col = fpColors(box = "royalblue",
                            line = "darkblue"),
             txt_gp = fpTxtGp(cex = 0.8, label = gpar(fontfamily = "serif"), 
                              ticks=gpar(fontsize=5, cex=1.4)),
             graph.pos = 2,
             boxsize = 0.3,
             graphwidth = unit(45, 'mm'),
             title = "Forest Plot of Mortality Rates")
```

![](Forest-Plot_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->
