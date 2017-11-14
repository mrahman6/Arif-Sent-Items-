SoDA-502 Project
================
Arif Masrur
Nov 14, 2017

**Overarching Goal:**

The goal of this project has been to compare *[Integrated Crisis Early Warning (ICEWS)](https://www.lockheedmartin.com/us/products/W-ICEWS.html)* and *[Phoenix](http://phoenixdata.org/)* event datasets from spatial and temporal coverage perspectives. You all already know that both ICEWS and Phoenix datasets are automated, machine-codded, and maintained a common coding ontology - *[Conflict and Mediation Event (CAMEO)](http://eventdata.parusanalytics.com/data.dir/cameo.html)*.

**A Brief Overview of Datasets:**

The Phoenix dataset is considered significant improvement over previous event data generation efforts and labeled as "next-generation" dataset. This higher value of Phoenix data corresponds to its near-real time nature, and the use of advance coding process (i.e., open-source NLP software) and copious online news content during data generation steps. The most updated version of Phoenix dataset is built on content from the New York Times (NYT; 1945-2005), the BBC Monitoring's Summary of World Broadcasts (SWB; 1979-2015) and the CIA's Foreign Broadcast Information Service (FBIS; 1995-2004).

The ICEWS project is somewhat similar to the Phoenix data project. News sources are from roughly 300 different publishers. However, data spans roughly from 1995 to 2015.

It's been suggested in (Beieler, 2016) that ICEWS project and Phoenix dataset shares common vision, in terms of their goals. However, this comment has been made only by analyzing and comparing very small subset of both datasets (only few years; 2014 to 2015/16) than available now. Therefore, by leveraging currently released large volume, longer time period Phoenix dataset this project aimed to provide a detailed, in-depth compariosn between Phoenix and ICEWS, to examine the extent of their shared vision and goals. In doing so we have focused on analyzing their *spatial*, *temporal*, and *attribute* characterisitcs.

**My Talk Today:**

Here, as part of this project I'm presenting some data-level comparison between Phoenix and ICEWS datasets. This comparison primarily focuses on teasing out differences in the temporal coverages of events and their attributes. Mark's exploratory analysis already has revealed some excellent temporal characteristics, however, here I'll provide some additional insights into our current understanding.

``` r
# Set workplace

setwd("H:/SoDA502 Project/Data_by_year") 
```

### Overall Temporal Patterns of Phoenix and ICEWS Datasets

With the aim for a meaningful comparison I have selected events between 1995 and 2015, since this period interests for both datasets. In doing so, I aggregated constituent datasets (i.e., NYT, SWB, and FBIS) to span over 1995-2015, since these dataset's temporal coverages are mutually exclusive.

Codes below produce plots that show time series of daily counts of events in both datasets. It's obvious that overall ICEWS captured more events per month than Phoenix. Phoenix shows comparatively more variability of event counts than ICEWS. There are few points where the number of events drop close to zero. This may have happened due to server failure or software bugs in the web scrapper, as suggested by Beieler (2016) who looked at temporal distribution of Phoenix datasets between 2014-2016.

``` r
Phoenix1995_15 = read.csv("Phoenix1995_15.csv")
Phoenix1995_15$story_date = as.Date(Phoenix1995_15$story_date, format="%m/%d/%Y")

phoenix_plot = ggplot(Phoenix1995_15, aes(x = story_date)) +
  geom_histogram(binwidth = 30,
                 fill="#756bb1", colour="grey") +
  scale_x_date(date_breaks = "1 year",
               date_minor_breaks = "1 year",
               date_labels = "%Y") +
  labs( title = "Phoenix Events per Month", 
        x = NULL,
        y = "Event Count") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))


icews1995_15 = read.csv("icews1995_15.csv")
icews1995_15$story_date = as.Date(icews1995_15$story_date)

icews_plot = ggplot(icews1995_15, aes(x = story_date)) +
  geom_histogram(binwidth = 30,
                 fill="#dd1c77", colour="grey") +
  scale_x_date(date_breaks = "1 year",
               date_minor_breaks = "1 year",
               date_labels = "%Y") +
  labs( title = "Icews Events per Month", 
        x = NULL,
        y = "Event Count") +
  theme(plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))

phoenix_plot
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-1-1.png" style="display: block; margin: auto;" />

``` r
icews_plot
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-1-2.png" style="display: block; margin: auto;" />

### International Events: Phoenix Vs. ICEWS

In order to have better understanding of both dataset's performance, I have looked at specific cases and examined similar set of attributes. Two of the major events of international interests in recent times are the conflicts in Syria and Georgia. However, instead of looking over any specific period when these conflicts actually climaxed, I have analyzed entire 1995-2015 period to get a comparative view as events unfold in time with their links to previous ones.

### Syria

<span style="color:blue">Syrian Events over Time as Represented in ICEWS and Phoenix Datasets</span>

One of the major events in recent times covered by the ICEWS and Phoenix dataset is the conflict in Syria. From Phoenix and ICEWS datasets I have extracted all events that have Syria country code (i.e., SYR) as a **source** OR **target** entity over the 1995-2015 period.

``` r
# SYRIA EVENTS IN PHOENIX 

Phoenix_SYR = Phoenix1995_15 %>% filter(source_root == "SYR" | target_root == "SYR")
Phoenix_SYR$story_date = as.Date(Phoenix_SYR$story_date, format="%m/%d/%Y")

phoenixSYR_plot = ggplot(Phoenix_SYR, aes(x = story_date)) +
  geom_histogram(binwidth = 30,
                 fill="#756bb1", colour="grey") +
  scale_x_date(date_breaks = "1 year",
               date_minor_breaks = "1 year",
               date_labels = "%Y") +
  labs( title = "Total Syrian Events in Phoenix per Month", 
        x = NULL,
        y = "Event Count") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))

# SYRIA EVENTS IN ICEWS

icews_SYR = icews1995_15 %>% filter(source_root == "SYR" | target_root == "SYR")

icews_SYR$story_date = as.Date(icews_SYR$story_date, format="%m/%d/%Y")

icewsSYR_plot = ggplot(icews_SYR, aes(x = story_date)) +
  geom_histogram(binwidth = 30,
                 fill="#dd1c77", colour="grey") +
  scale_x_date(date_breaks = "1 year",
               date_minor_breaks = "1 year",
               date_labels = "%Y") +
  labs( title = "Total Syrian Events in ICEWS per Month", 
        x = NULL,
        y = "Event Count") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))


phoenixSYR_plot
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-2-1.png" style="display: block; margin: auto;" />

``` r
icewsSYR_plot
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-2-2.png" style="display: block; margin: auto;" />

<span style="color:blue">Syrian Quad Class Distribution</span>

The breakdown of low-level political event types in both datasets are quite similar.

``` r
## Aggregate Phoenix and ICEWS datasets by Quad class (or Event Types)

pho_SYR_quad = Phoenix_SYR  %>% group_by(quad_class) %>% tally()
icews_SYR_quad = icews_SYR %>% group_by(quad_class) %>% tally()

## Plot Aggregated Event Types along the Temporal Axis

# Phoenix

ggplot(pho_SYR_quad, aes(x = quad_class, y = n, fill = quad_class)) + geom_bar(stat = "identity") +
  labs(x="Quad Classes", y="Counts") +
  ggtitle("Syrian Quad Class Distribution in Phoenix \n (1995-2015)")+
  theme( plot.title = element_text(size=15, 
                                   face="bold", 
                                   #family="American Typewriter",
                                   color="black",
                                   hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  vjust=.5))
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-3-1.png" style="display: block; margin: auto;" />

``` r
# ICEWS

ggplot(icews_SYR_quad, aes(x = quad_class, y = n, fill = quad_class)) + geom_bar(stat = "identity") +
  labs(x="Quad Classes", y="Counts") +
  ggtitle("Syrian Quad Class Distribution in ICEWS \n (1995-2015)")+
  theme( plot.title = element_text(size=15, 
                                   face="bold", 
                                   #family="American Typewriter",
                                   color="black",
                                   hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  vjust=.5))
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-3-2.png" style="display: block; margin: auto;" />

<span style="color:blue">Top Actor Codes for Syria</span>

We can see which actor codes most frequently appeared in the Phoenix and ICEWS datasets. Not surprisingly Syria is the top actor in both datasets. Distribution of other actors are pretty similar.

``` r
## Actor Codes of Syria Events in Phoenix
Phoenix_SYR = read.csv("Phoenix_SYR.csv")
Phoenix_SYR = Phoenix_SYR %>% filter(source_root != "---") %>% filter(target_root != "---")
i = data.frame(Phoenix_SYR$source_root)
names(i) = c("actors")
j = data.frame(Phoenix_SYR$target_root)
names(j) = c("actors")
pho_SYR_actors = rbind(i, j)
pho_SYR_actors_grouped = pho_SYR_actors %>% group_by(actors) %>% tally()
pho_top_SYR_actors = pho_SYR_actors_grouped %>% filter(n >= mean(n))


## Actors Codes of Syria Events in ICEWS

icews_SYR = read.csv("icews_SYR.csv")
names(icews_SYR)
```

    ##  [1] "X.1"                 "X"                   "story_date"         
    ##  [4] "year"                "month"               "source_root"        
    ##  [7] "Source_country_code" "source_agent"        "target_root"        
    ## [10] "target_country_code" "target_agent"        "V8"                 
    ## [13] "goldstein"           "quad_class"

``` r
icews_SYR = icews_SYR %>% filter(source_root != "---") %>% filter(target_root != "---")
i = data.frame(icews_SYR$source_root)
names(i) = c("actors")
j = data.frame(icews_SYR$target_root)
names(j) = c("actors")
icews_SYR_actors = rbind(i, j)
icews_SYR_actors_grouped = icews_SYR_actors %>% group_by(actors) %>% tally()
icews_top_SYR_actors = icews_SYR_actors_grouped %>% filter(n >= mean(n))


## Plot actors codes for Phoenix
ggplot(pho_top_SYR_actors, aes(x = actors, y = n)) + 
  geom_bar(stat = "identity", color = "black", fill = "#756bb1") +
  labs( title = "Top Actor Codes (for Syria Events) in Phoenix", 
        x = NULL,
        y = "Frequency") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-4-1.png" style="display: block; margin: auto;" />

``` r
## Plot actors codes for ICEWS
ggplot(icews_top_SYR_actors, aes(x = actors, y = n)) + 
  geom_bar(stat = "identity", color = "black", fill = "#dd1c77") +
 labs( title = "Top Actor Codes (for Syria Events) in ICEWS", 
        x = NULL,
        y = "Frequency") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-4-2.png" style="display: block; margin: auto;" />

### Georgia

<span style="color:blue">Georgian Events over Time</span>

Similarly, from both datasets I have extracted all events that have Georgia country code (i.e., Geo) as a **source** OR **target** entity over the 1995-2015 period.

``` r
# Georgia Events in Phoenix 

Phoenix_Geo = Phoenix1995_15 %>% filter(source_root == "GEO" | target_root == "GEO")
Phoenix_Geo$story_date = as.Date(Phoenix_Geo$story_date, format="%m/%d/%Y")

Phoenix_Geo_plot = ggplot(Phoenix_Geo, aes(x = story_date)) +
  geom_histogram(binwidth = 30,
                 fill="#756bb1", colour="grey") +
  scale_x_date(date_breaks = "1 year",
               date_minor_breaks = "1 year",
               date_labels = "%Y") +
  labs( title = "Total Georgia Events in Phoenix per Month", 
        x = NULL,
        y = "Event Count") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))


# Georgia Events in ICEWS

icews_GEO = icews1995_15 %>% filter(source_root == "GEO" | target_root == "GEO")

icews_GEO$story_date = as.Date(icews_GEO$story_date, format="%m/%d/%Y")

icews_GEO_plot = ggplot(icews_GEO, aes(x = story_date)) +
  geom_histogram(binwidth = 30,
                 fill="#dd1c77", colour="grey") +
  scale_x_date(date_breaks = "1 year",
               date_minor_breaks = "1 year",
               date_labels = "%Y") +
  labs( title = "Total Georgian Events in ICEWS per Month", 
        x = NULL,
        y = "Event Count") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))

Phoenix_Geo_plot
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-5-1.png" style="display: block; margin: auto;" />

``` r
icews_GEO_plot
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-5-2.png" style="display: block; margin: auto;" />

As manifested in these spikes of event counts, it seems both datasets captured major events related to major conflicts, albeit total number of events in the Phoenix dataset is lower.

<span style="color:blue">Georgian Quad Class Distribution</span>

Quad class distribution for Georgia events is quite similar in both datasets - a similar pattern previously manifested in the Syria case.

``` r
icews_GEO_quad = icews_GEO %>% group_by(quad_class) %>% tally()
pho_GEO_quad = Phoenix_Geo  %>% group_by(quad_class) %>% tally()

Quadplot_pho = ggplot(pho_SYR_quad, aes(x = quad_class, y = n, fill = quad_class)) + geom_bar(stat = "identity") +
  labs(x="Quad Classes", y="Counts") +
  ggtitle("Georgian Quad Class Distribution in Phoenix \n (1995-2015)")+
  theme( plot.title = element_text(size=15, 
                                   face="bold", 
                                   #family="American Typewriter",
                                   color="black",
                                   hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  vjust=.5))

Quadplot_icews = ggplot(icews_SYR_quad, aes(x = quad_class, y = n, fill = quad_class)) + geom_bar(stat = "identity") +
  labs(x="Quad Classes", y="Counts") +
  ggtitle("Georgian Quad Class Distribution in ICEWS \n (1995-2015)")+
  theme( plot.title = element_text(size=15, 
                                   face="bold", 
                                   #family="American Typewriter",
                                   color="black",
                                   hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  vjust=.5))

Quadplot_pho
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-6-1.png" style="display: block; margin: auto;" />

``` r
Quadplot_icews
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-6-2.png" style="display: block; margin: auto;" /> <span style="color:blue">Top Actor Codes for Georgia</span>

The frequency distribution of top actors in Georgia case is pretty similar for both datasets, as also was seen in the Syria case.

``` r
## Actors Codes of Syria Events in ICEWS

icews_GEO = icews_GEO %>% filter(source_root != "---") %>% filter(target_root != "---")

i = data.frame(icews_GEO$source_root)
names(i) = c("actors")
j = data.frame(icews_GEO$target_root)
names(j) = c("actors")
icews_GEO_actors = rbind(i, j)
icews_GEO_actors_grouped = icews_GEO_actors %>% group_by(actors) %>% tally()
icews_top_GEO_actors = icews_GEO_actors_grouped %>% filter(n >= mean(n))

## Actor Codes of Syria Events in Phoenix
Phoenix_Geo = Phoenix_Geo %>% filter(source_root != "---") %>% filter(target_root != "---")
i = data.frame(Phoenix_Geo$source_root)
names(i) = c("actors")
j = data.frame(Phoenix_Geo$target_root)
names(j) = c("actors")
pho_GEO_actors = rbind(i, j)
pho_GEO_actors_grouped = pho_GEO_actors %>% group_by(actors) %>% tally()
pho_top_GEO_actors = pho_GEO_actors_grouped %>% filter(n >= mean(n))


## Plot actors codes for Phoenix
ggplot(pho_top_GEO_actors, aes(x = actors, y = n)) + 
  geom_bar(stat = "identity", color = "black", fill = "#756bb1") +
  labs( title = "Top Actor Codes (for Georgia Events) in Phoenix", 
        x = NULL,
        y = "Frequency") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-7-1.png" style="display: block; margin: auto;" />

``` r
## Plot actors codes for ICEWS
ggplot(icews_top_GEO_actors, aes(x = actors, y = n)) + 
  geom_bar(stat = "identity", color = "black", fill = "#dd1c77") +
 labs( title = "Top Actor Codes (for Georgia Events) in ICEWS", 
        x = NULL,
        y = "Frequency") +
  theme( plot.title = element_text(size=15, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))
```

<img src="SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-7-2.png" style="display: block; margin: auto;" />

### Correlations

Here, I have used correlation measures for the comparison between two datasets.

First I subset data for "verbal cooperation" from USA (source country) to the rest of the world, to examine how these phenomena have been represented in both datasets. For this purpose, I have used weekly aggregated data, since USA as a world leader most frequently mediate/influence world events.

**Data processing for only international events:**

### For this purpose, I subset data only for international events. That means, in these events source actors and target actors are different.

``` r
## Phoenix 

# source1<-read.csv("H:/SoDA502 Project/Data_by_year/PhoenixFBIS_1995-2004.csv")
# source2<-read.csv("H:/SoDA502 Project/Data_by_year/PhoenixNYT_1945-2005.csv")
# source3<-read.csv("H:/SoDA502 Project/Data_by_year/PhoenixSWB_1979-2015.csv")
# 
# sources <-rbind(source1, source2, source3)
# 
# phoenix.data_conflicts = sources %>% 
#   filter(source_root != target_root) %>%
#   filter(source_agent=="GOV" | source_agent=="MIL") %>%
#   filter(source_root!="") %>%
#   filter(target_agent=="GOV" | target_agent=='MIL') %>%
#   filter(target_root!="") %>%
#   filter(is.na(year)==F) %>% 
#   filter(year >= 1995) %>%
#   filter(quad_class == 1 | quad_class == 2 | quad_class == 3 | quad_class == 4) %>%
#   select(story_date, source_root, source_agent, target_root, target_agent, year, month, quad_class, goldstein)
# 
# write.csv(phoenix.data_conflicts, "pho_international_conflicts.csv")


########## ICEWS ############

# icews.data = read.csv("icews1995_2015.csv")
# 
# ##### international crisis ######
# #create dataset, which includes year and month
# 
# icews.data_conflicts = icews.data %>%
#   filter(is.na(V1)==F) %>%
#   filter(V4=='GOV' | V4=='MIL') %>% #actors are only government and military
#   filter(V7=='GOV' | V7=='MIL') %>%
#   filter(V2 != V5) %>% # exclude within-country observations
#   filter(V2 != "---") %>%
#   filter(V5 != "---") %>%
#   filter(V3 != 0 & V6 != 0) %>% 
#   filter(V3 != 997 & V6 != 997) %>% 
#   mutate(year = substr(V1, 1, 4)) %>% # create year variable
#   mutate(month = substr(V1, 6, 7)) %>% # month variable
#   filter(V10 == 1 | V10 == 2 | V10 == 3 | V10 == 4) %>%
#   select(V1, year, month, V2, V3, V4, V5, V6, V7, V8, V9, V10)
# 
# setnames(icews.data_conflicts, old = c("V1", "year", "month", "V2", "V3", "V4", "V5", "V6", "V7", "V8", "V9", "V10"), new = c("story_date", "year", "month", "source_root", "Source_country_code", "source_agent", "target_root", "target_country_code", "target_agent", "V8", "goldstein", "quad_class"))
# 
# write.csv(icews.data_conflicts, "icews_international_conflicts.csv")
#icews.data_conflicts = read.csv("icews_international_conflicts.csv")

## Read Datasets

phoenix.data_conflicts = read.csv("pho_international_conflicts.csv")
icews.data_conflicts = read.csv("icews_international_conflicts.csv")
```

**Weekly Aggregation of Datasets**

``` r
## Daily Agrregation of Verbal cooperation events Triggered by USA in Phoenix

Phoenix_verbal_USA = phoenix.data_conflicts %>% filter(quad_class == 1) %>% filter(source_root == "USA") %>% group_by(story_date) %>% tally()
names(Phoenix_verbal_USA)[1] = "event_date"
Phoenix_verbal_USA$event_date = as.Date(Phoenix_verbal_USA$event_date, format="%m/%d/%Y")

## Weekly-aggregation 
Phoenix_verbal_USA_weekly = Phoenix_verbal_USA %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
## Monthly-aggregation 
Phoenix_verbal_USA_monthly = Phoenix_verbal_USA %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
## Yearly-aggregation
Phoenix_verbal_USA_yearly = Phoenix_verbal_USA %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

icews_verbal_USA = icews.data_conflicts %>% filter(quad_class == 1) %>% filter(source_root == "USA") %>% group_by(story_date) %>% tally()
names(icews_verbal_USA)[1] = "event_date"
icews_verbal_USA$event_date = as.Date(icews_verbal_USA$event_date, format="%m/%d/%Y")

## Weekly-aggregation 
icews_verbal_USA_weekly = icews_verbal_USA %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
## Monthly-aggregation
icews_verbal_USA_monthly = icews_verbal_USA %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
## Yearly-aggregation
icews_verbal_USA_yearly = icews_verbal_USA %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)


