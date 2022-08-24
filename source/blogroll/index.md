---
title: Blogroll
date: 2022-08-25 22:59:02
comments: false
---

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
        d = new Date(json[i].date);
        let e = document.createElement('a');
        e.href = "https://www.ryankert.cc/";
        e.innerHTML = json[i].title;
        p_f.appendChild(e);
        e = document.createElement('br');
        p_f.appendChild(e);
        // p_f.append("[app](https://www.ryanket.cc/)")
    }
  }) 
</script>
