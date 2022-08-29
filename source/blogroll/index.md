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
var p_f = document.querySelector('.posts_friends');
const request = 'https://www.ryankert.cc/rss-friend/sorted.json';
let d = new Date();
// fetch 會依照 request 去取得資料
fetch(request)
  .then(response => response.json()) // json()會解析回傳的Response物件
  .then(json => {
    // console.log(json);
    for(let i = 0; i < json.length; i++) {
        //d = new Date(Number(json[i].year), Number(json[i].month) - 1, Number(json[i].day));
        let currentItem = document.createElement('div'); // div as a element div (a, a, ...)
        d = new Date(json[i].date);
        // sequence
        let e;
        // e = document.createElement('a');
        // e.style = "border-bottom: none;";
        // let num = i + 1;
        // let seq = num.toString();
        // e.innerHTML = seq + ". ";
        // currentItem.appendChild(e);
        // date
        e = document.createElement('a');
        e.style = "border-bottom: none; opacity:65%;";
        let monthAppend = d.getMonth()+1;
        monthAppend = monthAppend.toString();
        monthAppend = monthAppend.length < 2 ? "0" + monthAppend : monthAppend;
        let dayAppend = d.getDate();
        dayAppend = dayAppend.toString();
        dayAppend = dayAppend.length < 2 ? "0" + dayAppend : dayAppend;
        let tempAppend = monthAppend + "-" + dayAppend + " ";
        e.innerHTML = tempAppend;
        
        currentItem.appendChild(e);
        // title + link
        e = document.createElement('a');
        e.href = json[i].link;
        e.innerHTML = json[i].title;
        currentItem.appendChild(e);

        e = document.createElement('a');
        e.style = "border-bottom: none;";
        e.innerHTML = " - ";
        currentItem.appendChild(e);

        // author + link
        e = document.createElement('a');
        e.href = json[i].author.link;
        e.innerHTML = json[i].author.name;
        currentItem.appendChild(e);
        p_f.appendChild(currentItem);
        // p_f.append("[app](https://www.ryanket.cc/)")
    }
  }) 
</script>