## Combine these two weekly aggregated datasets

icews_pho_weekly_common = merge(icews_verbal_USA_weekly, phoenix_verbal_USA_weekly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
```

    ## Error in as.data.frame(y): object 'phoenix_verbal_USA_weekly' not found

``` r
names(icews_pho_weekly_common) = c("event_date", "icews_events_weekly", "phoenix_events_weekly")
```

    ## Error in names(icews_pho_weekly_common) = c("event_date", "icews_events_weekly", : object 'icews_pho_weekly_common' not found

``` r
## Combine these two monthly aggregated datasets

icews_pho_monthly_common = merge(icews_verbal_USA_monthly, phoenix_verbal_USA_monthly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
```

    ## Error in as.data.frame(y): object 'phoenix_verbal_USA_monthly' not found

``` r
names(icews_pho_monthly_common) = c("event_date", "icews_events_monthly", "phoenix_events_monthly")
```

    ## Error in names(icews_pho_monthly_common) = c("event_date", "icews_events_monthly", : object 'icews_pho_monthly_common' not found

``` r
par(mfrow=c(1,2))

######------- Plot---------
######---------------------

## Weekly

ggplot() +
  geom_line(data = icews_pho_weekly_common, aes(y = phoenix_events_weekly, x = event_date, colour = "red")) +  
  geom_line(data = icews_pho_weekly_common, aes(y = icews_events_weekly, x = event_date, colour = "blue")) + 
  scale_x_date(date_breaks = "1 year", date_labels = "%y")+
  ## specify yaxis limits and remove any axis expansion
  scale_y_continuous(expand = c(0,0), limits = c(0,700)) +
  scale_color_discrete(name = "Weekly Events", labels = c("Icews", "Phoenix"))+
  labs( title = "Verbal Cooperation Events per Week (Source: USA, Target: All)", 
        x = NULL,
        y = "Event Count per Same Week") +
  theme( plot.title = element_text(size=13, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))
```

    ## Error in fortify(data): object 'icews_pho_weekly_common' not found

``` r
## Monthly

ggplot() +
  geom_line(data = icews_pho_monthly_common, aes(y = phoenix_events_monthly, x = event_date, colour = "red")) +  
  geom_line(data = icews_pho_monthly_common, aes(y = icews_events_monthly, x = event_date, colour = "blue")) + 
  scale_x_date(date_breaks = "1 year", date_labels = "%y")+
  ## specify yaxis limits and remove any axis expansion
  scale_y_continuous(expand = c(0,0), limits = c(0,700)) +
  scale_color_discrete(name = "Monthly Events", labels = c("Icews", "Phoenix"))+
  labs( title = "Verbal Cooperation Events per Month (Source: USA, Target: All)", 
        x = NULL,
        y = "Event Count per Same Month") +
  theme( plot.title = element_text(size=13, 
                              face="bold", 
                              #family="American Typewriter",
                              color="black",
                              hjust=0.5, lineheight=1.2),
         axis.text = element_text(size = 12, 
                                  angle = 30, vjust=.5))
```

    ## Error in fortify(data): object 'icews_pho_monthly_common' not found

``` r
#VCUS_icews_pho_weekly_plot

#VCUS_icews_pho_monthly_plot
```

We can see that Phoenix dataset covers significantly lower amount of such events. This may be an interesting finding (or not!).

``` r
ggscatter(icews_pho_weekly_common, x = "icews_events_weekly", y = "phoenix_events_weekly",
                                      add = "reg.line",
                                      add.params = list(color = "blue", fill = "lightgray"),
                                      conf.int = TRUE, 
                                      cor.coef = TRUE, 
                                      cor.method = "pearson", 
                                      main = "Verbal Cooperation Events per Week \n (Source: USA, Target: All)", 
                                      xlab = "ICEWS (1995-2015)", ylab = "Phoenix (1995-2015)")
```

    ## Error in ggscatter(icews_pho_weekly_common, x = "icews_events_weekly", : object 'icews_pho_weekly_common' not found

``` r
#cor_VCUS_icews_pho_weekly 

ggscatter(icews_pho_monthly_common, x = "icews_events_monthly", y = "phoenix_events_monthly",
                                      add = "reg.line",
                                      add.params = list(color = "blue", fill = "lightgray"),
                                      conf.int = TRUE, 
                                      cor.coef = TRUE, 
                                      cor.method = "pearson", 
                                      main = "Verbal Cooperation Events per Month \n (Source: USA, Target: All)", 
                                      xlab = "ICEWS (1995-2015)", ylab = "Phoenix (1995-2015)")
```

    ## Error in ggscatter(icews_pho_monthly_common, x = "icews_events_monthly", : object 'icews_pho_monthly_common' not found

``` r
#cor_VCUS_icews_pho_monthly 
```

Obviously, Correlation coefficient is very low, as well.

Next, I examined associations between these two datasets for event counts of each of the four major event types (from quad classes) at three temporal scales (i.e., weekly, monthly, and yearly).

Comparison between Phoenix and ICEWS based on Quad Counts
---------------------------------------------------------

**Quad classes used: Verbal cooperation, material cooperation, verbal conflict, and material conflict**

``` r
###--------------------------------------------------- 
### Subset and group (by day) data for each quad class  
###---------------------------------------------------

# Phoenix
pho_vcoop = Phoenix1995_15 %>% filter(quad_class == 1) %>% group_by(story_date) %>% tally()
names(pho_vcoop)[1] = "event_date"
pho_vcoop$event_date = as.Date(pho_vcoop$event_date, format="%m/%d/%Y")

pho_mcoop = Phoenix1995_15 %>% filter(quad_class == 2) %>% group_by(story_date) %>% tally()
names(pho_mcoop)[1] = "event_date"
pho_mcoop$event_date = as.Date(pho_mcoop$event_date, format="%m/%d/%Y")

pho_vcon = Phoenix1995_15 %>% filter(quad_class == 3) %>% group_by(story_date) %>% tally()
names(pho_vcon)[1] = "event_date"
pho_vcon$event_date = as.Date(pho_vcon$event_date, format="%m/%d/%Y")

pho_mcon = Phoenix1995_15 %>% filter(quad_class == 4) %>% group_by(story_date) %>% tally()
names(pho_mcon)[1] = "event_date"
pho_mcon$event_date = as.Date(pho_mcon$event_date, format="%m/%d/%Y")

# ICEWS
icews_vcoop = icews1995_15 %>% filter(quad_class == 1) %>% group_by(story_date) %>% tally()
names(icews_vcoop)[1] = "event_date"
icews_vcoop$event_date = as.Date(icews_vcoop$event_date)

icews_mcoop = icews1995_15 %>% filter(quad_class == 2) %>% group_by(story_date) %>% tally()
names(icews_mcoop)[1] = "event_date"
icews_mcoop$event_date = as.Date(icews_mcoop$event_date)

icews_vcon = icews1995_15 %>% filter(quad_class == 3) %>% group_by(story_date) %>% tally()
names(icews_vcon)[1] = "event_date"
icews_vcon$event_date = as.Date(icews_vcon$event_date)

icews_mcon = icews1995_15 %>% filter(quad_class == 4) %>% group_by(story_date) %>% tally()
names(icews_mcon)[1] = "event_date"
icews_mcon$event_date = as.Date(icews_mcon$event_date)

###---------------------------------------------------------- 
### Multi-temporal scale data aggregation for each quad class 
###----------------------------------------------------------

##--------------- Weekly-aggregation of Phoenix data 
##--------------------------------------------------
pho_vcoop_weekly = pho_vcoop %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
pho_vcoop_monthly = pho_vcoop %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
pho_vcoop_yearly = pho_vcoop %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

pho_mcoop_weekly = pho_mcoop %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
pho_mcoop_monthly = pho_mcoop %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
pho_mcoop_yearly = pho_mcoop %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

pho_vcon_weekly = pho_vcon %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
pho_vcon_monthly = pho_vcon %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
pho_vcon_yearly = pho_vcon %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

pho_mcon_weekly = pho_mcon %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
pho_mcon_monthly = pho_mcon %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
pho_mcon_yearly = pho_mcon %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

##------------------Weekly-aggregation of Icews data  
##--------------------------------------------------
icews_vcoop_weekly = icews_vcoop %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
icews_vcoop_monthly = icews_vcoop %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
icews_vcoop_yearly = icews_vcoop %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

icews_mcoop_weekly = icews_mcoop %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
icews_mcoop_monthly = icews_mcoop %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
icews_mcoop_yearly = icews_mcoop %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

icews_vcon_weekly = icews_vcon %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
icews_vcon_monthly = icews_vcon %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
icews_vcon_yearly = icews_vcon %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

icews_mcon_weekly = icews_mcon %>% tq_transmute(select = n, mutate_fun = apply.weekly, FUN = sum)
icews_mcon_monthly = icews_mcon %>% tq_transmute(select = n, mutate_fun = apply.monthly, FUN = sum)
icews_mcon_yearly = icews_mcon %>% tq_transmute(select = n, mutate_fun = apply.yearly, FUN = sum)

###---------------------------------------------------------------------------------
### Combine ICEWS and Phoenix datasets for each quadclass based on common time-scale 
###---------------------------------------------------------------------------------

##--------------- Verbal Cooperation
##----------------------------------
pho_icews_vcoop_weekly = merge(pho_vcoop_weekly, icews_vcoop_weekly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_vcoop_weekly) = c("event_date", "pho_vcoop_weekly", "icews_vcoop_weekly")

pho_icews_vcoop_monthly = merge(pho_vcoop_monthly, icews_vcoop_monthly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_vcoop_monthly) = c("event_date", "pho_vcoop_monthly", "icews_vcoop_monthly")

pho_icews_vcoop_yearly = merge(pho_vcoop_yearly, icews_vcoop_yearly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_vcoop_yearly) = c("event_date", "pho_vcoop_yearly", "icews_vcoop_yearly")



##--------------- Material Copoperation 
##-------------------------------------
pho_icews_mcoop_weekly = merge(pho_mcoop_weekly, icews_mcoop_weekly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_mcoop_weekly) = c("event_date", "pho_mcoop_weekly", "icews_mcoop_weekly")

pho_icews_mcoop_monthly = merge(pho_mcoop_monthly, icews_mcoop_monthly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_mcoop_monthly) = c("event_date", "pho_mcoop_monthly", "icews_mcoop_monthly")

pho_icews_mcoop_yearly = merge(pho_mcoop_yearly, icews_mcoop_yearly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_mcoop_yearly) = c("event_date", "pho_mcoop_yearly", "icews_mcoop_yearly")


##------------------ Verbal Conflict 
##----------------------------------
pho_icews_vcon_weekly = merge(pho_vcon_weekly, icews_vcon_weekly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_vcon_weekly) = c("event_date", "pho_vcon_weekly", "icews_vcon_weekly")

pho_icews_vcon_monthly = merge(pho_vcon_monthly, icews_vcon_monthly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_vcon_monthly) = c("event_date", "pho_vcon_monthly", "icews_vcon_monthly")

pho_icews_vcon_yearly = merge(pho_vcon_yearly, icews_vcon_yearly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_vcon_yearly) = c("event_date", "pho_vcon_yearly", "icews_vcon_yearly")


##---------------- Material Conflict
##----------------------------------
pho_icews_mcon_weekly = merge(pho_mcon_weekly, icews_mcon_weekly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_mcon_weekly) = c("event_date", "pho_mcon_weekly", "icews_mcon_weekly")

pho_icews_mcon_monthly = merge(pho_mcon_monthly, icews_mcon_monthly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_mcon_monthly) = c("event_date", "pho_mcon_monthly", "icews_mcon_monthly")

pho_icews_mcon_yearly = merge(pho_mcon_yearly, icews_mcon_yearly, by.x = "event_date", by.y = "event_date", all.x = FALSE, all.y = FALSE)
names(pho_icews_mcon_yearly) = c("event_date", "pho_mcon_yearly", "icews_mcon_yearly")
```

### Scatter Plots

We can see poor correlation coefficients across all level of comparisons. Mostly event counts related to Verbal Cooperation captured show positive correlation in both datasets.

**For Weekly Aggregated Events**

``` r
###---------------------------------
### Weekly Verbal Cooperation Events 
###---------------------------------


par(mfrow=c(1,2))

cor_pho_icews_vcoop_weekly = ggscatter(pho_icews_vcoop_weekly, x = "pho_vcoop_weekly", y = "icews_vcoop_weekly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Verbal Cooperation Events per Week", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_vcoop_weekly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-12-1.png)

``` r
###-----------------------------------
### Weekly Material Cooperation Events 
###-----------------------------------


cor_pho_icews_mcoop_weekly = ggscatter(pho_icews_mcoop_weekly, x = "pho_mcoop_weekly", y = "icews_mcoop_weekly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Material Cooperation Events per Week", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
 
par(mfrow=c(1,2))                                                                    
                                  
cor_pho_icews_mcoop_weekly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-12-2.png)

``` r
###------------------------------
### Weekly Verbal Conflict Events 
###------------------------------


cor_pho_icews_vcon_weekly = ggscatter(pho_icews_vcon_weekly, x = "pho_vcon_weekly", y = "icews_vcon_weekly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Verbal Conflict Events per Week", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_vcon_weekly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-12-3.png)

``` r
###--------------------------------
### Weekly Material Conflict Events 
###--------------------------------


cor_pho_icews_mcon_weekly = ggscatter(pho_icews_mcon_weekly, x = "pho_mcon_weekly", y = "icews_mcon_weekly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Material Conflict Events per Week", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_mcon_weekly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-12-4.png)

**For Monthly Aggregated Events**

``` r
###----------------------------------
### Monthly Verbal Cooperation Events 
###----------------------------------

par(mfrow=c(1,2))

cor_pho_icews_vcoop_monthly = ggscatter(pho_icews_vcoop_monthly, x = "pho_vcoop_monthly", y = "icews_vcoop_monthly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Verbal Cooperation Events per Month", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_vcoop_monthly
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-13-1.png)

``` r
###------------------------------------
### Monthly Material Cooperation Events 
###------------------------------------

cor_pho_icews_mcoop_monthly = ggscatter(pho_icews_mcoop_monthly, x = "pho_mcoop_monthly", y = "icews_mcoop_monthly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Material Cooperation Events per Month", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
 
par(mfrow=c(1,2))                                                                    
                                  
cor_pho_icews_mcoop_monthly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-13-2.png)

``` r
###-------------------------------
### Monthly Verbal Conflict Events 
###-------------------------------

cor_pho_icews_vcon_monthly = ggscatter(pho_icews_vcon_monthly, x = "pho_vcon_monthly", y = "icews_vcon_monthly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Verbal Conflict Events per Month", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_vcon_monthly
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-13-3.png)

``` r
###---------------------------------
### Monthly Material Conflict Events 
###---------------------------------


cor_pho_icews_mcon_monthly = ggscatter(pho_icews_mcon_monthly, x = "pho_mcon_monthly", y = "icews_mcon_monthly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Material Conflict Events per Month", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_mcon_monthly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-13-4.png)

**For Yearly Aggregated Events**

``` r
par(mfrow=c(1,2))

###---------------------------------
### Yearly Verbal Cooperation Events 
###--------------------------------- 

cor_pho_icews_vcoop_yearly = ggscatter(pho_icews_vcoop_yearly, x = "pho_vcoop_yearly", y = "icews_vcoop_yearly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Verbal Cooperation Events per Year", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_vcoop_yearly
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-14-1.png)

``` r
###-----------------------------------
### Yearly Material Cooperation Events  
###-----------------------------------


cor_pho_icews_mcoop_yearly = ggscatter(pho_icews_mcoop_yearly, x = "pho_mcoop_yearly", y = "icews_mcoop_yearly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Material Cooperation Events per Year", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
 
par(mfrow=c(1,2))                                                                    
                                  
cor_pho_icews_mcoop_yearly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-14-2.png)

``` r
###------------------------------
### Yearly Verbal Conflict Events   
###------------------------------

cor_pho_icews_vcon_yearly  = ggscatter(pho_icews_vcon_yearly , x = "pho_vcon_yearly", y = "icews_vcon_yearly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Verbal Conflict Events per Year", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_vcon_yearly
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-14-3.png)

``` r
###--------------------------------
### Yearly Material Conflict Events   
###--------------------------------


cor_pho_icews_mcon_yearly = ggscatter(pho_icews_mcon_yearly, x = "pho_mcon_yearly", y = "icews_mcon_yearly",
                                       color = "blue", shape = 21, size = 3,
                                       add = "reg.line",
                                       add.params = list(color = "blue", fill = "lightblue"),
                                       conf.int = TRUE, 
                                       cor.coef = TRUE, 
                                       cor.method = "pearson") +

                                      labs(title = "Material Conflict Events per Year", 
                                        x = "Phoenix (1995-2015)",
                                        y = "ICEWS (1995-2015)") +
                                        theme(plot.title = element_text(size=15, 
                                                                face="bold", 
                                                                #family="American Typewriter",
                                                                color="black",
                                                                hjust=0.5, lineheight=1.2),
                                                                axis.text = element_text(size = 12, vjust=.5)) 
                                                                     
                                  
cor_pho_icews_mcon_yearly 
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-14-4.png)

### Dyadic Networks

**Syria Events**

Here I'm showing dyadic links and average Goldstein scores between Syria and other actors. Distances between nodes are a function of average Goldstein scores between them.

**Phoenix**

``` r
Phoenix_SYR = read.csv("Phoenix_SYR.csv")
#names(Phoenix_SYR)

i = data.frame(unique(Phoenix_SYR$source_root))
names(i) = c("actors")
j = data.frame(unique(Phoenix_SYR$target_root))
names(j) = c("actors")
nodes_pho = rbind(i, j)

nodes_pho = unique(nodes_pho$actors)
edges_pho = Phoenix_SYR

source_summary = edges_pho %>% group_by(source_root) %>% filter(source_root != "---") %>% count()
names(source_summary) = c("Source", "n")

target_summary = edges_pho %>% group_by(target_root) %>% filter(target_root != "---") %>% count()
names(target_summary) = c("Target", "n")

node_summary = merge(source_summary, target_summary, by.x = "Source", by.y="Target")

colnames(node_summary)[1] = "Actors"
node_summary$frequency = sort(node_summary$n.x + node_summary$n.y, decreasing = T)
#plot(node_summary$Actors, node_summary$frequency)

edges_pho = edges_pho[,c(9,13,19:ncol(Phoenix_SYR))]
edges_pho$source_root = as.character(edges_pho$source_root)
edges_pho$target_root = as.character(edges_pho$target_root)

graph <- graph_from_data_frame(edges_pho, directed = TRUE, vertices = nodes_pho)
plot(graph, main = "Dyadic Links between Syria and Other Actors \n Phoenix (1995-2015)")
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-15-1.png)

``` r
layout <- layout_with_fr(graph, weights = graph$goldstein)

dev.new()
plot(graph,
     layout = layout,
     vertex.label = V(graph)$name,
     vertex.size= 2.5,
     vertex.label.cex = 0.5,
     edge.arrow.size = 0.1
     )
```

**ICEWS**

``` r
icews = read.csv("icews_SYR.csv")
#names(icews)

i = data.frame(unique(icews$source_root))
names(i) = c("actors")
j = data.frame(unique(icews$target_root))
names(j) = c("actors")
nodes_icews = rbind(i, j)

nodes_icews = unique(nodes_icews$actors)
edges_icews = icews

source_summary = edges_icews %>% group_by(source_root) %>% filter(source_root != "---") %>% count()
names(source_summary) = c("Source", "n")

target_summary = edges_icews %>% group_by(target_root) %>% filter(target_root != "---") %>% count()
names(target_summary) = c("Target", "n")

node_summary = merge(source_summary, target_summary, by.x = "Source", by.y="Target")

colnames(node_summary)[1] = "Actors"
node_summary$frequency = sort(node_summary$n.x + node_summary$n.y, decreasing = T)
#plot(node_summary$Actors, node_summary$frequency)

edges_icews = edges_icews[,c(6,9,13:ncol(icews))]
edges_icews$source_root = as.character(edges_icews$source_root)
edges_icews$target_root = as.character(edges_icews$target_root)

graph <- graph_from_data_frame(edges_icews, directed = TRUE, vertices = nodes_icews)
plot(graph, main = "Dyadic Links between Syria and Other Actors \n ICEWS (1995-2015)")
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-16-1.png)

``` r
layout <- layout_with_fr(graph, weights = graph$goldstein)

dev.new()
plot(graph,
     layout = layout,
     vertex.label = V(graph)$name,
     vertex.size= 2.5,
     vertex.label.cex = 0.5,
     edge.arrow.size = 0.1
     )
```

**Georgia Events**

Similarly, here I'm showing dyadic links and average Goldstein scores between Georgia and other actors. Distances between nodes are a function of average Goldstein scores between them.

**Phoenix**

``` r
Phoenix_Geo = read.csv("Phoenix_Geo.csv")
#names(Phoenix_Geo)

i = data.frame(unique(Phoenix_Geo$source_root))
names(i) = c("actors")
j = data.frame(unique(Phoenix_Geo$target_root))
names(j) = c("actors")
nodes_pho = rbind(i, j)

nodes_pho = unique(nodes_pho$actors)
edges_pho = Phoenix_Geo

source_summary = edges_pho %>% group_by(source_root) %>% filter(source_root != "---") %>% count()
names(source_summary) = c("Source", "n")

target_summary = edges_pho %>% group_by(target_root) %>% filter(target_root != "---") %>% count()
names(target_summary) = c("Target", "n")

node_summary = merge(source_summary, target_summary, by.x = "Source", by.y="Target")

colnames(node_summary)[1] = "Actors"
node_summary$frequency = sort(node_summary$n.x + node_summary$n.y, decreasing = T)
#plot(node_summary$Actors, node_summary$frequency)

edges_pho = edges_pho[,c(10,14,20:ncol(Phoenix_Geo))]
edges_pho$source_root = as.character(edges_pho$source_root)
edges_pho$target_root = as.character(edges_pho$target_root)

graph <- graph_from_data_frame(edges_pho, directed = TRUE, vertices = nodes_pho)
plot(graph, main = "Dyadic Links between Georgia and Other Actors \n Phoenix (1995-2015)")
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-17-1.png)

