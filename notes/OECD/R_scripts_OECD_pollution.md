### OECD Environment - Pollution 
`https://data.oecd.org/air/air-and-ghg-emissions.htm`

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
df_only_co2_aus_tonne_cap <- filter(df_only_co2_aus, measure == "TONNE_CAP")

```

### Basic barplot
```
p1 <-ggplot(data=df_only_co2_aus_tonne_cap, aes(x=time, y=value)) +
    geom_line()

```

