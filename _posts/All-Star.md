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

I also dealt with the obvious class imbalance.  There are many methods to accomidating this, but I chose to do it before the actual training of the model.  I wanted to start fresh.  I multiplied the minority class by 5 to oversample it.  I experimented with higher numbers but this caused the model to predict more 0s than it should.  

# When you come to a fork in the road, take it.

Yogi Berra was a wise man in his own way.  And one of his famous quotes came into play during my project.  I experimented with many models with my data.  At the time I knew of five models (ok, really six but was told to rarely use SVM) so I tested them all out. KNN and Logistic Regression models had a decent accuracy but a low AUC score.  Plus I knew it would probably be too simple with my features. Gaussian Naiive Bayes and Decision Trees both had low accuracy scores.  Goodbye.  But Random Forrest showed promise.  It was the only accuracy score in the 90s with a .9092 accuracy and I liked the way the ROC-AUC curve looked.  I was excited that I thought I found my model.

The next day we learned in class about the XG Boost model.  This model created a recursive function to correct small errors over each round of analyzing the data.  And since my model had tens of thousands of lines, not millions, the fact that a Boost model could be time consuming didn't really apply here.  I played around with the parameters of the model, mostly the learning rate.  Long story short, my XG Boost model produced a .9128 accuracy score and a slightly better AUC area than the Random Forest.  It was settled, XG Boost was my model.  

I then spent the next few days working on this project using the XG Boost model.  I was making visualizations, testing examples in Jupyter notebook, and much more that I will explain in a minute.  But then I went to create a Flask App for managers and/or fans to input their data into.  I hit a road block.  I kept getting an error "ValueError: feature_names mismatch".  I tried it all to fix this error.  I then learned this is a small problem with XG Boost.  As it goes through its many rounds of updating, sometimes a column gets dropped.  The model is ok with this until you try to classify a new piece of data.  So here I was in my Flask app trying to test out sample numbers of a possible All-Star, and the model couldn't do it.  So I came to the conclusion I had to say goodbye to the XG Boost model and go with Random Forest instead. I sacrificed the 1% accuracy for something reliable.


I also adjusted the threshold after training which is another way to find the right balance and make the best model.  I ended up finding the threshold of .25.  This created the following confusion matrix:

<p align="center">
  <img width="500" height="375" src="../images/ConfusionMatrix.PNG" alt="Confusion Matrix">
</p>

I settled on this with my model for a couple of reasons.  One, it realistically portrays the rate at which people are selected to the All-Star team.  The True Negatives are the many people who do not make the team while the True Positives are the select few that do.  Secondly, I kept my False Negatives low.  I would hate for my model to keep someone off the All-Star team that truely deserved it.  Yet, this threshold kept my False Positives from being too high as some other thresholds did.  I would rather have a player be on the team that really didn't deserve it.  From a fan perspective, my undeserving player makes the team...I'm happy and none the wiser.  But my deserving player is kept off...I'm infuriated and devistated.

# Scorecard
<div>
    <a href="https://plotly.com/~nicole.semerano/1/?share_key=52HVgknBtJDJ80qU5ftyXE" target="_blank" title="All_Star_Feature_Importance" style="display: block; text-align: center;"><img src="https://plotly.com/~nicole.semerano/1.png?share_key=52HVgknBtJDJ80qU5ftyXE" alt="All_Star_Feature_Importance" style="max-width: 100%;width: 600px;"  width="600" onerror="this.onerror=null;this.src='https://plotly.com/404.png';" /></a>
    <script data-plotly="nicole.semerano:1" src="https://plotly.com/embed.js" async></script>
</div>
