### OECD Environment - Pollution 
`https://data.oecd.org/air/air-and-ghg-emissions.htm`

### Install and Import Libraries
```
install.packages("ggplot2")
library(ggplot2)

install.packages("tidyverse")
library(tidyverse)

install.packages("plyr")
library(plyr)

install.packages("dplyr")
library(dplyr)    

library(readr)
```

### Import dataframe from CSV file
```
df <- read_csv("dev/github/learning-r/datasets/OECD/DP_LIVE_20072019210839700.csv")
View(df)
```

### Lower case of column names
```
var.names <- tolower(colnames(df))
colnames(df) <- var.names
```

### Manipulating dataframe to filter by specific column value
```
df_only_co2 <- filter(df, subject == "CO2")
df_only_co2_aus <- filter(df_only_co2, location == "AUS")

# selecting only one measure otherwise the grap will look very strange
df_only_co2_aus_tonne_cap <- filter(df_only_co2_aus, measure == "TONNE_CAP") 
```

### Basic barplot
```
p1 <-ggplot(data=df_only_co2_aus_tonne_cap, aes(x=time, y=value)) +
    geom_line()

```

### Analysis of Australia CO2 emission over the years
```
p1 <-ggplot(data=df_only_co2_aus_tonne_cap, aes(x=time, y=value)) +
    geom_line() + #line plot
    geom_point(size=1) +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) + #puts x labels in 90 vertical
    xlab("Years") + ylab("CO2 emission in Tonnes/capita") + #labels for x and y axis
    scale_x_discrete(limits=c(df_only_co2_aus_tonne_cap$time)) #very important because will make all values in x discrete and based on the existing values of 'time' column, will not hide anything
p1
```

### Analysis of TOP CO2 emission per capita over the years
```
df_only_co2 <- filter(df, subject == "CO2")
df_only_co2_tonnes_cap <- filter(df_only_co2, measure == "TONNE_CAP") 

df_only_co2_tonnes_cap[is.na(df_only_co2_tonnes_cap)] <- 0 #treating the NA in values column
sum_by_location <- aggregate(df_only_co2_tonnes_cap$value, by=list(category=df_only_co2_tonnes_cap$location), FUN=sum) #sum by location
head(arrange(sum_by_location, desc(x)), n = 50) #sorting 50 of them in descending order

top_10 <- head(arrange(sum_by_location,desc(x)), n = 10) #getting the top 10 locations (descending order)
top_10

df_only_co2_tonnes_cap_top_10 <-filter(df_only_co2_tonnes_cap, location %in% top_10$category) #filtering the original dataset by the top 10 locations

p1 <-ggplot(data=df_only_co2_tonnes_cap_top_10, aes(x=time, y=value, color=location)) +
    geom_line() +
    geom_point() +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) +
    xlab("Years") + ylab("CO2 emission in Tonnes/capita") +
    scale_x_discrete(limits=c(df_only_co2_tonnes_cap_top_10$time))
p1


top_5 <- head(arrange(sum_by_location,desc(x)), n = 5) #getting the top 5
top_5

df_only_co2_tonnes_cap_top_5 <-filter(df_only_co2_tonnes_cap, location %in% top_5$category)

p1 <-ggplot(data=df_only_co2_tonnes_cap_top_5, aes(x=time, y=value, color=location)) +
    geom_line() +
    geom_point() +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) +
    xlab("Years") + ylab("CO2 emission in Tonnes/capita") +
    scale_x_discrete(limits=c(df_only_co2_tonnes_cap_top_5$time))
p1
```

### Analysis CO2 US emissions over the years
```
df_only_co2 <- filter(df, subject == "CO2")
df_only_co2_million_tonnes <- filter(df_only_co2, measure == "MLN_TONNE") 
df_only_co2_million_tonnes[is.na(df_only_co2_million_tonnes)] <- 0 #treating the NA in values column

df_only_co2_million_tonnes_us <- filter(df_only_co2_million_tonnes, location == "USA") 
p1 <-ggplot(data=df_only_co2_million_tonnes_us, aes(x=time, y=value)) +
    geom_line()
```

### Analysis of TOP 10 by absolute CO2 emissions over the years
```
df_only_co2 <- filter(df, subject == "CO2")
df_only_co2_million_tonnes <- filter(df_only_co2, measure == "MLN_TONNE") 
df_only_co2_million_tonnes[is.na(df_only_co2_million_tonnes)] <- 0 #treating the NA in values column

sum_by_location <- aggregate(df_only_co2_million_tonnes$value, by=list(location=df_only_co2_million_tonnes$location), FUN=sum)
#need to transform the value in thousands of million of tonnes to just million of tonnes

top_10 <- head(arrange(sum_by_location,desc(x)), n = 10)
names(top_10)[names(top_10) == "x"] <- "value"


#sorting the dataset in ascending order of value
top_10 <- top_10[order(top_10$value), ]
top_10$location <- factor(top_10$location, levels = top_10$location[order(top_10$value)])


p1 <-ggplot(data=top_10, aes(x=location, y=value, order = -as.numeric(value), fill=location)) + 
    geom_bar(stat="identity") +
    scale_fill_brewer(palette="RdYlGn", direction = -1) +
    labs(title = "CO2 emissions total by country/group",
       subtitle = "1960-2016",
       caption = "",
       tag = "Figure 1",
       x = "Countries and Groups",
       y = "CO2 emission (million of tonnes)",
       colour = "asd")

p1
```
Would be nice to have the bar plot above but with a splitted bar by location, or a PIE chart.