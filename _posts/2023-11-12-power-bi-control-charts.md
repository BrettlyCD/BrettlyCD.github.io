---
layout: post
title: Creating Control Charts in Power BI
subtitle: A Detailed Walkthrough
cover-img: /assets/img/proj-img/sql-cover.jpg
thumbnail-img: /assets/img/powerbi/control-charts/pbi_cover.png
share-img: /assets/img/powerbi/control-charts/pbi_thumb.png
tags: [Power BI, statistics, walkthrough]
---

## Introduction

My company recently made the switch from Tableau to Power BI for our analytics and visualization platform. I have really enjoyed learning the platform so far. The current capabilities and future plans I've seen are really exciting. But one area that has been a struggle for me is control charts. These are a visual tool our group loves to use to track KPIs.

I first learned about control charts, a Lean Six Sigma staple, from a [great post](https://dataremixed.com/2011/09/tom-brady-and-control-charts-with-tableau/) from Ben Jones in their [DataRemixed blog](https://dataremixed.com). In the post he uses Tom Brady's game stats to introduce control charts and how to build them in Tableau. If you work in Tableau or just want to see some great data work, I highly recommend this post.

Unfortunately, I found building a similar viz in Power BI to be much more challenging. There are some add-ins you can use that people have created. But most require a license and/or don't have the customization options I am looking for. So, I started researching and testing and finally found a way to build what I was looking for. It took a lot of time, so I wanted to share this in case it can help anybody same some time.

## Resources

I tend to learn best from reading through different documentation and how-tos and then doing some trial-and-error on my own projects. Here are the resources that really helped me in this build:
- [Towards Data Science Article by Natalie Garces](https://towardsdatascience.com/how-to-create-a-control-chart-in-power-bi-fccc98d3a8f9)
- [Excelerator BI Articel by Matt Allington](https://exceleratorbi.com.au/six-sigma-control-charts-in-power-bi/)
- [Shewhart Individuals Control Chart - Wikipedia](https://en.wikipedia.org/wiki/Shewhart_individuals_control_chart)

Combining the calculation and visualization setps in the articles from Natalie and Matt with the process guidance from Ben and the Wikipedia article, I decided to build a Power BI viz in honor of how I learned in Tableau. So, I built this using stats of one of my favorite quarterbacks: Kurt Warner.

**Data Source:** [Pro Football Reference](https://www.pro-football-reference.com/players/W/WarnKu00/gamelog/)


