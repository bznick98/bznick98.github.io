---
layout: single
title: 
header:
  image: /assets/images/slowmo_boomerang.gif
  image_description: "Baima Snow Mountain"
  caption: "[Learn more: Deqin, Yunnan, China](/pages/photo_work/index.html)"

---

#### Welcome, 
I'm currently a graduate student in UCLA studying Computer Science, focused on AI and Computer Vision. Before that, I received my bachelor's degree from University of Illinois at Urbana Champaign in 2021 as a Computer Engineering student.
<!-- ###### I'm currently a senior studying Computer Engineering at [UIUC](https://ece.illinois.edu/), my interests are Autonomous Vehicles, Computer Vision and AI&ML techniques used on Image Processing. I was advised by a really nice Professor [Sayan Mitra](https://mitras.ece.illinois.edu/) and his group during 2020 Summer, researched on reachability analysis of hybrid system, especially on the software tool [C2E2](http://publish.illinois.edu/c2e2-tool/). -->


In my spare time, I love photography and like to share my works. Currently, I am a top-1000 contributer on the platform named [Unsplash](https://unsplash.com/@nick19981122), also a Getty Images Contributor. All of my recent photos will be uploaded to Unsplash and are free to download.

<style>
    #stat{
        font-family:    'Trebuchet MS', sans-serif;
        font-size:      20px;
        font-weight:    bold;
    }
    #views, #downloads{
        color: gold;
        font-family:    'Courier New', monospace;
        font-size:      15px;
        font-weight:    bold;
    }
    #vtxt, #dtxt{
        font-family:    'Courier New', monospace;
        font-size:      15px;
        font-weight:    bold;
    }
</style>
<center>
<div id="stat"> Unsplash Stats </div>
<span id="views"></span> <span id="vtxt"> views</span> <br>
<span id="downloads"></span> <span id="dtxt"> downloads</span>
</center>

<script>
async function loadStats() {
  const url = "https://api.unsplash.com/users/nick19981122/statistics/?client_id=6t0qRfV_gaM3em6bzhVAZAg1PNl1vxOCTaqGWorNU5A"
  const response = await fetch(url);
  const stats = await response.json();
  // let my_stats = JSON.parse(stats);
  console.log(stats); 
  console.log("Displaying Views and Downloads");
  console.log(stats.views.total);
  console.log(stats.downloads.total);
  // write to html
  document.getElementById('views').innerHTML = stats.views.total;
  document.getElementById('downloads').innerHTML = stats.downloads.total;
};
loadStats();
</script>


<!-- <h1>
<p align="center"> Thanks for stopping by! <br /> Check out <a href="pages/resume/index.html">About</a>
 for more information. </p>
</h1> -->

<link rel="shortcut icon" type="image/png" href="favicon.png"> 


<script type="text/javascript" src="//rf.revolvermaps.com/0/0/8.js?i=5vjix09k2my&amp;m=0&amp;c=ff0000&amp;cr1=ffffff&amp;f=lucida_sans_unicode&amp;l=49&amp;hi=50" async="async"></script>

<sub>P.S. I'm open to any opportunities, feel free to reach me plz!</sub>       


