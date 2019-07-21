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

### Analysis of TOP 50 CO2 emission per capita over the years
```
df_only_co2 <- filter(df, subject == "CO2")
df_only_co2_tonnes_cap <- filter(df_only_co2, measure == "TONNE_CAP") 

df_only_co2_tonnes_cap[is.na(df_only_co2_tonnes_cap)] <- 0 #treating the NA in values column
sum_by_location <- aggregate(df_only_co2_tonnes_cap$value, by=list(category=df_only_co2_tonnes_cap$location), FUN=sum) #sum by location
head(arrange(sum_by_location,desc(x)), n = 50)

top_10 <- head(arrange(sum_by_location,desc(x)), n = 10)
top_10


df_only_co2_tonnes_cap_top_10 <-filter(df_only_co2_tonnes_cap, location %in% top_10$category)

p1 <-ggplot(data=df_only_co2_tonnes_cap_top_10, aes(x=time, y=value)) +
    geom_line(aes(linetype=location)) +
    geom_point(size=1) +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) +
    xlab("Years") + ylab("CO2 emission in Tonnes/capita") +
    scale_x_discrete(limits=c(df_only_co2_tonnes_cap_top_10$time))
p1

```