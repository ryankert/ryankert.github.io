---
title: Collect you and your friends' blog posts via blogroll
tags: [Hexo]
categories: [Web, Static Website Generator]
date: 2022/9/1 20:46:25
---

# Build an Blogroll on Hexo Next

Year ago, I saw many blogger collects their friends blog post via Github Action auto run.
Therefore, I always want to try it out.

Anyways, I build it with typescript. Also, if you have any questions or suggestions, any issues or prs are welcomed.

## Step1: Generate useful Data from RSS-feed

This project is based on rss feed, so if you or your friend blog doesn't have `/feed`, `feed.xml`, `atom.xml`, or any kinds of xml file contains your full blogs' intel, you will have to generate one yourself.

{% note info %}

#### Project URL

https://github.com/ryankert01/rss-friend

{% endnote %}

### How to use it

1. Fork this repository

2. install your friends' rss pages into this file `./_data/friends.json`, in this format:

```json
[
    {
        "title": "Ryan's Blog",
        "link": "https://blog.ryankert.cc/",
        "feed": "https://blog.ryankert.cc/atom.xml"
    },
    ...
]
```

3. Until it generate file sussessfully, it will generate a new branch automatically. Then, you setup your github page to display the branch `gh-pages`.

It will be display at `https://<github-username>.github.io/rss-friend/<file>`.

There are three file generated

```
rss.json       // sorted json year, month, day
sorted.json    // sorted json universal date ex:2022-08-22T17:47:35.000Z
unsort.json    // unsort raw data
```

### Auto Update

at (UTC) 1:00 and 13:00

or at (UTC+8) 9:00 and 21:00

## Step2: Hexo settings

### Setup blogroll page

1. in `./source` folder, add `blogroll/index.md`, change `<github_username>` to your github username.

   **index.md**

```md
---
title: Friends & Blogroll
date: 2022-08-25 22:59:02
comments: false
---

{% note info %}

#### Welcome to Friends & blog

This place collects my friends & some of the nice blogs of my select.

{% endnote %}

<div class="posts_friends"></div>

<script>
function createElement(elementType, style, link, innerhtml) {
  let elementCreated = document.createElement(elementType);
  elementCreated.href = link;
  elementCreated.innerHTML = innerhtml;
  elementCreated.style = style;
  return elementCreated;
}

var p_f = document.querySelector('.posts_friends');
const request = 'https://<github_username>.github.io/rss-friend/sorted.json';
let d = new Date();
fetch(request)
  .then(response => response.json()) 
  .then(json => {
    for(let i = 0; i < json.length; i++) {
        let currentItem = document.createElement('div');
        d = new Date(json[i].date);
        let e;

        // date
        let monthAppend = d.getMonth()+1;
        monthAppend = monthAppend.toString();
        monthAppend = monthAppend.length < 2 ? "0" + monthAppend : monthAppend;
        let dayAppend = d.getDate();
        dayAppend = dayAppend.toString();
        dayAppend = dayAppend.length < 2 ? "0" + dayAppend : dayAppend;
        let tempAppend = monthAppend + "-" + dayAppend + " ";
        
        let margin = 10;
        for (let j = 0; j < tempAppend.length; j++) {
          if(tempAppend[j] === '1')
            margin += 2;
          console.log(margin);
        }
        let style = "border-bottom: none; opacity: 65%; margin: ";
        style += margin.toString() + "px";
        e = createElement(
          'a',
          style, 
          null, 
          tempAppend
        );
        currentItem.appendChild(e);

        // title + link
        e = createElement('a', null, json[i].link, json[i].title);
        currentItem.appendChild(e);


        // author + link
        e = createElement('a', "opacity: 75%;", json[i].author.link, json[i].author.name);
        e.classList.add("e-author");
        currentItem.appendChild(e);
        p_f.appendChild(currentItem);
    }
  }) 
</script>

<style>
.e-author {
  position: absolute; 
  right: 5px;   
}

@media screen and (max-width: 760px) {
  .e-author {
    position: relative;
    left: 15px
  }
}

</style>
```

2. in `./theme/hexo-theme-next/_config.yml`, add Blogroll

```yml
menu:
  ...
  Blogroll: /blogroll/ || fa fa-blog # add
```

## Debug

visit [Hexo Next Docs](https://theme-next.js.org/docs/theme-settings/custom-pages.html) or email me : ryan@ryankert.cc.
