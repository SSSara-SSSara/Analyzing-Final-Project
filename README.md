
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

modified_df <- modified_df %>%
  mutate(HOURLYPrecip = gsub("s", "", HOURLYPrecip))

unique(modified_df$HOURLYPrecip)

glimpse(modified_df)



cleaned_df <- modified_df %>% 

    mutate_if(is.character, as.numeric)

glimpse(cleaned_df)

final_df <- cleaned_df %>% 
rename(
    "relative_humidity" = HOURLYRelativeHumidity,
    "dry_bulb_temp_f" = HOURLYDRYBULBTEMPF,
"precip" = HOURLYPrecip,
"wind_speed" = HOURLYWindSpeed,
"station_pressue" = HOURLYStationPressure)

glimpse(final_df)

set.seed(1234)
data_split <- initial_split(final_df, prop = 4/5)
train_data <- training(data_split)
test_data <- testing(data_split)

ggplot(data = train_data, mapping = aes(x = relative_humidity, y = precip)) +
  geom_boxplot(fill = "bisque",color = "black", alpha = 0.3) +
  geom_jitter(aes(color = 'blue'), alpha=0.2) +
  labs(x = "Relative Humidity", y = "Precipitation") +
  ggtitle("trial") +
  guides(color = FALSE) +
  theme_minimal() 
coord_cartesian(ylim = quantile(train_data$precip, c(0, 5)))


