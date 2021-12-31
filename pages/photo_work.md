---
title: Photography
layout: splash
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: "../assets/images/star.jpg"
  actions:
    - label: "Unsplash (Free & Downloadable)"
      url: "https://unsplash.com/@nick19981122"
    - label: "Videography"
      url: "../pages/videos/index.html"

intro:
  - excerpt: "Top-1000 **Unsplash** Contributor"
  - excerpt: "**Getty Images** Contributor"

gallery:
  - url: ../assets/images/duku.jpg
    image_path: ../assets/images/duku.jpg
    alt: "Duku - G217, Xinjiang"
    title: "G217 - 独库公路南段, 新疆."

  - url: ../assets/images/xiata.jpg
    image_path: ../assets/images/xiata.jpg
    alt: "Xiata, Xinjiang"
    title: "夏塔古道, 昭苏, 新疆."

  - url: ../assets/images/xiapu.jpg
    image_path: ../assets/images/xiapu.jpg
    alt: "Tantu, Xiapu"
    title: "滩涂, 霞浦福建 Xiapu, Fujian."

  - url: ../assets/images/BlueFire.jpg
    image_path: ../assets/images/BlueFire.jpg
    alt: "BlueFire"
    title: "Blue Fire at Ijen, Java, Indonesia - In the crater of Mount Ijen."

  - url: ../assets/images/FoggyBromo.jpg
    image_path: ../assets/images/FoggyBromo.jpg
    alt: "FoggyBromo"
    title: "Foggy Mount Bromo - It was a foggy day and we weren't able to see the full Mount Bromo."

  - url: ../assets/images/mzm.jpg
    image_path: ../assets/images/mzm.jpg
    title: "Mianzimu (缅茨姆) "
    excerpt: "One of summits in the Meili Xue Shan of Yunnan province, China"

  - url: ../assets/images/baima.jpg
    image_path: ../assets/images/baima.jpg
    title: "Baima Snow Mountain "
    excerpt: "Summits of Baima Snow Mountain in Yunnan province, China"

  - url: ../assets/images/village.jpg
    image_path: ../assets/images/village.jpg
    title: "Village"
    excerpt: "Southeastern village in China."

  - url: ../assets/images/g214.jpg
    image_path: ../assets/images/g214.jpg
    title: "Views on G214"
    excerpt: "On the road(G214) to Deqin."

  - url: ../assets/images/FoggyBromoHorse.jpg
    image_path: ../assets/images/FoggyBromoHorse.jpg
    alt: "FoggyBromo"
    title: "Foggy Mount Bromo - The man with the horse in the fog."

  - url: ../assets/images/meili.jpg
    image_path: ../assets/images/meili.jpg
    title: "Meili (梅里) "
    excerpt: "Golden hour, Meili Xue Shan of Yunnan province, China"

  - url: ../assets/images/trump.jpg
    image_path: ../assets/images/trump.jpg
    title: "Trump Tower"
    excerpt: "A street view on a bridge in downtown Chicago, Illinois."

  - url: ../assets/images/lake.jpg
    image_path: ../assets/images/lake.jpg
    alt: "Lake Da Qaidam"
    title: "Lake Da Qaidam"
    excerpt: "Took this panorama at 3000m altitude, Da Qaidam, Qinghai Northwestern China."

  - url: ../assets/images/st_ml.jpg
    image_path: ../assets/images/st_ml.jpg
    title: "Star Trail Meili"
    excerpt: "2hr Star Trail at Meili Snow Mountain of Yunnan province, China"

  - url: ../assets/images/menyuan.jpg
    image_path: ../assets/images/menyuan.jpg
    alt: "Menyuan, West China"
    title: "Menyuan, West China"
    excerpt: "Maginificent scenery in the West part of China."

  - url: ../assets/images/IjenMount.jpg
    image_path: ../assets/images/IjenMount.jpg
    alt: "IjenMount"
    title: "Mount Ijen"
    excerpt: "Standing at the edge of the crater."

  - url: ../assets/images/kremlin.jpg
    image_path: ../assets/images/kremlin.jpg
    title: "Kremlin"
    excerpt: "A scene at Kremlin, Moscow Russia."

  - url: ../assets/images/wnd.jpg
    image_path: ../assets/images/wnd.jpg
    title: "Wu Nong Ding"
    excerpt: "A small and peaceful village under Meili Snow Mountain of Yunnan province, China"

  - url: ../assets/images/star.jpg
    image_path: ../assets/images/star.jpg
    alt: "Star Trail"
    title: "Star Trail"
    excerpt: "Long Exposure at a corn field in Illinois."


  - url: ../assets/images/smoke-bw.jpg
    image_path: ../assets/images/smoke-bw.jpg
    title: "Smoke B&W"
    excerpt: "A heating plant in winter Champaign, Illinois."


  - url: ../assets/images/atlanta.jpg
    image_path: ../assets/images/atlanta.jpg
    title: "Street View in Atlanta"
    excerpt: "A street view in downtown Atlanta, Georgia."


  - url: ../assets/images/night-chicago.jpg
    image_path: ../assets/images/night-chicago.jpg
    title: "Night Chicago"
    excerpt: "A night view at 360 Chicago in downtown Chicago, Illinois."
 
---
{% include feature_row id="intro" type="center" %}
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

{% include gallery layout = 'third' %}


<!-- {% include feature_row id="feature_row2" type="left" %}

{% include feature_row id="feature_row3" type="right" %}

{% include feature_row id="feature_row4" type="center" %} -->

<link rel="shortcut icon" type="image/png" href="favicon.png"> 

![Champaign Photography](../assets/images/XBYL.png)
