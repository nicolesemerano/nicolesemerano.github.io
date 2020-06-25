---
layout: post
title: "Are You An All-Star?: My Classification Project"
---

#### By: Nicole Semerano

I was born into a baseball family.  I was raised a Yankees fan, but I have cousins who are Mets and Phillies fans.  So it was only natural that as I explored the Data Science world I complete a project based on baseball.  In this day and age there are so many statistics to choose from:  Player WAR rates, injury probability, where to start?  I decided to go a more classical route.  I will walk you through how I built a classification metric to determine whether a Major League Baseball player would classify as an All-Star.  I know as a fan the process can have its favorites.  Starting players are chosen by fans as a way to get people involved.  However, in some years the starting line-up is made of predominately New York, Boston, and California players.  Then the prior year's World Series manager chooses the auxilary players over a couple of rounds as players with injuries are replaced.  My goal in this classification system was to eliminate or less the bias in this process.   

# Batting Clean-up

<p align="center">
  <img width="320" height="196" src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5d/Home_Plate_Maintenance_-_Lakewood_%283927470101%29.jpg/320px-Home_Plate_Maintenance_-_Lakewood_%283927470101%29.jpg">
</p>

Like any good project, I started out with cleaning my data.  I downloaded the Lahman's Baseball Database.  I started with the batting statistics to focus on the largest set of players.  Then I eliminated all rows for players prior to 1933 when the All-Star Game started.  I also made sure all my players played at least 35 games and had at least that many at bats (a way to eliminate pitchers).  In the All-Star table of the database I eliminated 1945 as there was no game and made sure players were only listed once for years 1959-1961 as I learned those years there were two games each year.  I also added a columns of 1s as All-Stars so when I merged this table with the batting statistics I had my target classification values of 0s and 1s.  In both tables and after the merge I also changed any NaN value to zero.  I ended up with 32943 non All-Stars and 4056 All-Stars.  This matched any historical research I had done.

# Spring Training

I started off by using 17 features as my X value.  These features ranged from ones you would assume, like hits, RBIs, and runs, to ones you wouldn't, like how many times a player was hit by a pitch.  Fast forward in my experimenting, I tried a model with just the features with the highest importance and it did drastically worse.  In my short time in the data science world I have learned the big rule that the more data or features you have it more likely will make things more accurate.  And this project proves that stance correct.  As said before, the target was whether a player was an All-Star(1) or not(0).  I then made a train/test split with a test size of 30%.  This is maybe a little large compared to what recent theory suggests, but I was following the sample projects shown to me.  

I also dealt with the obvious class imbalance.  There are many methods to accomidating this, but I chose to do it before the actual training of the model.  I wanted to start fresh.  I multiplied the minority class by 5 to oversample it.  I also did adjust the threshold after training which is another way to find the right balance and make the best model.


<div>
    <a href="https://plotly.com/~nicole.semerano/1/?share_key=52HVgknBtJDJ80qU5ftyXE" target="_blank" title="All_Star_Feature_Importance" style="display: block; text-align: center;"><img src="https://plotly.com/~nicole.semerano/1.png?share_key=52HVgknBtJDJ80qU5ftyXE" alt="All_Star_Feature_Importance" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plotly.com/404.png';" /></a><script data-plotly="nicole.semerano:1" sharekey-plotly="52HVgknBtJDJ80qU5ftyXE" src="https://plotly.com/embed.js" async></script>
</div>
