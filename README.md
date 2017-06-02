# web_scraping
scraping a data from weather forecast site
import requests
from bs4 import BeautifulSoup
import pandas as pd
page = requests.get('http://forecast.weather.gov/MapClick.php?lat=37.7772&lon=-122.4168#.WTEX32iGNPY')

soup = BeautifulSoup(page.content,'lxml')

seven_day = soup.find(id="seven-day-forecast")
forecast_items = seven_day.find_all(class_='tombstone-container')

period_tags = seven_day.select('.tombstone-container .period-name')
periods = [pt.get_text() for pt in period_tags]
print(periods)

sd_tags = seven_day.select('.tombstone-container .short-desc')

short_descs = [sd.get_text() for sd in sd_tags]
print(short_descs)


temp_tags = seven_day.select('.tombstone-container .temp')

temps = [t.get_text() for t in temp_tags]
print(temps)

desc_tags = seven_day.select('.tombstone-container img')

descs=[d["title"] for d in desc_tags]
print(descs)

weather = pd.DataFrame({
      "period" : periods,
      "short-desc" : short_descs,
      "temp" : temps,
      "desc" : descs


}

)
print(weather)
