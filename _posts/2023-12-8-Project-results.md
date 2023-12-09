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

> ### This is a walk-through project video on YouTube API creation that helps a lot.
<iframe width="800" height="400" src="https://youtu.be/SwSbnmqk3zY?si=j6fvApzQM9mTqcfX" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Introduction:

As I mentioned before, YouTube has been an important part of the day for me and my wife, we use it to learn and share time. We were planning to start our own YouTube channel about cooking and food, so I started this little project to better understand what successful cooking YouTubers have done, so we have a notion about how to do it. 

[Here is the repository of the project](https://github.com/JT98CH/PROJECT_YOUTUBE.git)

## Vizulizations:

As part of any EDA, I wanted to just see the data frame that we got from the API extraction.

<pre>
  <code>
df = pd.read_csv("youtube_data.csv", index_col=False)
df
Unnamed: 0	channel_name	title	published	description	length	tag	view_count	like_count	comment_count	collaboration	duration_in_minutes	day_published	year	month	day
0	0	Joshua Weissman	Perfect Steak Au Poivre	2023-11-15 17:16:05	Check out Google’s Holiday 100 list for trendi...	PT30S	No tags	165706	10342	93	False	0.500000	wednesday	2023	11	15
1	1	Joshua Weissman	Every Way to Cook Steak (34 Ways)	2023-11-12 15:30:10	The steak recipe to end all recipes. Special t...	PT29M18S	Have tags	1062094	40946	2220	False	29.300000	sunday	2023	11	12
2	2	Joshua Weissman	I Made The Easiest Ramen Ever	2023-11-08 16:00:23	Easiest Ramen (that's not instant ramen) 3 dif...	PT10M56S	Have tags	1341324	56279	1283	False	10.933333	wednesday	2023	11	8
3	3	Joshua Weissman	MCRIB At Home With Terry Crews	2023-11-04 19:30:00	NaN	PT31S	No tags	875029	73229	727	False	0.516667	saturday	2023	11	4
4	4	Joshua Weissman	I Tried Every Fast Food Fried Chicken Sandwich...	2023-11-01 14:30:16	Get MY NEW Cookbook: https://bit.ly/TextureOve...	PT23M1S	Have tags	2678009	85991	6291	False	23.016667	wednesday	2023	11	1
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
1789	1789	Nick DiGiovanni	How To Cook Arctic Char	2020-10-13 01:56:10	The perfect combination of salmon and trout, s...	PT4M55S	Have tags	430600	13068	379	False	4.916667	tuesday	2020	10	13
1790	1790	Nick DiGiovanni	How To Cook Salmon	2020-09-29 00:12:38	This is salmon made easy. Follow my 80-20 rule...	PT2M31S	Have tags	444451	14634	282	False	2.516667	tuesday	2020	9	29
1791	1791	Nick DiGiovanni	Beef Wellington	2020-09-14 18:41:27	Beef Wellington doesn't have to be scary. \n\n...	PT8M5S	Have tags	830370	27474	1116	False	8.083333	monday	2020	9	14
1792	1792	Nick DiGiovanni	Japanese A5 Wagyu Beef	2020-08-31 21:42:22	You can almost eat this steak with a spoon. \n...	PT10M29S	Have tags	795780	20420	592	False	10.483333	monday	2020	8	31
1793	1793	Nick DiGiovanni	MasterChef Finale Dessert	2019-10-21 13:02:08	Thank you to the entire MasterChef team for su...	PT58S	Have tags	3549046	88542	2935	False	0.966667	monday	2019	10	21
  </code>
</pre>

From this simple action, I can see that I don't need that extra first column, so it will be better to remove it. Also, I did some research and I found this website, and I think I'll use the engagement metric in my analysis, so I will add it too. Along with that, I will take advantage and add the collaboration flag to see later if that has an impact on the views.

<pre>
  <code>
df = df.drop('Unnamed: 0', axis=1)
df['engagement'] = (df['comment_count'] + df['like_count']) / df['view_count']
df['collaboration'] = np.where(df['collaboration'] == True, 1, 0)
df

channel_name	title	published	description	length	tag	view_count	like_count	comment_count	collaboration	duration_in_minutes	day_published	year	month	day	engagement
0	Joshua Weissman	Perfect Steak Au Poivre	2023-11-15 17:16:05	Check out Google’s Holiday 100 list for trendi...	PT30S	No tags	165706	10342	93	0	0.500000	wednesday	2023	11	15	0.062973
1	Joshua Weissman	Every Way to Cook Steak (34 Ways)	2023-11-12 15:30:10	The steak recipe to end all recipes. Special t...	PT29M18S	Have tags	1062094	40946	2220	0	29.300000	sunday	2023	11	12	0.040642
2	Joshua Weissman	I Made The Easiest Ramen Ever	2023-11-08 16:00:23	Easiest Ramen (that's not instant ramen) 3 dif...	PT10M56S	Have tags	1341324	56279	1283	0	10.933333	wednesday	2023	11	8	0.042914
3	Joshua Weissman	MCRIB At Home With Terry Crews	2023-11-04 19:30:00	NaN	PT31S	No tags	875029	73229	727	0	0.516667	saturday	2023	11	4	0.084518
4	Joshua Weissman	I Tried Every Fast Food Fried Chicken Sandwich...	2023-11-01 14:30:16	Get MY NEW Cookbook: https://bit.ly/TextureOve...	PT23M1S	Have tags	2678009	85991	6291	0	23.016667	wednesday	2023	11	1	0.034459
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
1789	Nick DiGiovanni	How To Cook Arctic Char	2020-10-13 01:56:10	The perfect combination of salmon and trout, s...	PT4M55S	Have tags	430600	13068	379	0	4.916667	tuesday	2020	10	13	0.031229
1790	Nick DiGiovanni	How To Cook Salmon	2020-09-29 00:12:38	This is salmon made easy. Follow my 80-20 rule...	PT2M31S	Have tags	444451	14634	282	0	2.516667	tuesday	2020	9	29	0.033561
1791	Nick DiGiovanni	Beef Wellington	2020-09-14 18:41:27	Beef Wellington doesn't have to be scary. \n\n...	PT8M5S	Have tags	830370	27474	1116	0	8.083333	monday	2020	9	14	0.034430
1792	Nick DiGiovanni	Japanese A5 Wagyu Beef	2020-08-31 21:42:22	You can almost eat this steak with a spoon. \n...	PT10M29S	Have tags	795780	20420	592	0	10.483333	monday	2020	8	31	0.026404
1793	Nick DiGiovanni	MasterChef Finale Dessert	2019-10-21 13:02:08	Thank you to the entire MasterChef team for su...	PT58S	Have tags	3549046	88542	2935	0	0.966667	monday	2019	10	21	0.025775
  </code>
</pre>

Another important thing that I wanted to do before starting the to viz this data frame, I wanted to see if I will have missing or irrelevant data.

<pre>
  <code>
df['year'].value_counts()

2021    498
2022    494
2023    330
2020    187
2019    141
2018    109
2017     15
2016     13
2015      6
2014      1
Name: year, dtype: int64
  </code>
</pre>

In my opinion, it will be better if I just remove those first 4 years.

<pre>
  <code>
df_year_cleaned = df[df['year'] >= 2018]
  </code>
</pre>

Now, I want to know if I can find a common pattern in the length of the videos submitted by these three YouTubers that I selected.

<pre>
  <code>
#Lenght of Videos
plt.hist(df_year_cleaned['duration_in_minutes'], bins=25, edgecolor='black')
plt.title('Histogram Length')
plt.xlabel('Value')
plt.ylabel('Frequency')
plt.show()
  </code>
</pre>

It looks like I have YouTube videos and also Shorts, so I will classify them. 
According to YouTube the max length of a "short" is 1 minute, so I will use that to the logic.

<pre>
  <code>
df_year_cleaned['short'] = np.where(df_year_cleaned['duration_in_minutes'] <= 1, 1, 0)

	channel_name	title	published	description	length	tag	view_count	like_count	comment_count	collaboration	duration_in_minutes	day_published	year	month	day	engagement	short
1	Joshua Weissman	Every Way to Cook Steak (34 Ways)	2023-11-12 15:30:10	The steak recipe to end all recipes. Special t...	PT29M18S	Have tags	1062094	40946	2220	0	29.300000	sunday	2023	11	12	0.040642	0
2	Joshua Weissman	I Made The Easiest Ramen Ever	2023-11-08 16:00:23	Easiest Ramen (that's not instant ramen) 3 dif...	PT10M56S	Have tags	1341324	56279	1283	0	10.933333	wednesday	2023	11	8	0.042914	0
4	Joshua Weissman	I Tried Every Fast Food Fried Chicken Sandwich...	2023-11-01 14:30:16	Get MY NEW Cookbook: https://bit.ly/TextureOve...	PT23M1S	Have tags	2678009	85991	6291	0	23.016667	wednesday	2023	11	1	0.034459	0
6	Joshua Weissman	I Tried Food From Every State In America	2023-10-17 14:30:12	Get the NEW COOKBOOK: https://bit.ly/TextureOv...	PT33M53S	Have tags	4623827	139547	26353	0	33.883333	tuesday	2023	10	17	0.035879	0
8	Joshua Weissman	Meals So Easy A College Student Could Make It	2023-10-13 14:30:09	Cheap and easy meals that everyone can make, w...	PT21M56S	Have tags	2518224	103266	2218	0	21.933333	friday	2023	10	13	0.041888	0
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
1788	Nick DiGiovanni	I Cooked A Deep-Sea Fish	2020-11-02 23:40:52	And it definitely tastes better than it looks!...	PT3M24S	Have tags	2706243	79461	2581	0	3.400000	monday	2020	11	2	0.030316	0
1789	Nick DiGiovanni	How To Cook Arctic Char	2020-10-13 01:56:10	The perfect combination of salmon and trout, s...	PT4M55S	Have tags	430600	13068	379	0	4.916667	tuesday	2020	10	13	0.031229	0
1790	Nick DiGiovanni	How To Cook Salmon	2020-09-29 00:12:38	This is salmon made easy. Follow my 80-20 rule...	PT2M31S	Have tags	444451	14634	282	0	2.516667	tuesday	2020	9	29	0.033561	0
1791	Nick DiGiovanni	Beef Wellington	2020-09-14 18:41:27	Beef Wellington doesn't have to be scary. \n\n...	PT8M5S	Have tags	830370	27474	1116	0	8.083333	monday	2020	9	14	0.034430	0
1792	Nick DiGiovanni	Japanese A5 Wagyu Beef	2020-08-31 21:42:22	You can almost eat this steak with a spoon. \n...	PT10M29S	Have tags	795780	20420	592	0	10.483333	monday	2020	8	31	0.026404	0
  </code>
</pre>

Now, what about the title length? I wonder if a relationship or something is interesting about it.

</pre>
  </code>
#Checking Title lengths
#Calculate the lengths of the strings in the 'title' column
title_lengths = df_videos_clean['title'].str.len()

#Create a histogram plot
plt.hist(title_lengths, bins=25, alpha=0.5, color='blue')
plt.xlabel('Title Length')
plt.ylabel('Frequency')
plt.title('Histogram of Title Lengths')
plt.show()
 </code>
</pre>

The graph shows that it follows a normal distribution, I will consider that as something interesting

Now let's dive into the actually fun part

Do you think there's a relationship between likes and views?
Let's see

</pre>
  </code>
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

I'm not sure, but it kind of looks linear, so let's see something better

</pre>
  </code>
lm = sns.lmplot(x='like_count', y='view_count', hue='channel_name', data=df_videos_clean,
                lowess=True, legend_out=False)
legend = lm._legend

plt.show()
 </code>
</pre>

It's interesting to see the different slops for each YouTuber but it's indeed linear

What about the relationship between the length of the videos and their view?

</pre>
  </code>
lm = sns.lmplot(x='duration_in_minutes', y='view_count', hue='channel_name', data=df_videos_clean,
                lowess=True, legend_out=False)
legend = lm._legend

plt.show()
 </code>
</pre

Not quite as good as the likes and views, let come back again to the views but in a boxplot now

</pre>
  </code>
#boxplot View per Youtube Channel
plt.figure(figsize=(10, 6))
sns.boxplot(x='channel_name', y='view_count', data=df_videos_clean)

#Setting details
plt.title('View per YouTube channel')
plt.xlabel('Channnel names')
plt.ylabel('Views')

plt.show()
 </code>
</pre

That's cool, it confirms that Nick DiGiovanni is a bigger YouTuber. Also, I like those boxplot so let's go to check the views by year

</pre>
  </code>
#View per Year
plt.figure(figsize=(10, 6))
sns.boxplot(x='year', y='view_count', data=df_videos_clean)

#Setting details
plt.title('View per Year')
plt.xlabel('Channnel names')
plt.ylabel('Views')

plt.show()
 </code>
</pre

It looks like the bump of Covid is ending and fewer people are spending time on YouTube, or we need to wait until the year 2023 finishes, or people are getting less excited about Food and Cooking

What about the relationship between their submission day and the view? Let's find out

</pre>
  </code>

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
</pre

I'm not surprised that videos that were uploaded during the weekend get more views, so this confirms my theory

</pre>
  </code>

correlation_matrix = df_year_cleaned[['view_count', 'comment_count', 'like_count', 'duration_in_minutes','short', 'collaboration']].corr()

plot = sns.heatmap(correlation_matrix, annot=True, cmap="YlGnBu")

plot.set_xticklabels(plot.get_xticklabels(), rotation=25, horizontalalignment='center', fontsize=8)

 </code>
</pre

## Key Insights:


## Conclusion:

