
library(tidymodels)

# Load tidyverse
library(tidyverse)

url <- "https://dax-cdn.cdn.appdomain.cloud/dax-noaa-weather-data-jfk-airport/1.1.4/noaa-weather-sample-data.tar.gz"

download.file(url, destfile = "noaa-weather-sample-data.tar.gz")

untar("noaa-weather-sample-data.tar.gz", tar = "internal")

project_data <- read_csv("noaa-weather-sample-data/jfk_weather_sample.csv")

head(project_data)

glimpse(project_data)
dim(project_data)

modified_df <- project_data %>%
select(HOURLYRelativeHumidity, HOURLYDRYBULBTEMPF, HOURLYPrecip, HOURLYWindSpeed, HOURLYStationPressure)

head(modified_df, 10)

unique(modified_df$HOURLYPrecip)

modified_df$HOURLYPrecip[modified_df$HOURLYPrecip == 'T'] <- '0.0'

unique(modified_df$HOURLYPrecip)
# the last step does not yield the results I want which is to remove "s" values #
