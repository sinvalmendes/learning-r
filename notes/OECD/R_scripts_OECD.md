### OECD Investment by Sector dataframe
`https://data.oecd.org/gdp/investment-by-sector.htm#indicator-chart`

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
df <- read_csv("dev/github/R/datasets/OECD/DP_LIVE_18072019202517650.csv")
View(df)
```

### Basic barplot
```
p <-ggplot(data=df, aes(x=reorder(LOCATION, Value), y=Value)) +
    geom_bar(stat="identity")
p

# Horizontal bar plot
p + coord_flip()
```

### Manipulating dataframe
`my_data <- as_tibble(df)`

### Selecting only 4 tables
`my_data <- my_data %>% select(LOCATION, SUBJECT, TIME, Value)`

### Filtering dataset by data about Household
`my_data_only_household <- my_data[my_data$SUBJECT == 'HH',]`

### Filtering dataset by data about Japan
`my_data_only_japan <- my_data_only_household[my_data_only_household$LOCATION == 'JPN',]`


### Basic barplot
```
p <-ggplot(data=my_data_only_japan, aes(x=TIME, y=Value)) +
    geom_bar(stat="identity")
```

### Analysing Investiment of Japan in General Government over the years
```
my_data_only_gg <- filter(df, SUBJECT == "GG")
my_data_only_japan <- filter(my_data_only_gg, LOCATION == "JPN")

p <-ggplot(data=my_data_only_japan, aes(x=TIME, y=Value)) + #plot my_data_only_japan with x=TIME and y=Value
    geom_bar(stat="identity") + #barplot
    theme(axis.text.x = element_text(angle = 90, vjust = 0.5)) + #puts x labels in 90 vertical
    xlab("Years") + ylab("Value in billions") + #labels for x and y axis
    scale_x_discrete(limits=c(my_data_only_japan$TIME)) #very important because will make all values in x discrete and based on the existing values of TIME column, will not hide anything

p
```



