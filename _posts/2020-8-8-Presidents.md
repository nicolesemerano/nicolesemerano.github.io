---
layout: post
title: "Presidential Speech Analysis with Natural Language Processing"
---

#### By: Nicole Semerano

Natural Language Processing intrigued me from the beginning.  I first heard about it at a presentation of an author predictor based on word patterns.  It later was explained to me with song lyrics across the decades.  So I sought to also use it in an historic way.  And where could I get a lot of data?  Presidents: they love to talk!  Or at least most did.  So I created a NLP project on Presidential speeches in American history. It includes all 44 presidents from George Washington's first inauguration in 1789 to speeches on Coronavirus at the end  April of 2020. Yes, I said 44 presidents as Grover Cleveland had two separate terms.  Fair warning as you read along:  I was an American Studies major and taught history for 12 years.  So don’t mind me as I throw in some presidential knowledge along with my knowledge on Data Science.  Enjoy!

I scraped the majority of my speeches with Selenium from UVA's Miller Center.   Their collection of speeches and other primary sources is considered top-notch, even being referenced by Harvard’s database.  The page required scrolling so I needed more than your basic Beautiful Soup in scraping. 

<p align="left">
  <img width="33%" height="33%" src="../images/No._Pres_Speeches.png" alt="Speeches Count Bar Graph" align = "left"/> 
<p align="right">
    <br></br>
    
As I started cleaning up and analyzing my data, I realized some presidents should have had more speeches than were present in this collection.  I could fix this for Truman and Eisenhower by adding in their missing State of the Union Speeches from the NLTK’s corpus.  Every other president in this corpus had their SOU speeches already in the Miller Center collection.  In total my analysis included 1018 speeches with approximately 23.8 million words.  The math side of me did have to look at a few numbers and stats.  The shortest speech came from George Washington’s Second Inaugural Address with 787 words.  On the other hand, the longest speech goes to Harry Truman’s State of the Union address in 1946 at just shy of 170 thousand.  He had to discuss such historic topics as the post-war economy, protection for veterans, the creation of the United Nations, communism concerns...and that doesn’t even cover the first half!  
  
The data, each being a full speech, has understandable asymmetry.  I could have scraped other speeches from other sources, but it would have made it extremely skewed to modern times.  Technology such as the radio and television led to more speeches versus letters written.  Examples of this already in the data set include televised Addresses to the Nation and FDR’s Fireside Chat’s. I color coded my bar graph to represent presidents who served more than one term versus 4 or less years.  Lyndon Johnson, with the most, had historic events such as the Civil Rights Acts and the Vietnam War to address.  On the flip side, the two presidents with only one speech in the collection died months into their term. William Henry Harrison died specifically BECAUSE he gave his 2-hour inaugural speech in a snowstorm outside without a coat on. 
</p>
<p>
  <br></br>
  <br></br>
</p>

## Topic Modeling
I first split up each speech to check the frequency of all the words.  Here I was able to make my own list of stop words in preparation of count vectorizing.  Some words I was not surprised to find, like ‘united’, ‘states’ and ‘america’.’ One that caught me off guard but made sense was ‘thank’ and ‘you’.  Think of how many speeches start off with that phrase or are thanking people later on for their accomplishments.  Two other words of note are ‘applause’ and ‘transcript’.  This just shows how the speeches are documented for history along with how presidents and their speech writers make notes within the speech. 

I processed the data with a Count Vectorization with a df-maximum of 40% . This allowed many of the random topics to be filtered out of my topic modeling.  On the other hand I combined the NLTK’s stopwords with my list of over a dozen words to filter out the words that show up too much.  I settled on the nonnegative matrix factorization(NMF) for my modeling. I experimented with various topic modelers and vectorizers.  Using my knowledge of history, I did not like anything about what LDA put out.  Within NMF, the TDIF Vectorizer created mismatched topics. It had two different categories based on the Vietnam War yet missed major topics that I found in my final result that I will discuss below.  I also experimented with how many topics to focus on.  This part of NLP really is like Goldielocks and the 3 Bears.  I tried 5, that was too little.  I tried 15 and that was too much.  My initial run of 10 ended up being the perfect amount.   

When I ran it that first time I got to appreciate NLP’s full power.  The first two topics were words that related to domestic and economic affairs.  Ok, makes sense.  Then I read the 3rd topic.  I realized all the words had to do with slavery and the antebellum compromises.  I was ecstatic.  NMF was supposed to be better for short text documents.  However its benefits shined through.   NMF learns topics by directly decomposing the term-document matrix and it reduces the dimensions to find the main topics of the speeches.

I set out to find a correlation with the 10 topics.  Topic modeling showed me some topics I wouldn’t expect, such as the Civil Service establishment.  I used to teach this in about 20 minutes yet this shows teachers should give this more emphasis. Also, there was a topic I best named ‘Encouragement’.  You have Presidents that have campaign slogans of positivity that make it into their presidency, but that has been going on for years.   This topic shows up in the 1950s with the advent of television and proves that television has shaped the presidency probably more than any invention.  There  are a few topics that no matter my tinkering, I was surprised never showed up as a major topic. There were no words or topics that I could find that related specifically to the World Wars or the creation of new amendments.  

<p align="center">
  <img  width="100%" height="100%"src="../images/Speech_heatmap.png" alt="Speeches Timeline Heatmap">
</p>

??????? Do I put in code after first two sentences of how I grouped topics??????????????

I’m a visual learner and this heatmap taught me even more.  Summing up the words of each topic you can observe them across time.  The darkest box belongs to the Cold War in the 1960s.  This time period whether good(Space Race) or bad (Cuban Missile Crisis) encompassed the United States in many ways.  The darker stretch in the 1800s Politics demonstrates how this is a time period that should not be shortchanged in its teachings.  The politics around the National Bank were a debate for years.  The Mexican American War and Manifest Destiny thoughts formulated the shape and land of our country. 

## Sentiment Analysis

The second major analysis I did was sentiment analysis.  I went into this cautious as I knew my data is pre-written speeches.  Half the time they are not written by the president himself but by a team of speechwriters. If you want real true feelings go listen to the LBJ's tapes on YouTube. Or maybe you follow Donald Trump on Twitter.  However, in the end I saw sentiment results that matched many of our presidents. 

**************Put Tableau Dashbord???? Or just pics*****

So as you can see Dwight Eisenhower's had the highest positivity sentiment which can be explained by the prosperity, culture and good economy of the 1950's. I was surprised that there was not lower positivity during hard times such as the Great Depression, the Vietnam War, or other wars.  I did look at negative sentiment as well and there was not a peak then either. I guess this gets back to those encouragement vocabulary where US presidents used rally cries and language of reassurance during the hard times. 


