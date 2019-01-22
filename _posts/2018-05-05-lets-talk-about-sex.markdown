---
title: "Let's Talk About Sex"
subtitle: "A Survey of Hook-up Culture at Duke University"
layout: post
date: 2018-05-05 17:00
headerImage: false
tag:
- R
- STA199
projects: true
category: project
author: maxbartlett
description: Results from a Spring 2018 survey of hook-up culture at Duke University
--- 
## Introduction
*Let's Talk About Sex* was my [final team project](https://www2.stat.duke.edu/courses/Spring18/Sta199/project/project.html
) for [STA199: Introduction to Data Science](https://www2.stat.duke.edu/courses/Spring18/Sta199/). I worked with [Liam Pulsifer](https://github.com/lwpulsifer) and [Jennifer Chin](https://github.com/jenniferchin) to formulate an interesting research question, collect, clean, and analyze relevant data, and present our findings to our professor, teaching assistants, and peers.

Hook-up culture at Duke has long been a hot topic [among](https://www.dukechronicle.com/article/2017/03/sexual-assault-and-hookup-culture) [both](https://www.dukechronicle.com/article/2017/04/in-defense-of-dating-at-duke) [students](https://www.dukechronicle.com/article/2018/01/df3dfiztz28o4rd) and in [the media](https://www.theatlantic.com/magazine/archive/2011/01/the-hazards-of-duke/308328/), especially since the Duke lacrosse scandal hit national headlines in 2006. The narrative that Duke students eschew traditional romantic relationships for hook-ups, instead prioritizing their intense studies and future high-powered careers, continues to dominate campus discourse on dating. Despite all of the attention, most reports on Duke's hook-up culture rely on anecdotal evidence, save for a [2007 survey conducted by Duke researchers](http://dukemagazine.duke.edu/article/sex-love-and-celibacy). Jennifer, Liam, and I believe that without data representative of the entire university, unfounded expectations and unhealthy social pressures will continue to permeate through the student body. A decade after the 2007 study, our team wanted to answer the following questions:

* Do high school students with little/no sexual experience significantly change their behavior upon coming to Duke?
* How do these findings align with common stereotypes surrounding hook-up culture?
* What demographic variables correlate most with number of sexual partners?

## Data Collection

We created a 14-question survey that focuses on a range of demographic factors, such as religion, year in school, and political views, as well as number of sexual partners before and during college. The data dictionary can be found [here](../assets/projects/lets_talk_about_sex/data_dictionary.html). Although the survey was anonymous, one could potentially use different pieces of individual data to determine the identity of a respondent. For this reason, I am keeping the raw dataset private.

After posting our survey in the "All Duke" Facebook group, we received 400+ responses in just a matter of days. After cleaning the data, we were left with 361 observations. As S. Philip Morgan found a decade ago, "people in general are more likely to respond to questionnaires that pertain to something that interests them," and Duke students were eager to share their experiences with hook-up culture.

After cleaning the data, we created an R Shiny application to display relationships between various demographic factors, which can be found below. I encourage you to play around with the data and comment if you find anything interesting.

## R Shiny Application

<iframe src="https://maxbartlett.shinyapps.io/STA199FinalProj/" height="800px" width="1000px"></iframe>

## Presentation

<object data="../assets/projects/lets_talk_about_sex/presentation.pdf" width="600px" height="350px">
    <embed src="../assets/projects/lets_talk_about_sex/presentation.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="../assets/projects/lets_talk_about_sex/presentation.pdf">Download PDF</a>.</p>
    </embed>
</object>

Though there are potentially an unlimited number  of ways our team could have extracted meaning from this  dataset, we had to focus our narrative to fit the six-minute time limit for our final presentation. We first performed exploratory data analysis by grouping the observations in ways that are meaningful to our research questions. We picked four trends of interest from our research to present to the class.

#### Trends between high school and college

We answered the first question regarding differences between sexual behavior before and during college by creating a new discrete variable, *partner_group*, which represents whether someone was/wasn't sexually active in both high school and college, in high school but not in college, or not in high school but in college. These data can be found on page seven of the presentation. To our surprise we found that no individual in the dataset fit into the last category. That is, not a single person who wasn't sexually active in high school became sexually active in college.

Our immediate thought was that members of this group might be more religious than members of the other three groups, but further analysis showed that nearly two thirds of this group wasn't religious. Though we couldn't attribute this to another variable in the dataset, we demonstrated that religiousness wasn't the sole reason for remaining abstinent. Our thoughts are that students may be abstaining voluntarily for some other reason (perhaps cultural differences that can't only be attributed to religion), or it may be involuntary.

#### Difference in means between schools

Next, we grouped the observations by major, and eventually by school - either the Trinity College of Arts & Sciences or the Pratt School of Engineering. A cursory glance at the summary statistics revealed a noticeable difference between the two schools. We performed a two-sample t-test for difference of means with a significance level of 0.05, finding a p-value of 0.132 between the two samples. That is, assuming that there is no difference between Pratt and Trinity students, we would expect to see a difference in mean number of partners as extreme as this in approximately 13.2% of random samples. Therefore, we failed to reject the null hypothesis: that there is no difference in promiscuity between Trinity and Pratt students.

#### Difference in means between Greek and independent students

After examining the differences between schools, we looked at those between independent students and students involved in Greek life. We used our Shiny Application to do some quick-and-dirty analysis of Greek and non-Greek students, finding that while there is little difference between the mean number of partners of Greek and non-Greek freshmen, the gap widens significantly as students move through their four years at Duke. (You can recreate this graph from the presentation by selecting Group X Axis by year, Y Axis by partners_college, and Color by greek).

Again, we performed a two-sample t-test for difference of means with a significance level of 0.05, but instead found a p-value of essentially 0. Therefore, we rejected the null hypothesis, accepting that there is a significant difference in promiscuity between Greek and independent students.

#### Linear Model

For the final part of our analysis, we built a linear model in order to gauge what variables were most strongly associated with a student's number of partners. As we expected, a student's religion factored most heavily in this model. However, the low R^2 value means that the model explains some, but not much of the variability of the data around the mean.

## Conclusions and Disclaimers

Though the results of our survey were well recieved by faculty and students in the class, we could've improved some aspects of our data collection and analysis. We struggled to balance the length and thoroughness of the survey with the number of responses we hoped to gather. Generally, the longer the survey, the fewer the number of people willing to finish the survey. Working with limited time and resources (we created the survey and collected all of the data within a week), we opted to create a short survey that most people could complete in less than a minute.

Our data are subject to voluntary response bias, as people are generally more likely to misreport sensitive data about themselves, though generally less so when this data is given anonymously, as was done in our survey.

There were some issues with certain variables, notably *politics*, for which people could select multiple responses. This made cleaning and analyzing some of the data more difficult.

We also struggled with the ambiguity of certain aspects of the survey. We asked the respondent if they were currently in a romantic relationship lasting longer than one month, but we found this data to be of little use. Had we asked for the length of the person's relationship, whether the relationship was monogomous or open, or even the total amount of time the respondent had been in exclusive relationships throughout their life/at Duke, we could've used this data to provide a clearer look at dating at Duke. The current model data distinguish between people solely on number of partners, but there is likely difference between a student who is single versus a student who has been in a long-term relationship for many years.

Additionally, we left the definition of "partners" to be ambiguous as this can mean different things to different people, especially between heterosexual and LGBT+ individuals. However, this is again a trade-off that results in a shorter survey, but makes it more difficult to compare different segments of our data.

Ultimately, there are thousands of different factors that influence a person's romantic and sexual history. We can neither hope to paint a full picture of Duke's dating culture through one number, their number of partners, nor attribute one's history directly to Duke's culture. But through our analysis, we were able to examine commonly held stereotypes surrounding this culture and scrutinize their veracity.