``` r
layout <- layout_with_fr(graph, weights = graph$goldstein)

dev.new()
plot(graph,
     layout = layout,
     vertex.label = V(graph)$name,
     vertex.size= 2.5,
     vertex.label.cex = 0.5,
     edge.arrow.size = 0.1
     )
```

**ICEWS**

``` r
icews_GEO = read.csv("icews_GEO.csv")
#names(icews_GEO)

i = data.frame(unique(icews_GEO$source_root))
names(i) = c("actors")
j = data.frame(unique(icews_GEO$target_root))
names(j) = c("actors")
nodes_icews = rbind(i, j)

nodes_icews = unique(nodes_icews$actors)
edges_icews = icews_GEO

source_summary = edges_icews %>% group_by(source_root) %>% filter(source_root != "---") %>% count()
names(source_summary) = c("Source", "n")

target_summary = edges_icews %>% group_by(target_root) %>% filter(target_root != "---") %>% count()
names(target_summary) = c("Target", "n")

node_summary = merge(source_summary, target_summary, by.x = "Source", by.y="Target")

colnames(node_summary)[1] = "Actors"
node_summary$frequency = sort(node_summary$n.x + node_summary$n.y, decreasing = T)
#plot(node_summary$Actors, node_summary$frequency)

edges_icews = edges_icews[,c(6,9,13:ncol(icews_GEO))]
edges_icews$source_root = as.character(edges_icews$source_root)
edges_icews$target_root = as.character(edges_icews$target_root)

graph <- graph_from_data_frame(edges_icews, directed = TRUE, vertices = nodes_icews)
plot(graph, main = "Dyadic Links between Georgia and Other Actors \n ICEWS (1995-2015)")
```

![](SoDA-502_Project_Arif_files/figure-markdown_github/unnamed-chunk-18-1.png)

``` r
layout <- layout_with_fr(graph, weights = graph$goldstein)

dev.new()
plot(graph,
     layout = layout,
     vertex.label = V(graph)$name,
     vertex.size= 2.5,
     vertex.label.cex = 0.5,
     edge.arrow.size = 0.1
     )
```

Conclusion
----------

Overall temporal distribution, frequency distribution of top actors, and event types for major events (i.e., Syria and Georgia) are reasonably similar in both datasets. However, correlation coefficients are very low for overall event records corresponding to four major event types. The factors that contributed to this divergence need to be investigated.
