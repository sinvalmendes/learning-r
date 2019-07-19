### OECD Environment - Forest 
`https://www.oecd-ilibrary.org/environment/data/oecd-environment-statistics/forest-resources_data-00600-en`

### Install and Import Libraries
```
install.packages("ggplot2")
library(ggplot2)

install.packages("tidyverse")
library(tidyverse)

install.packages("dplyr")
library(dplyr)    

library(readr)
```

### Import dataframe from CSV file
```
df <- read_csv("dev/github/learning-r/datasets/OECD/FOREST_19072019191542708.csv")
View(df)
```

### Lower case of column names
```
var.names <- tolower(colnames(df))
colnames(df) <- var.names
```

### Manipulating dataframe to filter by specific column value
```
df_only_aus <- filter(df, cou == "AUS")
df_only_aus_felling <- filter(df_only_aus, variable == "Fellings")

df_only_dnk <- filter(df, cou == "DNK")
df_only_dnk_felling <- filter(df_only_dnk, variable == "Fellings")

df_only_aus <- filter(df, cou == "AUS")
df_only_aus_felling <- filter(df_only_aus, variable == "Fellings")

df_only_dnk <- filter(df, cou == "DNK")
df_only_dnk_felling <- filter(df_only_dnk, variable == "Fellings")
```

### Basic barplot
```
p1 <-ggplot(data=df_only_aus_felling, aes(x=year, y=value)) +
    geom_bar(stat="identity")

p2 <-ggplot(data=df_only_dnk_felling, aes(x=year, y=value)) +
    geom_bar(stat="identity")
```

### Analysing Felling (M3) of Australia and Denmark over the years
```
df_only_aus <- filter(df, cou == "AUS")
df_only_aus_felling <- filter(df_only_aus, variable == "Fellings")

p1 <-ggplot(data=df_only_aus_felling, aes(x=year, y=value)) +
    geom_bar(stat="identity") +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) +
    xlab("Years") + ylab("Thousands of Cubic Metres") +
    scale_x_discrete(limits=c(df_only_aus_felling$year))


df_only_dnk <- filter(df, cou == "DNK")
df_only_dnk_felling <- filter(df_only_dnk, variable == "Fellings")

p2 <-ggplot(data=df_only_dnk_felling, aes(x=year, y=value)) +
    geom_bar(stat="identity") +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) +
    xlab("Years") + ylab("Thousands of Cubic Metres") +
    scale_x_discrete(limits=c(df_only_dnk_felling$year))

df_aus_and_dnk_felling <- rbind(df_only_aus_felling, df_only_dnk_felling)

p3 <-ggplot(data=df_aus_and_dnk_felling, aes(x=year, y=value)) +
    geom_bar(stat = "identity", aes(fill = country), position = "dodge") +
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) + 
    xlab("Years") + ylab("Thousands of Cubic Metres") +
    scale_x_discrete(limits=c(df_aus_and_dnk_felling$year)) 

```

### Analysing 2015 Felling
```
df_felling <- filter(df, variable == "Fellings")
df_felling_2015 <- filter(df_felling, year == 2015)

p4 <-ggplot(data=df_felling_2015, aes(x=reorder(country, value), y=value)) +
    geom_bar(stat = "identity", aes(fill = country), position = "dodge") +
    theme(axis.text.x = element_text(angle = 70, vjust = 0.5)) + 
    xlab("Countries") + ylab("Thousands of Cubic Metres")
p4
```




