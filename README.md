# Open my Soundcloud account and scroll down the page and get the whole html code of the page to get the Tracklist with selenium.


from selenium import webdriver
from selenium.webdriver.common import keys
import time

# write your chromedriver path in the parentheses
Chrome = webdriver.Chrome('/Users/apple/Downloads/chromedriver')
Chrome.get('https://soundcloud.com/negzzzz/sets/negar')
Chrome.maximize_window()

SCROLL_PAUSE_TIME = 0.5

# Get scroll height
last_height = Chrome.execute_script("return document.body.scrollHeight")

while True:
    # Scroll down to bottom
    Chrome.execute_script("window.scrollTo(0, document.body.scrollHeight);")

    # Wait to load page
    time.sleep(SCROLL_PAUSE_TIME)

    # Calculate new scroll height and compare with last scroll height
    new_height = Chrome.execute_script("return document.body.scrollHeight")
    if new_height == last_height:
        break
    last_height = new_height

html = Chrome.page_source

html_list = html.split('trackItem__trackTitle sc-link-dark sc-font-light')

print('len is:',len(html_list)) #len should be equal to the number of song tracks in the playlist

track_list = []
for item in html_list[1:]:
    str = ''
    for char in item[2:]:       
        if char is not '<':
            str += char
        else:
            break
    track_list.append(str)

print('My tracklist is:\n\n',track_list)
