   # Portfolio
### Applied Data Science, KB-74
### Petri Johannes Pellinen
### Student Number: 18116256

1. ### Introduction

My name is Petri Pellinen, I am a 25-year old student from Tampere, Finland. At my home university (Tampere University of Applied Sciences) I study software engineering in the department of ICT-engineering. This is my third year of a four-year program to graduate with a title of ICT-engineer or Bachelor of Science. I’m now studying Applied Data Science in The Hague University of Applied Sciences. It is a course of different group projects. My project group is working on a project called Cyber Security. In the project we use machine learning and programming skills learned as the course goes on. Programming learning happens mainly in learning environment called Data Camp, where we are assigned programming tasks to complete. This portfolio is a proof of my efforts on the group project.

2. ### Jargon
 
Frequent and important words used in Cybersecurity.

 * _API_.- Abbreviation for application programming interface: a way ofcommunicating with a particular computer program or internet service.
 
 * _Cyberattack_.- An attempt by hackers to damage or destroy a computer network or system.
 
 * _Cybersecurity_.- The state of being protected against the criminal or unauthorized use of electronic data, or the measures taken to achieve this.
 
 * _Hacktivism_.- The activity of getting into computer systems without permission inorder to achieve political aims.
 
 * _Hacktivist_.- A person who gains unauthorized access to computer files or networks in order to further social or political ends.
 
 * _Sentiment Analysis_.- The process of computationally identifying and categorizing opinions expressed in a piece of text, especially in order to determine whether the writer's attitude towards a particular topic, product, etc. is positive, negative, or neutral.
 
 * _Social media_.- Refers to websites and computer programs that make communication possible with the use of computers or mobile phones.


                
3. ### Project overwiev

Social networks have been a powerful platform to achieve globalization in recent years, huge amounts of data is generated every day. Sharing information with a large demographic has never been easier. Our focus on this project is one of the most popular social media platforms; Twitter. More importantly for us, we focus on a problem called hacktivism. Hacktivism is the use of technology to promote political agenda or social change. Hacktivists often promote something like freedom of information and human rights, of which both are very important but the methods are often questionable.
‘Cyber Security’ is a project that aims to create two working classifiers for Twitter data. The main goal is identifying hacktivists based on their tweets. This is achieved by building two classifiers with different functions and machine learning and training them with Twitter data. The two classifiers tell if the user is involved in the hacktivism scene and the machine learning part tries to detect patterns and make predictions on Twitter data. In this kind of data, some text pre-processing and manual labelling is necessary before the data can be actually used in the classifiers. My effort on the project focused mainly on the data cleaning and processing.


4. ### Data Camp  

Here is a proof of completing the assigned tasks in Data Camp. In the picture is the main banner of my Data Camp account showing XP earned and the amount of completed courses and exercises and the proof of completion of assigned tasks.
    
     
   ![Account banner](banner.JPG)
   ![Tasks_1](tasks1.JPG)
   ![Tasks_2](tasks2.JPG)
     
     
 5. ### My Work on the project
 
 In this chapter I will present and explain my work on the project with parts of the actual python code used in the project. I will go over my work on the code in small parts. I will also present the results my code with pictures and explanations. All our coding was done in Jupyter Python Notebooks, so all examples are from a Python Notebook. I will add the whole notebook to the end of this portfolio.
 
 
 
 ![Coursera](Coursera_1.png)
 
  ![Coursera](Coursera_2.png)


6. ### Python Notebooks about the project as an evidence of my abilities

In this section I put some of my personal programmes that we used for our project, the Python Notebooks are uploaded as a file in the Github so it is possible to look at it through the links with only one click and then come back to this section. Bellow each notebook I tried to make a summary of what I did with the code and maybe what I was expecting to do while I was developping this codes. Also can be some parts that are not completed or maybe some parts that was not useless for our goals at the last. Anyway, I will try to be the shortest possible, but if there is any doubt the code is in the link of each Python Notebook.
   
   * 1.- [Extract tweets](MAIN-Excell.pdf)
   
   In this code, the thing that I've done it was to extract the tweets from the accounts that our project owner gave us. The procedure of this is quite simple, the main thing here is to have the Tokens from Twitter´s development accounts, so once we got that I used Tweepy library to began exctraction, from an excel file which contained the names of the hacktivist accounts, then I put those names in a list, after that, a file was opened and each a loop was made, so each user got an extraction with a Tweepy method and the switch to the other user and so on, then those tweets were storaged in the 'txt' file. The fieldas that were extracted were 'Name', 'Lenght', 'ID','Date','Source','Likes' and 'Rts'. Besides that that information for each tweet was stored in a DataFrame using Pandas.
     
   * 2.- [Cleaning Data](Cleaning_Data_Programme.pdf)  
   
   In this file, it's where the data were cleaned, first of all the data was imported from an excel file to a Dataframe, after that we dropped the columns that were useless for our goal, in order to labeled the data the only columns required were the 'Tweets' and 'Name' columns, one more was add it, the length of the clenaed tweets. After that there is a function that is going to take away all the characters that we do not need, for example '@' and '#' also the links were cleaned from the tweets, and the non-english tweets also were dropped. So once again the power of the loops was shown because each tweet go through this function and stored in a Dataframe, after that those tweets were saved into a csv file. In that file was some missing values due to there was some tweets that was not in english and they were dropped, that row of the column was empty, so I just used a single method to erase those rows from the column in order to have the more cleaned data possible. Once again the tweets were stored in a file and there was the file in where the team labeled the tweets. 
   
   * 3.- [Naive Bayes and SMV comparation](Naive_Bayes_SMV_comparation_Classifiers.pdf)  
   
   Once again the first task in this programme is to upload the labeled file into a DataFrame using Pandas methods. With this we can have a better manipulation through the data. We used read_csv and then is ready, there was one more column that was added and it was the length of the labeled cleaned tweets. Then we figure out a plot about the tweets and it´s frequency to have a better vizualitation  about our data. We use Count Vectorizer to transform each word of a tweet in a matrix, so we put a value for each single word of the tweets this in order to check the frequency of the words in our classifiers, we split our data in training data and test data to start the machine learning, I started with multinomial Naive Bayes classifier, in a list I put different values of alpha in order to reach the best value that fit to the model and once again put that in a loop so al the end I got many values and I chose the one with the best test precision. So we filtered and we got the best alpha value, then I tried to make the confusion matrix, but I failed, and I still work on that. After that I made the same but for the SVM classifier, it was the same procedure but in this time the value that was changing in the loop was the C value, so then I made a table to filter the best results of that value in order to get the best test precision and accuracy. At the same the confusion still not ready so I am working on that, and the last part of the code it is another format, but I think it will be wise to work with matrices first. With this the team compared both classifiers and conclude that Naive Bayes got the best results, in this dataset.
 
