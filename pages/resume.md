---
layout: single
title: About Me
excerpt: ""
header:
  overlay_image: /assets/images/g214.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "[Learn more: Deqin, Yunnan, China](../photo_work/index.html)"
  actions:
    - label: "View Resume.pdf"
      url: "../assets/images/resume.pdf"
---

<head>
    <meta charset="utf-8">
    <style>
        .button1 {
            -webkit-transition-duration: 0.5s;
            transition-duration: 0.5s;
            padding: 8px 16px;
            text-align: center;
            background-color: rgba(150,150,150,0.3);
            color: black;
            border: 0px solid #4CAF50;
            border-radius:5px;
        }
        .button1:hover {
            background-color: rgba(255,255,255,0.5);
            color: white;
        }
    </style>
</head>

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

<!-- <button class="button1"> <a  href="#top_of_resume"> Resume </a> </button>
<button class="button1"> [PDF Version](../assets/images/resume.pdf) </button>

<div id="top_of_resume"> </div> -->

<!-- ###### I'm currently a senior studying Computer Engineering at [UIUC](https://ece.illinois.edu/), my interests are Autonomous Vehicles, Computer Vision and AI&ML techniques used on Image Processing. I was advised by a really nice Professor [Sayan Mitra](https://mitras.ece.illinois.edu/) and his group during 2020 Summer, researched on reachability analysis of hybrid system, especially on the software tool [C2E2](http://publish.illinois.edu/c2e2-tool/).

###### I like photography and like to expose my works to more and more people. Currently, I've gained 25,000,000+ views on [Unsplash](https://unsplash.com/@nick19981122). My photos are free to download at Unsplash, you can also view some of them under [Gallery](../photo_work/index.html) on this site. -->

<!-- I'm currently a graduate student in UCLA studying Computer Science, focused on AI and Computer Vision. I received my bachelor's degree from University of Illinois at Urbana Champaign in 2021 as a Computer Engineering student.  -->
<!-- ###### I'm currently a senior studying Computer Engineering at [UIUC](https://ece.illinois.edu/), my interests are Autonomous Vehicles, Computer Vision and AI&ML techniques used on Image Processing. I was advised by a really nice Professor [Sayan Mitra](https://mitras.ece.illinois.edu/) and his group during 2020 Summer, researched on reachability analysis of hybrid system, especially on the software tool [C2E2](http://publish.illinois.edu/c2e2-tool/). -->

<!-- In my spare time, I love photography and like to share my works. Currently, I've got 130,000,000+ views on [Unsplash](https://unsplash.com/@nick19981122), and became top-1000 contributer on the platform. All of my newest works will be uploaded to Unsplash and are free to download, you can also view some of them under [Gallery](../photo_work/index.html) on this site, though this site may not get updated as often as on Unsplash. -->

## Relevant Courses

