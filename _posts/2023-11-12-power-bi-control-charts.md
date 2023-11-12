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

**You can find a copy of my Power BI workbook here:** [Google Drive Resources](https://drive.google.com/drive/folders/1F63tmNQSSIaH6dxK_7b0jBsKmMIEAW7_?usp=share_link)

## Resources

I tend to learn best from reading through different documentation and how-tos and then doing some trial-and-error on my own projects. Here are the resources that really helped me in this build:
- [Towards Data Science Article by Natalie Garces](https://towardsdatascience.com/how-to-create-a-control-chart-in-power-bi-fccc98d3a8f9)
- [Excelerator BI Articel by Matt Allington](https://exceleratorbi.com.au/six-sigma-control-charts-in-power-bi/)
- [Shewhart Individuals Control Chart - Wikipedia](https://en.wikipedia.org/wiki/Shewhart_individuals_control_chart)

Combining the calculation and visualization setps in the articles from Natalie and Matt with the process guidance from Ben and the Wikipedia article, I decided to build a Power BI viz in honor of how I learned in Tableau. So, I built this using stats of one of my favorite quarterbacks: Kurt Warner.

**Data Source:** [Pro Football Reference](https://www.pro-football-reference.com/players/W/WarnKu00/gamelog/)

## Walkthrough

This will have quite a few steps, as I like to seperate each step in details.

### Optional Data Setup

![Kurt Warner Data Table](../assets/img/powerbi/control-charts/table_view.png)

The following steps are optional depending on how your raw data is structured. In my testing, the simplest method to build these charts is to summarize your data down to one row per timeframe or x-axis instance, for me this is each game.

1. Add an Index Row
    a. If you aren't planning to use a date field for your x-axis, you can add an index in the "Edit Query" window. You can navigate to this by clicking "Transform Data" upon import or right-clicking on your table in the data model view.
    ![Add Index](../assets/img/powerbi/control-charts/add_index.png)

2. Add Calculated Columns
    a. If you want to plug a metric into your control chart that isn't in your raw data, be sure to calculate it as a column.
    b. If it's a ratio, you could do this after step three, but if it is something like charges-discounts=net sales, you can do that here.

3. Create a Summary Table
    a. This is the step I have found to really simplify the process. I don't technically need to do it as my NFL data is already one row per game, but I will do it here for the sake of the example.
    b. In the data view, under the Home ribbon, click "New Table" and adapt this code:

    ```
    summary_table = 
    SUMMARIZECOLUMNS (
        'Kurt Warner - Pro Football Reference'[Index],
        'Kurt Warner - Pro Football Reference'[Date],
        'Kurt Warner - Pro Football Reference'[Opp],
        'Kurt Warner - Pro Football Reference'[H/A],
        --[ADD MORE SUMMARIZE COLUMNS HERE],
        --FILTER('Kurt Warner - Pro Football Reference', 'Kurt Warner - Pro Football Reference'[H/A] = "H"), --can add filter here
        "Yards", SUM('Kurt Warner - Pro Football Reference'[Yds]) --expression to return grouped yards
    )
    ```

If you do want to track a calculated ratio, you could add it to this summarized table to make it a clean metric per day, or game in my example. As you can see in my formula, I am going to keep things simple and just pull over the yards stat to visualize.

**Here is my summarized table:**

![Summarized Data Table](../assets/img/powerbi/control-charts/summary_table.png)

### Add New Columns

These are the columns necessary to drive our control chart calculations.

1. Prior Period Amount
    a. We use this to calculate the moving range
    ```
    Prior Game Yards = 
        CALCULATE(
            SUM(summary_table[Yards]),
            FILTER('summary_table',summary_table[Index]=EARLIER(summary_table[Index])-1) --filter to prior index value to grab value
        )
    ```

2. Moving Range
    ```
    Moving Range = 
        if(summary_table[Index]<=1,0, --ignore the first record
        ABS(summary_table[Yards]-summary_table[Prior Game Yards])
    )

    //calculate the absolute difference between the current game and the prior game yards
    ```

3. Prior Period Moving Range
    ```
    Prior Game Moving Range = 
        CALCULATE(
            SUM(summary_table[Moving Range]),FILTER('summary_table','summary_table'[Index]=EARLIER('summary_table'[Index])-1)
        )
    ```

**Here is my table with these added columns:**

![Table with Added Fields](../assets/img/powerbi/control-charts/added_columns.png)



