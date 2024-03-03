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
// return Element created with provide circumstances
function createElement(elementType, style, link, innerhtml) {
  let elementCreated = document.createElement(elementType);
  elementCreated.href = link;
  elementCreated.innerHTML = innerhtml;
  elementCreated.style = style;
  return elementCreated;
}

var p_f = document.querySelector('.posts_friends');
const request = 'https://ryankert01.github.io/rss-friend/sorted.json';
let d = new Date();
// fetch 會依照 request 去取得資料
fetch(request)
  .then(response => response.json()) // json()會解析回傳的Response物件
  .then(json => {
    // console.log(json);
    for(let i = 0; i < json.length; i++) {
        let currentItem = document.createElement('div'); // div as a element div (a, a, ...)
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

        
        // e = createElement('a', "border-bottom: none;", null, " - ");
        // currentItem.appendChild(e);

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
