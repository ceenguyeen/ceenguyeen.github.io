---
layout: post
title: Will you accept this rose?
subtitle: A visualization of data on the Bachelor franchise.
thumbnail-img: /assets/img/maxresdefault.jpg
cover-img: /assets/img/roses.jpg
gh-repo: daattali/beautiful-jekyll
tags: [Tableau]
comments: false
---

**A comparison between the Bachelor and Bachelorette**

<script src="https://www.example.com/javascripts/api/tableau-2.js"></script>
<div id="tableauViz"></div>

function initializeViz() {
var placeholderDiv = document.getElementById("tableauViz");
var url = "https://public.tableau.com/profile/anh.cindy.nguyen#!/vizhome/Bachelor_16123269153700/Dashboard2";
var options = {
 width: '600px',
 height: '600px',
 hideTabs: true,
 hideToolbar: true,
 };
viz = new tableau.Viz(placeholderDiv, url, options);
}


