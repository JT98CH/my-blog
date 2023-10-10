---
title: How to use Tableau in data science, parameters
layout: post
post-image: ""
description: My first view of data science as a freshman
tags:
- data science
- Tableau
- interactive viz
---
> ### This topic assumes that you know the very basics of tableau, [here](https://jekyllrb.com/){:targe="https://help.tableau.com/current/pro/desktop/en-us/getstarted_buildmanual_ex1basic.htm"} is a good tutorial for new users.
---

# Parameters
In Tableau, a parameter is a dynamic value that can replace a constant in a calculation, filter, or reference line. For example, if you have a formula that returns a value of Sales > 5000, you can replace 5000 with a parameter. This allows you to input different values or choose from a predefined list or range, making the visualization or result set dynamic based on that parameter.
Today we are going to learn how to take advantage of this dynamic power of parameters.

### Connect to your data
The first step is to connect to the data you want to explore. This example shows how to connect a CSV file data in Tableau Desktop.<br>
* Open Tableau. On the start page, under Connect, click More. In the Open dialog box, navigate to the CSV file in your local computer. For this example, I'm using the Employee data from [Kaggle](https://jekyllrb.com/){:targe="https://www.kaggle.com/datasets/prakharjadaun/employee-data?rvi=1"}
* After you connect to the CSV data, the Data Source page shows the sheets or tables in your data. Drag the "employees" table to the canvas to start exploring that data.
* Click the sheet tab to go to the new worksheet and begin your analysis.

<INSERT IMAGE HERE>

### Build the parameter
In the Data pane, click on Create Parameter.<be>
<INSERT IMAGE HERE>

* Choose a name for the parameter, in this case "p.Breakdown".
* Choose Integer as the Data Type.
* Choose List as Allowable Values.
* Add the categories of interest to the list.
* Click Ok.

<INSERT IMAGE HERE>

### Build the breakdown
In the Data pane, click on Create calculated field.<be>
<INSERT IMAGE HERE>

* Add a formula, in this case, the purpose is to assign the group, categories, or breakdown of interest.
* Click Ok.

<INSERT IMAGE HERE>



