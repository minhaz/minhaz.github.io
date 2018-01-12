# Data Studio - #makeovermonday - 2018 02

## Dataset

For this week's [#makeovermonday][mom twitter hashtag], we have the [Across the
globe, personality is rated as more important than looks][original blog post]
dataset from [YouGov][dataset link]. The dataset contains survey data from male
and female respondents of twenty nationalities. The respondents ranked six
different attributes that they most prioritize in a partner. We used [Data
Studio][datastudio] to explore the dataset.

## Importing into Data Studio

The dataset is hosted on [data.world][data.world dataset].

-   Step 1: We opened the [data.world Community Connector][data.world cc] to
    directly connect with data.world's API from Data Studio.
-   Step 2: We configured the connector using these inputs:
    -   Dataset or Project URL \
        `https://data.world/makeovermonday/2018-w-2-looks-vs-personality/`
    -   SQL Query \
        `Select * from looks_vs_personality`
-   Step 3 (optional): In the fields screen, we changed type of the `percentage`
    field from *Numeric* to *Percentage*

## Exploratory analysis

Using Data Studio, we completed [exploratory analysis][exploratory] of the
dataset . All visualizations you see below were embedded via
Data Studio and are fully interactive. The visualizations can be recreated by
anyone using the data.world Community Connector and Data Studio.

### How Men and Women of a nationality prioritize attributes of their partners

First, we created an interactive national snapshot dashboard where you can
select a nationality and to see how men and women from that nationality ranked
different attributes. Try selecting different nationalities here to explore the
data:

<iframe width="600" height="700" src="https://datastudio.google.com/embed/reporting/1o8pRpnkvFihWQ8uMTqajCcf3-2e8Dvoz/page/xn7L" frameborder="0" style="border:0" allowfullscreen></iframe>

For this visualization, we used *bar charts* in Data Studio. The bars were
stacked and the colors were set manually. We highlighted data for the 1st ranked
as well as the last ranked attribute. The default nationality was set to
*American* using the data filter.

An example insight that can be highlighted using a snapshot of this
visualization: For *American* respondents, *Personality* is the most favored
attribute for both men and women. Also, more American men respondents had
preferences for *Good Looks* in their partners compared to American women
respondents.

### How Men and Women across nationalities ranked specific partner attribute

Here we are looking at specific attributes across all nationalities for both men
and women. For each attribute, we are displaying the percentage of of
respondents that ranked the attribute either as the top(#1) or the lowest(#6).

<iframe width="600" height="700" src="https://datastudio.google.com/embed/reporting/1kkhIqtXfsQNMd4Cp0qec5xe_RGB0QGh3/page/xn7L" frameborder="0" style="border:0" allowfullscreen></iframe>

An example insight: Compared to the rest of the nations surveyed, *Personality*
ranks as #1 more often in Scandinavian nations including Denmark, Finland,
Norway, and Sweden.

### What Men and Women are (or are not) interested in

For these visualizations, we wanted to make XY plot charts with different
attributes on x and y axis. But the default data schema would did not allow us
to do that since the chart requires the data series to be in different columns.

We added the Community Connector again as a new data source to our dashboard.
However, this time, the query was different:

```sql
select
    gender,
    nationality,
    rank_number,
    sum(if(question='They are good looking', percentage, 0)) as good_looks,
    sum(if(question='They have a personality I like', percentage, 0)) as personality,
    sum(if(question='They have a sense of humour I like', percentage, 0)) as sense_of_humor,
    sum(if(question='They are intelligent', percentage, 0)) as intelligence,
    sum(if(question='They have/make a decent amount of money', percentage, 0)) as affluence,
    sum(if(question='They have similar interests to me', percentage, 0)) as similar_interests
FROM
    looks_vs_personality
WHERE
    rank_number in (1,6)
Group BY
    gender,
    nationality,
    rank_number
```

This query lets us pivot the data and filter it only for the highest and lowest
ranking attributes. We also renamed the fields to have shorter lengths. Ideally
we would like to have a 6 by 6 grid that compares each attribute with the
others. For the sake of simplicity, we only picked 4 comparisons here.

This visualization shows the percentage of respondents that ranked specific
attributes as the top one (#1). That means the higher the response percentage,
the more desirable was the attribute:

<iframe width="600" height="700" src="https://datastudio.google.com/embed/reporting/1sCrzcYi8PjD4gXE35mTrxUKOG8bGzM-G/page/xn7L" frameborder="0" style="border:0" allowfullscreen></iframe>

This shows us that more men are interested in *Good Looks* compared to women
across nations (top left). A significant portion of *Vietnamese* men were
interested in *Good Looks* for their partner. Though *Intelligence* seems to be
favored equally by both gender, more *Indian* women were interested in this
attribute compared to the rest of the nations. We also see that more women in
general favor *Sense of Humor* compared to men with *British* women being ahead
the most (bottom right). Interestingly *Indian* men and women preferred *Sense
of Humor* equally and at a high rate.

The next visualization shows data points for respondents ranking the attribute
at the lowest (#6). We can assume here that the higher the response percentage,
the less desirable was the attribute.

<iframe width="600" height="700" src="https://datastudio.google.com/embed/reporting/1t28Jf6DlcO8btCF5NCK8SrlkeeMmYKgG/page/xn7L" frameborder="0" style="border:0" allowfullscreen></iframe>

Most women do not seem to think *Good Looks* is an important attribute (top
left). The gap between men and women for this attribute is more visible here
compared to the *desirable* plot above. More men in general are not interested
in *Affluence* for their partners compared to women (bottom left).

## Conclusion

It would be interesting to take some of the findings and create explanatory
visualizations from them focusing on specific insights. Using the [data.world
Community Connector][data.world cc] in [Data Studio][datastudio] was
seamless and helped to get started with data exploration pretty quickly. The SQL
support in the Community Connector was also helpful to transform the data -
shout-out to the [data.world team][dataworld team] for that!

[mom twitter hashtag]: https://twitter.com/hashtag/makeovermonday?lang=en
[original blog post]: https://yougov.co.uk/news/2017/11/29/personality-more-important-looks-across-globe/
[dataset link]: https://d25d2506sfb94s.cloudfront.net/cumulus_uploads/document/ucgs0hwj7h/YouGov%20global%20partner%20preferences.pdf
[datastudio]: https://datastudio.google.com
[data.world dataset]: https://data.world/makeovermonday/2018-w-2-looks-vs-personality
[data.world cc]: https://datastudio.google.com/datasources/create?connectorId=AKfycbwGs5GlUrTE6y9x1cD80uTg005lDFj3BAYy0IFpmIGit2QnlcDrnreeLg
[exploratory]: http://www.storytellingwithdata.com/blog/2014/04/exploratory-vs-explanatory-analysis
[dataworld team]: https://data.world/about/
