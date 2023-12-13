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

<pre>
  <code>
plt.hist(df_year_cleaned['duration_in_minutes'], bins=25, edgecolor='black')
plt.title('Histogram Length')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.show()
  </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/hist.png)

The histogram revealed the presence of both regular YouTube videos and Shorts, with Shorts typically lasting around 30 seconds. To classify these videos, I introduced a "short" column based on a 1-minute threshold.

<pre>
  <code>
df_year_cleaned['short'] = np.where(df_year_cleaned['duration_in_minutes'] <= 1, 1, 0)
  </code>
</pre>

I also examined the distribution of title lengths.

<pre>
  <code>
title_lengths = df_videos_clean['title'].str.len()
plt.hist(title_lengths, bins=25, alpha=0.5, color='blue')
plt.xlabel('Title Length')
plt.ylabel('Frequency')
plt.title('Histogram of Title Lengths')
plt.show()
  </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/hist2.png)

The graph shows that it follows a normal distribution, I will consider that as something interesting

Now let's dive into the actually fun part

Do you think there's a relationship between likes and views?
Let's see

<pre>
  <code>
#more likes = more views?

#Extract the data from the DataFrame
x_data = df_videos_clean['like_count']
y_data = df_videos_clean['view_count']
hue_data = df_videos_clean['channel_name']  # Channel name for hue

#Create a scatter plot with hue
plt.figure(figsize=(10, 6))
sns.scatterplot(x=x_data, y=y_data, hue=hue_data, palette='tab10', alpha=0.7)

#Customize the plot
plt.title('Scatter Plot with Hue by Channel Name')
plt.xlabel('Likes')
plt.ylabel('Views')

#Show the legend
plt.legend(title='Channel Name', bbox_to_anchor=(1.05, 1), loc='upper left')

#Show the plot
plt.show()
 </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/scatter.png)

I'm not sure, but it kind of looks linear, so let's see something better

<pre>
  <code>
lm = sns.lmplot(x='like_count', y='view_count', hue='channel_name', data=df_videos_clean,
                lowess=True, legend_out=False)
legend = lm._legend

plt.show()
 </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/line.png)

It's interesting to see the different slops for each YouTuber but it's indeed linear

What about the relationship between the length of the videos and their view?

<pre>
  <code>
lm = sns.lmplot(x='duration_in_minutes', y='view_count', hue='channel_name', data=df_videos_clean,
                lowess=True, legend_out=False)
legend = lm._legend

plt.show()
 </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/line2.png)

Not quite as good as the likes and views, let come back again to the views but in a boxplot now

<pre>
  <code>
#boxplot View per Youtube Channel
plt.figure(figsize=(10, 6))
sns.boxplot(x='channel_name', y='view_count', data=df_videos_clean)

#Setting details
plt.title('View per YouTube channel')
plt.xlabel('Channnel names')
plt.ylabel('Views')

plt.show()
 </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/boxplot.png)

That's cool, it confirms that Nick DiGiovanni is a bigger YouTuber. Also, I like those boxplots, so let's go to check the views by year

<pre>
  <code>
#View per Year
plt.figure(figsize=(10, 6))
sns.boxplot(x='year', y='view_count', data=df_videos_clean)

#Setting details
plt.title('View per Year')
plt.xlabel('Channnel names')
plt.ylabel('Views')

plt.show()
 </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/boxplot2.png)

It looks like the bump of Covid is ending and fewer people are spending time on YouTube, or we need to wait until the year 2023 finishes, or people are getting less excited about Food and Cooking

What about the relationship between their submission day and the view? Let's find out

<pre>
  <code>
#Boxplot View per Day published
#Define the desired order for the days
day_order = ['monday', 'tuesday', 'wednesday', 'thursday', 'friday', 'saturday', 'sunday']

#Create the boxplot with the specified order
plt.figure(figsize=(10, 6))
sns.boxplot(x='day_published', y='view_count', data=df_videos_clean, order=day_order)

#Setting details
plt.title('View per Day published')
plt.xlabel('Day of the Week')
plt.ylabel('Views')

plt.show()
  </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/boxplot3.png)

I'm not surprised that videos that were uploaded during the weekend get more views, so this confirms my theory

<pre>
  <code>
correlation_matrix = df_year_cleaned[['view_count', 'comment_count', 'like_count', 'duration_in_minutes','short', 'collaboration']].corr()

plot = sns.heatmap(correlation_matrix, annot=True, cmap="YlGnBu")

plot.set_xticklabels(plot.get_xticklabels(), rotation=25, horizontalalignment='center', fontsize=8)
  </code>
</pre>

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/output.png)

In summary, the strongest correlation between likes and views (0.83) indicates a strong positive relationship. Views and comments have a moderate positive correlation (0.37), while duration in minutes has a weak negative correlation (-0.22) with views. Short has a moderate positive correlation (0.31) with views, and collaboration has a very weak positive correlation (0.099) with views.

## Key Insights:

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight1.png)

Video Lengths: For our cooking channel, we should aim for regular videos to be at least 10 minutes long, and Shorts should be around 30 seconds.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight2.png)

Title Length: Titles for regular videos should ideally be around 50 characters, while Shorts can have shorter titles, approximately 20 characters.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight3.png)

Shorts Impact: Shorts tend to receive a higher engagement rate on average for all three channels, suggesting that creating Shorts can be an effective strategy to engage the audience.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight4.png)

Likes and Views Relationship: There is a strong positive correlation (0.83) between likes and views, indicating that more likes generally lead to more views.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight5.png)

Views by Year: The graph shows a decline in views in 2021 and 2022, possibly suggesting that the surge in YouTube viewership during the COVID-19 pandemic may be subsiding.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight6.png)

Views by Day of the Week: Videos uploaded during the weekend tend to receive more views, highlighting the importance of strategic video publishing.

![Test Image](https://raw.githubusercontent.com/JT98CH/my-blog/main/assets/images/insight7.png)

Consistency in Uploads: Successful YouTubers follow a consistent pattern in uploading videos, which can help maintain viewer engagement.

## Conclusion:

This project has provided valuable insights into the world of successful cooking YouTube channels. While it didn't reveal groundbreaking discoveries, it has equipped us with a solid foundation to start our own channel. We've also created a web application to explore the data further. <https://youtube-project-jt.streamlit.app/>