7. ### Some visual content
   #### All the fragments of code that are bellow in this section, they are in the links of python codes that are shown above.
   
   Those are also some graphics that I did in our code to see the most frequent words in the Tweets, they are the first ones because still missing to take away the common english words like 'the', 'a', and that word. But here, it's a good approach on the handle on the tools for the data vizualitation.
   
   In this part I am showing the extracts of the tweets that we did at the begining of the project.
   
   ![Extraction](Extraction.png) 
   
   Here is the code used to clean the data from those characters that are unimportant for os.
   
   ![Cleaning](Cleaning.png) 
   
   ![Cleaning_2](Cleaning_2.png) 
   
   
   Those are some graphics about the frecuency of words from hacktivist users and non-hacktivist users.
   
 
   ![Graphic1](Graphic_1.png) 
   
 
 
   ![Graphic2](Graphic_2.png) 
   
   Here I put the two classifiers that I did, its a table that shows different values for each iteration so then we can see which one is 
   the best value to use. 
   In this case this is Naive Bayes
    
   ![Naive_Bayes](Naive_Bayes.png) 
   
   And here is the same comparation but with SMV
   
   ![SMV](SMV.png) 
   
   Now we can see in this block of code that we choose the best value for each classifier.
   
   Naive Bayes
   
   ![Best_Naive_Bayes](Best_Naive_Bayes.png) 
   
   SMV
   
    ![Best_SMV](Best_SMV.png) 
    
8. ### Planning Tools

     The tool that we used for planned our tasks in each sprint was Microsoft Planner, we use that to communicate each team member
     the tasks that must be done. This is a simple but a very efficient tool because I also have it in my phone so I can see my tasks anywhere, since we began the project we have used this tool and in my opinion, it has been a good way to plan out tasks and time, deadlines and delivers.
     
  ![Microsoft_Planner](Tool.png)

9. ### Presentations
 
 Each of us have to present at least two times the progress, problems or just approach of our project, these are the presentations that I did with the group, the next week will be my last presentation, so that is why I only attached two, because the last one and the final one they are not ready yet. 
    
    
   * 1.- [My first Presentation](cybersecurity_week3.pdf)
     
     
   * 2.- [My Second Presentation](cybersecurity_week9.pdf)
     

10. ### References

    * 1.- Raúl E. López Briega. (2017). Procesamiento de Lenguaje Natural con Python. 25/10/2018, de Blog Sitio web: https://relopezbriega.github.io/blog/2017/09/23/procesamiento-del-lenguaje-natural-con-python/ 
    
    * 2.- tthustla. (2018). Another Twitter Sentiment Analysis with Python - Part 1. 26/10/2018, de GitHub Sitio web: https://github.com/tthustla/twitter_sentiment_analysis_part1/blob/master/Capstone_part2.ipynb
    
    * 3.- Alec Go, Richa Bhayani y Lei Huang (2009). Twitter Sentiment Classification using Distant Supervision,*academia.edu.9*,1-6.
    
    * 4.- Cambridge University Press. (2018). Cambridge Dictionary. 09/11/2018, de Cambridge University Press Sitio web: https://dictionary.cambridge.org/dictionary/english/
    * 5.- Pablov. (november 09, 2016). Spam classification with Naive Bayes and Support Vector Machines. december 04, 2018, de Kaggle Sitio web: https://www.kaggle.com/pablovargas/naive-bayes-svm-spam-filtering
    
    * 6.- Jacob. (Mayo 10, 2010). TEXT CLASSIFICATION FOR SENTIMENT ANALYSIS – NAIVE BAYES CLASSIFIER. Diciembre 03, 2018, de StreamHacker Sitio web: https://streamhacker.com/2010/05/10/text-classification-sentiment-analysis-naive-bayes-classifier
    
