---
title: My Project of Youtube (Part 2)
layout: post
post-image: "https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/youtube-data-image.jpeg"
description: A little bit about my YouTube project
tags:
- data science
- YouTube
- API
---

> ### This is a walk-through project video on data viz and streamlit that helps a lot.
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/Sb0A9i6d320?si=mMZQdD7Z2DktUEl0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Introduction:

As I mentioned before, YouTube has been an important part of the day for me and my wife, we use it to learn and share time. We were planning to start our own YouTube channel about cooking and food, so I started this little project to better understand what successful cooking YouTubers have done, so we have a notion about how to do it. 

[Here is the repository of the project](https://github.com/JT98CH/PROJECT_YOUTUBE.git)

## Vizulizations:

As part of the initial exploratory data analysis (EDA), I began by loading the dataset extracted from the YouTube API. However, I noticed that the first column, "Unnamed: 0," was unnecessary, so I removed it. Additionally, I added two new columns to the dataset: "engagement," calculated as the sum of comment_count and like_count divided by view_count, and "collaboration," which is binary (1 or 0) to indicate whether a video involved collaboration.

<pre>
  <code>
# Dropping unnecessary column
df = df.drop('Unnamed: 0', axis=1)

# Adding the 'engagement' and 'collaboration' columns
df['engagement'] = (df['comment_count'] + df['like_count']) / df['view_count']
df['collaboration'] = np.where(df['collaboration'] == True, 1, 0)

  </code>
</pre>

I also noticed that some data from the years 2014 to 2017 had limited representation, so I decided to focus on data from 2018 onwards for a more relevant analysis.
<pre>
  <code>
df_year_cleaned = df[df['year'] >= 2018]
  </code>
</pre>

To understand the distribution of video lengths, I created a histogram.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/hist.png)

The histogram revealed the presence of both regular YouTube videos and Shorts, with Shorts typically lasting around 30 seconds. To classify these videos, I introduced a "short" column based on a 1-minute threshold.

<pre>
  <code>
df_year_cleaned['short'] = np.where(df_year_cleaned['duration_in_minutes'] <= 1, 1, 0)
  </code>
</pre>

I also examined the distribution of title lengths.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/hist2.png)

The graph shows that it follows a normal distribution, I will consider that as something interesting

Now let's dive into the actually fun part

<b>Do you think there's a relationship between likes and views?</b>
Let's see

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/scatter.png)

I'm not sure, but it kind of looks linear, so let's see something better

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/line.png)

It's interesting to see the different slops for each YouTuber but it's indeed linear

<b>What about the relationship between the length of the videos and their view?</b>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/line2.png)

Not quite as good as the likes and views, let come back again to the views but in a boxplot now

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/boxplot.png)

That's cool, it confirms that Nick DiGiovanni is a bigger YouTuber. Also, I like those boxplots, so let's go to check the views by year

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/boxplot2.png)

It looks like the bump of Covid is ending and fewer people are spending time on YouTube, or we need to wait until the year 2023 finishes, or people are getting less excited about Food and Cooking

<b>What about the relationship between their submission day and the view?</b> Let's find out

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/boxplot3.png)

I'm not surprised that videos that were uploaded during the weekend get more views, so this confirms my theory
So, <b>is there any correlation then?</b>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/output.png)

In summary, the strongest correlation between likes and views (0.83) indicates a strong positive relationship. Views and comments have a moderate positive correlation (0.37), while duration in minutes has a weak negative correlation (-0.22) with views. Short has a moderate positive correlation (0.31) with views, and collaboration has a very weak positive correlation (0.099) with views.

## Key Insights:

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight1.png)

Video Lengths: For our cooking channel, we should aim for regular videos to be at least 10 minutes long, and Shorts should be around 30 seconds.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight2.png)

Title Length: Titles for regular videos should ideally be around 50 characters, while Shorts can have shorter titles, approximately 20 characters.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight4.png)

Shorts Impact: Shorts tend to receive a higher engagement rate on average for all three channels, suggesting that creating Shorts can be an effective strategy to engage the audience.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight5.png)

Likes and Views Relationship: There is a strong positive correlation (0.83) between likes and views, indicating that more likes generally lead to more views.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight6.png)

Views by Year: The graph shows a decline in views in 2021 and 2022, possibly suggesting that the surge in YouTube viewership during the COVID-19 pandemic may be subsiding.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight7.png)

Views by Day of the Week: Videos uploaded during the weekend tend to receive more views, highlighting the importance of strategic video publishing.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight8-1.png)

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight8-2.png)

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight8-3.png)

Consistency in Uploads: Successful YouTubers follow a consistent pattern in uploading videos, which can help maintain viewer engagement.

## Conclusion:

This project has provided valuable insights into the world of successful cooking YouTube channels. While it didn't reveal groundbreaking discoveries, it has equipped us with a solid foundation to start our own channel. We've also created a web application to explore the data further. <https://youtube-project-jt.streamlit.app/>
