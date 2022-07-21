---
title: Youtube Thumbnail Scraping
tags: Scraping
categories: [Scraping]
date: 2022/4/20 20:46:25
---

To crawl thumbnails on YouTube, we use a interesting website that YouTube made for us for quick request for thumnail image.

```
https://i.ytimg.com/vi/{YouTube_video_id}/maxresdefault.jpg
```

Firstly, we convert YouTube url to id by following code, and use "get" methed request the image.

Then, we stored the image in folder you like
using `imagedown(YouTube_video_id, 'folder_name')`

If there's no such folder, then it will auto generate a new one.

```python
import re
import requests
import os

#urls to id
url = "YouTube URL"
exp = "^.*((youtu.be\/)|(v\/)|(\/u\/\w\/)|(embed\/)|(watch\?))\??v?=?([^#&?]*).*"
s = re.findall(exp,url)[0][-1]
thumbnail = f"https://i.ytimg.com/vi/{s}/maxresdefault.jpg"

#image scraping
def imagedown(url, folder):
    try:
        os.mkdir(os.path.join(os.getcwd(), folder))
    except:
        pass
    os.chdir(os.path.join(os.getcwd(), folder))

    name = url
    link = url
    with open(name.replace(' ', '-').replace('/', '') + '.jpg', 'wb') as f:
        im = requests.get(link)
        f.write(im.content)
        print('Writing: ', name)

imagedown(thumbnail, 'image')
```

Credits: [StackoverFlow](https://stackoverflow.com/questions/47730259/installing-urllib-in-python3-6) and [jhnwr](https://github.com/jhnwr)
