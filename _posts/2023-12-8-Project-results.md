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

From this simple action, I say see that I don't need that extra first column, so it will be better to remove it. Also, I did some research and I found this website, and I think I'll use the engagement metric in my analysis, so I will add it too.


Another important tool is the Google API client. I won’t get into the details about how to get a key because, at the beginning of this post, I shared a video that’s better than me in explaining that, so check that first. I just securely read the key from a file. The most

<pre>
  <code>
df = df.drop('Unnamed: 0', axis=1)
df['engagement'] = (df['comment_count'] + df['like_count']) / df['view_count']
df

channel_name	title	published	description	length	tag	view_count	like_count	comment_count	collaboration	duration_in_minutes	day_published	year	month	day	engagement
0	Joshua Weissman	Perfect Steak Au Poivre	2023-11-15 17:16:05	Check out Google’s Holiday 100 list for trendi...	PT30S	No tags	165706	10342	93	False	0.500000	wednesday	2023	11	15	0.062973
1	Joshua Weissman	Every Way to Cook Steak (34 Ways)	2023-11-12 15:30:10	The steak recipe to end all recipes. Special t...	PT29M18S	Have tags	1062094	40946	2220	False	29.300000	sunday	2023	11	12	0.040642
2	Joshua Weissman	I Made The Easiest Ramen Ever	2023-11-08 16:00:23	Easiest Ramen (that's not instant ramen) 3 dif...	PT10M56S	Have tags	1341324	56279	1283	False	10.933333	wednesday	2023	11	8	0.042914
3	Joshua Weissman	MCRIB At Home With Terry Crews	2023-11-04 19:30:00	NaN	PT31S	No tags	875029	73229	727	False	0.516667	saturday	2023	11	4	0.084518
4	Joshua Weissman	I Tried Every Fast Food Fried Chicken Sandwich...	2023-11-01 14:30:16	Get MY NEW Cookbook: https://bit.ly/TextureOve...	PT23M1S	Have tags	2678009	85991	6291	False	23.016667	wednesday	2023	11	1	0.034459
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
1789	Nick DiGiovanni	How To Cook Arctic Char	2020-10-13 01:56:10	The perfect combination of salmon and trout, s...	PT4M55S	Have tags	430600	13068	379	False	4.916667	tuesday	2020	10	13	0.031229
1790	Nick DiGiovanni	How To Cook Salmon	2020-09-29 00:12:38	This is salmon made easy. Follow my 80-20 rule...	PT2M31S	Have tags	444451	14634	282	False	2.516667	tuesday	2020	9	29	0.033561
1791	Nick DiGiovanni	Beef Wellington	2020-09-14 18:41:27	Beef Wellington doesn't have to be scary. \n\n...	PT8M5S	Have tags	830370	27474	1116	False	8.083333	monday	2020	9	14	0.034430
1792	Nick DiGiovanni	Japanese A5 Wagyu Beef	2020-08-31 21:42:22	You can almost eat this steak with a spoon. \n...	PT10M29S	Have tags	795780	20420	592	False	10.483333	monday	2020	8	31	0.026404
1793	Nick DiGiovanni	MasterChef Finale Dessert	2019-10-21 13:02:08	Thank you to the entire MasterChef team for su...	PT58S	Have tags	3549046	88542	2935	False	0.966667	monday	2019	10	21	0.025775
  </code>
</pre>


Final step, I exported this cleaned and transformed data into a CSV file.

## Key Insights:


## Conclusion:

