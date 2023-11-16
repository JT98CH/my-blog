---
title: My Project of Youtube (Part 1)
layout: post
post-image: "https://raw.githubusercontent.com/JTCH98/my-blog/master/assets/images/youtube-data-image.png"
description: A little bit about my YouTube project
tags:
- data science
- YouTube
- API
---
> ### Most of the knowledge of YouTube API I got from this video. [here](https://www.youtube.com/watch?v=SwSbnmqk3zY&t=3442s&ab_channel=techTFQ){:targe="[https://help.tableau.com/current/pro/desktop/en-us/getstarted_buildmanual_ex1basic.htm](https://www.youtube.com/watch?v=SwSbnmqk3zY&t=3442s&ab_channel=techTFQ)"}.
---

### Unlocking YouTube's Secrets: A Data-Driven Approach to Video Success

### Introduction:

YouTube has integrated into our daily lives massively in the past years, serving as an infinite wellspring of knowledge on a vast array of topics. My personal journey with YouTube has been a mix of guilt because the time spent and a proud satisfaction to the knowledge gained. This dichotomy sparked a curiosity within me, leading to a fascinating research question: What makes a YouTube channel successful?

### Motivation:

Since with my wife we shared a passion for culinary arts and food. Together, we've consumed countless hours watching recipes, culinary experiments, and global food adventures on YouTube. This shared experience has stirred within us the aspiration to start our own YouTube channel. Fortunately, I have the control over a project of data analysis of my preference, I embarked on a project that would offer practical insights into the basics of YouTube algorithm and content strategy.

In this blog post, I'll share the journey of my data-driven exploration into YouTube's inner workings. Join me as we dissect these insights and learn how data can inform and elevate our YouTube content creation.

Here is the repository of the project: https://github.com/JT98CH/PROJECT_YOUTUBE.git

### Gathering the data:

I published the code to gather the data here: https://github.com/JT98CH/PROJECT_YOUTUBE/blob/main/PROJECT_YOUTUBE_DATA.ipynb

Getting Started: The Setup
As any adventure, it began with the basics, setting up. So, I set up the Python environment by importing the necessaries libraries like Pandas, Seaborn, Matplotlib. Another important tool is the Google API client. I won’t get into the details about how to get a key because at the beginning of this post I shared a video that’s better than me in explaining that, so check that first. I just securely read the key from a file.

Targeting Channels: Finding Our Data Source
Once you have everything setting up, now it’s time to choose or identify the YouTube channels to analyze. Due to the nature of the project, I only selected a few channel IDs (Joshua Weissman, Guga Foods and Nick DiGiovanni) and initiated our YouTube API client with the API key. This is like our gateway to accessing YouTube's reservoir of data.

The Collection Expedition: Gathering the Actual Data
The data collection was a multi-step voyage:
•	Channel Analysis: With the get_channel_stats function, we delved into each channel's core, extracting vital statistics like content details and viewer engagement.
•	Video List Compilation: Next, our get_video_list function acted like a net, capturing a comprehensive list of videos from each channel, ensuring even those hidden in the depths of YouTube were included.
•	Aggregating Video Data: The get_all_video_data_for_channels function was our aggregator, piecing together data from different channels into a cohesive whole.
•	Video Detail Mining: Finally, the get_video_details function was our data miner, extracting rich details from each video such as titles, publication dates, and various engagement metrics.

With our data collected, it was time to give it structure. We poured our data into a Pandas DataFrame, a versatile tool for data manipulation. 

Data, in its raw form, can be unwieldy. So, I refined it through:
•	Identifying Collaborations: By analyzing video titles, we marked out collaborations, adding a layer of understanding to our analysis.
•	Time Transformation: Video lengths, initially cryptic in ISO format, were converted into understandable minutes.
•	Date Decoding: We transformed publication dates into Python's datetime objects, adding a column to represent the day of the week, unraveling patterns in publication schedules.

Final step, I exported this cleaned, transformed data into a CSV file.

### Ethical Approach:

Standing on the precipice of launching our own YouTube channel, the ethical use of data becomes paramount. In today's data-rich world, it's all too easy to overlook the people behind the pixels. Throughout this project, respect for data privacy, viewer consent, and transparent practices have been more than mere buzzwords—they are the cornerstones of our methodology.

Even with Google providing the tools to scrape data from YouTube, recognizing the boundaries is critical. I’ve included an article that delves into this topic to provide a broader understanding: Understanding the Limitations of YouTube Data Scraping. Our analytical approach is designed to not only refine our content strategy but also to honor and comprehend the audience that breathes life into these statistics. Personal viewer information has remained untouched; our focus has been strictly on the data that is publicly available, aligning with YouTube's terms of service and ethical data practices.

### Conclusion:

Embarking on this exploration into YouTube's algorithm has already been revelatory. Although I am only at the beginning of analyzing the data, it has started to illuminate a path through the intricate landscape of content creation. This project isn't merely about decoding numbers; it's about understanding the stories they tell.

In my forthcoming post, I will dive deeper into the findings and share the insightful revelations from this analysis. Stay tuned as we continue to unravel the threads of YouTube success and lay the groundwork for our channel's content strategy.


![image](https://github.com/JT98CH/my-blog/assets/62812785/f1d2319c-71d6-429b-be50-d10cd6c98b3a)
