---
title: Download Google font and use it in offline html
tags: google-font
categories: [google-font]
date: 2022/4/18 20:46:25
---

## Download Google font and use it in offline html

**Steps required**

1. find your beloved font at google fonts.
2. select all font styles that you might want to have.
3. find its' url ex:`https://fonts.googleapis.com/css2?family=Caveat:wght@400;500;600;700&display=swap`, which will be in`To embed a font, copy the code into the <head> of your html`/`<link>`
4. open it in the browser and convert it to `example.css`
5. make a directory "fonts" ex:`mkdir fonts`
6. make a `main.py` in same directory.
7. paste the automate python code in it.
8. comfirm that this directory has three object:
   - [ ] `main.py`
   - [ ] empty directory `fonts`
   - [ ] `example.css`
9. run the python code `python main.py`
10. embed `example.css` in your html file

**`main.py`**

```python
import requests
import re

with open("example.css", "r") as f:
    text = f.read()
    urls = re.findall(r'(https?://[^\)]+)', text)

for url in urls:
    filename = url.split("/")[-1]
    r = requests.get(url)
    with open("./fonts/" + filename, "wb") as f:
        f.write(r.content)
    text = text.replace(url, "'./fonts/" + filename +"'")

with open("example.css", "w") as f:
        f.write(text)
```

credit: **duydb**, **Wytamma Wirth** and **me** who modified it to the better

resourse: https://stackoverflow.com/questions/15930003/downloading-a-google-font-and-setting-up-an-offline-site-that-uses-it
