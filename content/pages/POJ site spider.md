---
tags:
- BeautifulSoup
- Python
- Spider
date: 2022-03-22
title: POJ site spider
categories:
lastMod: 2025-10-12
--- 
## Description

Design a crawler for problems of [poj.org](http://poj.org), crawl 100 problem details, and save as \*.json file.
  
## Preparation

The lab can be divided into these sections:

1.  Send a request and receive a response
2.  Parse the response and save the desired data
3.  Repeatedly doing 1 and 2.
  
### Observation on url pattern

The URLs of problems on poj all follow this format:

http://poj.org/problem?id=<id>

where <id> is an integer starting at 1000. So when we only need to send a request to this pattern, when <id> ranges from 1000 to 1099.
  
### Strategies against anti-crawler

As a friendly crawler, we should not put much pressure on poj's server. This is something we should think about before we start coding. To avoid being banned, here are some simple strategies:

1.  Use fake UserAgents.
2.  Sleep for some seconds between 2 requests.
3.  If still get banned, we have to implement an ip-pool.
  
## Coding
  
### Parse response and save the data we want

We can manually obtain the HTML code of websites using Firefox's view-source function. So we will start with the parsing.

In this section, we will import **the BeautifulSoup** package. [Beautiful Soup](http://www.crummy.com/software/BeautifulSoup/) is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work.[Beautiful Soup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)

We can determine the position of our targets by manually analyzing the HTML code.

![](/images/2022/titlehere.png)

Title here

![](/images/2022/otherattr.png)

Other attributes here

We find the corresponding info are all the next siblings of our target string:
```html

<p class="pst">Description</p>
<div class="ptx" lang="en-US">Calculate a+b </div>

To a tag object Tag, we can access its content like this:
```

head\_tag.contents
{% raw %}
  
# \[<title>The Dormouse's story</title>\]

head\_tag.string
  
# 'The Dormouse's story'
{% endraw %}

_Tag.strings_ and _Tag.stripped_ strings are generator types that can be used when there is more than one thing inside the tag.

Also, we can navigate sideways using Tag.next\_sibling.

We create a dictionary, insert our target, and navigate using their tag names.

```python
from bs4 import BeautifulSoup

def html2json(html):
  """ parsing html and save data as json.
  """
  soup = BeautifulSoup(html, 'html.parser')

  target = {"Title", "Time Limit:", "Memory Limit:", "Total Submissions:", "Accepted:", "Description", "Input", "Output", "Sample Input", "Sample Output", "Hint", "Source"}
  JsonDic = {}

  try:
      DealingTag = soup.title
      JsonDic["Title"] = DealingTag.string
      print("Title:", soup.title.string)
  except:
      print("This page has no title.")
  DealingTag = soup.body.find_all('p')
  for tag in DealingTag:
      if tag.string in target:
          try:
              TempString = ''
              for string in tag.next_sibling.strings:
                  TempString = TempString + string
              JsonDic[tag.string] = TempString
          except:
              print("This page has no", tag.string)
  DealingTag = soup.body.find_all('b')
  for tag in DealingTag:
      if tag.string in target:
          JsonDic[tag.string[:-1]] = tag.next_sibling
```

Now that we have a dict containing the desired data, we can simply convert it to a json file using the package json:

```python
import json

#def html2json(html):
def html2json(html, PathToJson):

...

  Jsonfile = open(PathToJson, 'a', encoding = 'utf-8')
  Jsonfile.write(json.dumps(JsonDic, ensure_ascii=False, indent=4, separators=(',', ":")))
  Jsonfile.write("\n")
  Jsonfile.close
```

Here is the output:

![](/images/2022/oneoutput.png)

Json file
  
### Send a request and receive response

Next step, we should try to automatically get the HTML of any given URL. We need to import package **[request](https://docs.python-requests.org/en/latest/)**. As mentioned, we introduce a UA-pool.

```python
def Get_html(url, UA_Pool):
  """ Send request to the given url, return html of the request.
  """
  myheaders = {
      "User-Agent": UA_Pool[random.randint(0, len(UA_Pool) - 1)], 
      "Host": "poj.org",
          }
  print("UA:", myheaders["User-Agent"])
  try:
      client = requests.session()
      response = client.get(url, headers = myheaders)
      html = response.text
      return html
  except:
      print("Request Error")
      pass
```

We don't have much to say about this section. Because the routine is fairly consistent.
  
### Calling these functions

We already have **Get\_html**, which returns the HTML for a given URL, and html2json, which finds our desired data and saves it into json file. Then all we have to do is to repeatedly call them.

We sleep for a random duration of \[3s, 15s\] in each loop. (This is because we only need 100 results, we don't need to crawl as quickly.)

It's worth noting that the UA-pool is generated by [Useragentstring.com](http://useragentstring.com/pages/useragentstring.php). Thanks for its help!

```python
Path = './resultsample.json'
UA_Pool=[ # from useragentstring.com
  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3100.0 Safari/537.36",
  "Mozilla/5.0 (Windows; U; Windows NT 6.1; en-us) AppleWebKit/534.50 (KHTML, like Gecko) Version/5.1 Safari/534.50",
  "Mozilla/5.0 (Windows; U; Windows NT 6.1; rv:2.2) Gecko/20110201",
  "Mozilla/5.0 (X11; U; Linux x86_64; en-US; rv:1.9.0.3) Gecko/2008092814 (Debian-3.0.1-1)",
  "Mozilla/5.0 (Macintosh; U; PPC Mac OS X Mach-O; en-US; rv:1.8.1a2) Gecko/20060512",
  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 Edge/18.19582",
  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.77 Safari/537.36",
  "Mozilla/5.0 (X11; Ubuntu; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2919.83 Safari/537.36",
  "Mozilla/5.0 (compatible, MSIE 11, Windows NT 6.3; Trident/7.0;  rv:11.0) like Gecko",
  "Mozilla/5.0 (X11; Linux; rv:74.0) Gecko/20100101 Firefox/74.0",
      ]

URLPattern = "http://poj.org/problem?id="
for i in range(1000, 1100):
  url = URLPattern + str(i)
  print(url)
  time.sleep(random.uniform(3, 15))
  html = Get_html(url, UA_Pool)
  html2json(html, Path)
```
  
## Result & Summary

![](/images/2022/fullscreenshot.png)

This is the terminal when running

Maybe because [I have tried the Netease Crawler before](/2022/02/08/crawl-song-lyrics-in-netease/)(But the request part did not succeed because I don't know its API, so I had to import a _Cloudmusic_ package), this crawler did not take long(~5 hours, maybe). I didn't try to introduce a cookies-pool this time, but might try next time.
 