| Course (UCLA)  | Content |
| ------------- | ------------- |
| CS 269-3  | [Seminar in Computer Vision: Deformable Contour Models](https://ccle.ucla.edu/course/view/21F-COMSCI269-3) |
| CS M226  | [Machine Learning in Bioinformatics](https://ccle.ucla.edu/course/view/21F-COMSCIM226-1) |
| MATH 273A  | [Optimization and Calculus of Variations: Basic Optimization Theory](https://ccle.ucla.edu/course/view/21F-MATH273A-1) |

| Course (UIUC)  | Content |
| ------------- | ------------- |
| ECE110/120 | Intro to [Electronics](https://ece.illinois.edu/academics/courses/profile/ECE110)/[Computing](https://ece.illinois.edu/academics/courses/profile/ECE120)  |
| ECE210/310 | [Analog](https://ece.illinois.edu/academics/courses/profile/ECE210)/[Digital](https://ece.illinois.edu/academics/courses/profile/ECE310) Signal Processing |
| ECE313 | [Probability with Engineering Applications](https://ece.illinois.edu/academics/courses/profile/ECE313) |
| CS173  | [Discrete Structure](https://cs.illinois.edu/courses/profile/CS173) |
| CS225  | [Data Structure](https://cs.illinois.edu/courses/profile/CS225) |
| ECE385 | [Digital System Lab](https://ece.illinois.edu/academics/courses/profile/ECE385) |
| ECE391 | [Operating System](https://ece.illinois.edu/academics/courses/profile/ECE391) |
| ECE448 | [Artificial Intelligence](https://ece.illinois.edu/academics/courses/profile/ECE448) |
| ECE498 | [Principle for Safe Autonomy](https://publish.illinois.edu/safe-autonomy/) |
| CS411  | [Database Systems](https://cs.illinois.edu/academics/courses/CS411) |
| ECE470 | [Intro to Robotics](https://publish.illinois.edu/ece470-intro-robotics/syllabus/) |
| ECE417 | [Multimedia Signal Processing](https://courses.engr.illinois.edu/ece417/fa2020/) |
| CS374  | [Algorithms & Models of Computation](https://courses.engr.illinois.edu/cs374/fa2020/) |
| CS445  | [Computational Photography](https://courses.engr.illinois.edu/cs445/fa2020/) |
| CS446  | [Machine Learning](https://relate.cs.illinois.edu/course/CS446-fa20/) |
| ECE411  | [Computer Organization and Design](https://courses.grainger.illinois.edu/ece411/fa2021/course.html) |


## Contact

| Email:  | zb3@g.ucla.edu  |
| WeChat: | zbao_98  |

<sub>P.S. I'm open to any opportunities, please feel free to reach out to me!</sub>  







<!--
---
#### Education

---

**University of Illinois at Urbana-Champaign** - *Aug 2017 - May 2021*
<br>*- Champaign, IL*
<br>Bachelor of Science in *Computer Engineering*	- GPA: 3.76 / 4.00


* Relevant Courseworks: Data Structure, Digital System Lab, Computer System & Programming, Probability with Engineering Application, Analog/Digital Signal Processing...

---
#### Experience

---

**Malu Innovation**																													- **Shanghai, China**

*Software Engieering Intern, R&D Department*																	- *June 2019 - July 2019*

* Extracting and processing data from warehouse database, optimizing warehouse storage location.
* Extracting and transforming laser scan data from Lidar into usable data.
* Assisting R&D department, communicating between colleagues.

**Prevail Optoelectronics Equipment Co.,LTD**													        	- **Hangzhou, China**

*Maintenance Assistant*																							   	 -  *July 2018 - August 2018*

* Inspect damaged outdoor trunk amplifier (used for TV signal transmission), replace out malfunctioning or burned chips, transistors and fuses.
* Using multimeter and frequency analyzer to examine the circuit board and signal functionality.
* Assisting maintenance team, recording repair histories.

---
#### Project

---


<button class="button1">
**[Stickman Badminton](https://github.com/bznick98/ECE385/tree/master/Final_Project)**
</button>
**Video Game based on FPGA programming** -
**Champaign, IL**

* Implementing the game in hardware, supporting multiplayer using two keyboards(USB & PS/2).
* Implementing game graphics in frame buffer, connecting with VGA monitor.
* Complex game logic and state machine.


<button class="button1">
**[Car-Industry Database File Reader](https://github.com/CrysisDeu/malu_intern/tree/master/EXCEL_PROJECT_NEW)**
</button>																				- **Shanghai, China**

* Utilize file I/O, data structure and STL in C++ to process data information from car-industry data file.
* Organize information(such as auto parts and daily operation information) in hash-maps, search data in short time.

---
#### Activities

---

**Champaign Photography Association**																	- *September 2017 - Present*

* Organized a photography exhibition at a local coffee shop.
* Worked as an authorized agent in the association, make room reservations for activities.
* Developed a [time-lapse video](https://www.youtube.com/watch?v=D7_J1bN1dOU) with other members in the organization.

---
#### Skills

---

* C++, C, SystemVerilog, LC-3, MATLAB, Ubuntu, jekyll, Markdown, Arduino, Adobe Lightroom, Final Cut Pro, <button class="button1">[Photography](photo_work.html)</button> -->
