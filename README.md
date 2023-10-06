#part 1
from bs4 import BeautifulSoup
import urllib.request

# Check if the article is a press release
def is_press(soup):
    press_release_link = soup.find('a', href=lambda href: href and href.startswith("/en/press-release"), hreflang="en", text="Press Release")
    return press_release_link is not None

seed_url = "https://press.un.org/en"

urls = [seed_url]  # Queue of URLs to crawl
press_releases = [] # Store found press releases

maxNumUrl = 10  # Set the maximum number of URLs to visit
print("Starting with url=" + str(urls))
while urls and len(press_releases) < maxNumUrl:
    # Dequeue a URL from urls and try to open and read it
    try:
        curr_url = urls.pop(0)
        req = urllib.request.Request(curr_url, headers={'User-Agent': 'Mozilla/5.0'})
        webpage = urllib.request.urlopen(req).read()

    except Exception as ex:
        continue  

    # If URL opens, check if it's a press release page and contains "crisis"
    soup = BeautifulSoup(webpage, 'html.parser')

    if is_press(soup) and "crisis" in soup.get_text().lower():
        press_releases.append(curr_url)

    # Extract links
    for tag in soup.find_all('a', href=True):
        child_url = tag['href']
        child_url = urllib.parse.urljoin(seed_url, child_url)
        if seed_url in child_url and child_url not in urls:
            urls.append(child_url)
            

for press_release_url in press_releases:
    print(press_release_url)


#part 2
from bs4 import BeautifulSoup
import urllib.request

# Check if the article is a plenary session
def is_plenary(soup2):
    plenary_session_text = soup2.find('span', class_='ep_name', text="Plenary session")
    return plenary_session_text is not None

seed_url = "https://www.europarl.europa.eu/news/en/press-room"

urls = [seed_url]  # Queue of URLs to crawl
press_releases = [] # Store found press releases

maxNumUrl = 10  # Set the maximum number of URLs to visit

print("Starting with url=" + str(urls))
while urls and len(press_releases) < maxNumUrl:
    # Dequeue a URL from urls and try to open and read it
    try:
        curr_url = urls.pop(0)
        req = urllib.request.Request(curr_url, headers={'User-Agent': 'Mozilla/5.0'})
        webpage = urllib.request.urlopen(req).read()

    except Exception as ex:
        continue  

    # If URL opens, check if it's a press release page and contains "crisis"
    soup = BeautifulSoup(webpage, 'html.parser')

    if is_plenary(soup) and "crisis" in soup.get_text().lower():
        press_releases.append(curr_url)

    # Extract links
    for tag in soup.find_all('a', href=True):
        child_url = tag['href']
        child_url = urllib.parse.urljoin(seed_url, child_url)
        if seed_url in child_url and child_url not in urls:
            urls.append(child_url)
            

for press_release_url in press_releases:
    print(press_release_url)
