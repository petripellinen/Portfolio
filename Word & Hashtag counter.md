

```python
import re
list=['#cat','#dog','a#yacht','cats']
regex=re.compile(".*(#).*")
[m.group(0) for l in list for m in [regex.search(l)] if m]


```




    ['#cat', '#dog', 'a#yacht']




```python
# General:
import tweepy           # To consume Twitter's API
import pandas as pd     # To handle data
import numpy as np      # For number computing
import json
import string
from collections import Counter
from nltk.corpus import stopwords
import nltk
nltk.download('stopwords')
stop = stopwords.words('english')

# For plotting and visualization:
from IPython.display import display
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline

# Twitter App access keys for @user

# Consume:
CONSUMER_KEY = "nui9d52bUoGa1tZBvvy4HGxTl"
CONSUMER_SECRET = "qTJ5W8EX2b2eFvKtSw80wIw714Hs9rXpUsb0RVwiGxAbyS4cnl"

# Access:
ACCESS_TOKEN = "1034084192936763393-FFXRmlq3PaoBWjjCdA7GLeP9kEIJD8"
ACCESS_TOKEN_SECRET = "tQvaBmXK0qlS2GSaxB7cLWneAVieYeomxN3ED8KrOp1b1"

#-----------------------------------------Excell extract-------------------------------------------------------------
archivo_excel = pd.read_excel('Twitter_accounts_with_IDs.xlsx')
#print(archivo_excel.columns)
values = archivo_excel['Twitter accounts'].values
#print(type(values))
columnas = ['Twitter accounts']
df_seleccionados = archivo_excel[columnas]
username=values
#print(df_seleccionados[columnas])

#----------------------------------------------------Authentication function----------------------------------------------------
def twitter_setup():
    """
    Utility function to setup the Twitter's API
    with our access keys provided.
    """
    # Authentication and access using keys:
    auth = tweepy.OAuthHandler(CONSUMER_KEY, CONSUMER_SECRET)
    auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)

    # Return API with authentication:
    api = tweepy.API(auth)
    return api
#----------------------------------------------------------Tweets Mining--------------------------------------------------------
result = pd.DataFrame()
#display(result)
while True:
    try:
        file = open("Tweets_file1.txt", "w")
        for username in username:
            print("The user name is:")
            print(username)
# We create an extractor object:
            extractor = twitter_setup()
   
# We create a tweet list as follows:
            tweets = extractor.user_timeline(screen_name=username , count=2000)
            #print("Number of tweets extracted: {}.\n".format(len(tweets)))

# We print the most recent 5 tweets:
#print("5 recent tweets:\n")
            file.write(str(username+"\n"))
            for tweet in tweets[0:-1]:
                file.write(str(tweet.created_at))
                file.write(tweet.author.screen_name)
                file.write(tweet.text)
                #file.write(tweet.text)
                #print(tweet.text)
                #print()

# We create a pandas dataframe as follows:
            data = pd.DataFrame(data=[tweet.text for tweet in tweets], columns=['Tweets'])


# We add relevant data:
            data['Name']= np.array([tweet.user.screen_name for tweet in tweets])
            data['Length']  = np.array([len(tweet.text) for tweet in tweets])
            data['ID']   = np.array([tweet.id for tweet in tweets])
            data['Date'] = np.array([tweet.created_at for tweet in tweets])
            data['Source'] = np.array([tweet.source for tweet in tweets])
            data['Likes']  = np.array([tweet.favorite_count for tweet in tweets])
            data['RTs']    = np.array([tweet.retweet_count for tweet in tweets])

# Display elements from dataframe:
            display(data)
            #print("merge")
        
            result = result.append(data,ignore_index=True)
            #display(result)
            
        
            
    except tweepy.TweepError:
        print("Failed to run the command on that user, Skipping...")
        print("User´s profile in a private mode, imposible extract tweets")
        break

```

    [nltk_data] Downloading package stopwords to
    [nltk_data]     /home/14058448/nltk_data...
    [nltk_data]   Unzipping corpora/stopwords.zip.
    The user name is:
    @AkincilarCW



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @ajmhashtag: من اخترق موقع وكالة أنباء الشر...</td>
      <td>AkincilarCW</td>
      <td>92</td>
      <td>1039757171754446848</td>
      <td>2018-09-12 06:06:42</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @BBCArabic: صورة قيادي بالإخوان تتصدر صفحة ...</td>
      <td>AkincilarCW</td>
      <td>139</td>
      <td>1039757123905888256</td>
      <td>2018-09-12 06:06:30</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @Elsanhory: اختراق موقع وكالة أنباء الشرق ا...</td>
      <td>AkincilarCW</td>
      <td>75</td>
      <td>1039757024937091072</td>
      <td>2018-09-12 06:06:07</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @BBCMonitoring: Egypt's official news agenc...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1039756976249556992</td>
      <td>2018-09-12 06:05:55</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @ElHady: عاجل: اختراق موقع وكالة أنباء الشر...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1039756828115120128</td>
      <td>2018-09-12 06:05:20</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>45</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @3yyash: Middle East News Agency, the state...</td>
      <td>AkincilarCW</td>
      <td>139</td>
      <td>1039756800143360000</td>
      <td>2018-09-12 06:05:13</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>RT @RassdNewsN: هاكرز تركي يخترق وكالة أنباء ا...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1039756543313502208</td>
      <td>2018-09-12 06:04:12</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RT @RassdNewsN: اخترق هاكر، وكالة أنباء الشرق ...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1039756367115022336</td>
      <td>2018-09-12 06:03:30</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @alkhames: قالت الهيئة الوطنية للصحافة في م...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1039756181164699648</td>
      <td>2018-09-12 06:02:45</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>107</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RT @CyberWarriorTIM: Türk Hackerlar Mısır Resm...</td>
      <td>AkincilarCW</td>
      <td>127</td>
      <td>1039756113879748608</td>
      <td>2018-09-12 06:02:29</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RT @CyberWarriorTIM: Türk Hackerlar Mısır Resm...</td>
      <td>AkincilarCW</td>
      <td>116</td>
      <td>1039756101418471425</td>
      <td>2018-09-12 06:02:26</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>11</th>
      <td>RT @CyberWarriorTIM: Türk Hackerlar Mısır Resm...</td>
      <td>AkincilarCW</td>
      <td>106</td>
      <td>1039756091092099072</td>
      <td>2018-09-12 06:02:24</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>12</th>
      <td>RT @CyberWarriorTIM: Türk Hackerlar Mısır Resm...</td>
      <td>AkincilarCW</td>
      <td>99</td>
      <td>1039756079868067847</td>
      <td>2018-09-12 06:02:21</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>22</td>
    </tr>
    <tr>
      <th>13</th>
      <td>RT @CyberWarriorTIM: Türk Hackerlar Mısır Resm...</td>
      <td>AkincilarCW</td>
      <td>105</td>
      <td>1039756068757430272</td>
      <td>2018-09-12 06:02:19</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>20</td>
    </tr>
    <tr>
      <th>14</th>
      <td>RT @CyberWarriorTIM: Türk Hacker Grubu @CyberW...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1039494872724586496</td>
      <td>2018-09-11 12:44:25</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>75</td>
    </tr>
    <tr>
      <th>15</th>
      <td>RT @CyberWarriorTIM: Kurban Bayramının, Ülkemi...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1033283292097929216</td>
      <td>2018-08-25 09:21:48</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>16</th>
      <td>RT @CyberWarriorTIM: Kendilerini "kürdish hack...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1030059946237485056</td>
      <td>2018-08-16 11:53:23</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>88</td>
    </tr>
    <tr>
      <th>17</th>
      <td>RT @yusufhanedangil: Bu ülkede Vatanseverler, ...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1008337890189996032</td>
      <td>2018-06-17 13:17:41</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>44</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @TSKGnkur: 15 Haziran 2018 tarihinde Irak k...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1008337857361203201</td>
      <td>2018-06-17 13:17:33</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>1398</td>
    </tr>
    <tr>
      <th>19</th>
      <td>RT @CyberWarriorTIM: @CyberWarriorTIM | #AKINC...</td>
      <td>AkincilarCW</td>
      <td>110</td>
      <td>1008337573805215745</td>
      <td>2018-06-17 13:16:26</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>20</th>
      <td>RT @CyberWarriorTIM: @CyberWarriorTIM Oku, Öğr...</td>
      <td>AkincilarCW</td>
      <td>123</td>
      <td>1008337563856396288</td>
      <td>2018-06-17 13:16:23</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>21</th>
      <td>RT @CyberWarriorTIM: Başta Aziz Şehitlerimizi"...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>1008337548459102208</td>
      <td>2018-06-17 13:16:20</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>21</td>
    </tr>
    <tr>
      <th>22</th>
      <td>RT @dirilispostasi: Akıncılar Hacker Grubu Dir...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>992400493577166849</td>
      <td>2018-05-04 13:48:10</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>23</th>
      <td>RT @eNeSVrol: Selamünaleyküm FSM hastanesinde ...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>992143345400107010</td>
      <td>2018-05-03 20:46:21</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>21</td>
    </tr>
    <tr>
      <th>24</th>
      <td>"Çılgın Türkler AKINCILAR" https://t.co/xNRr6B...</td>
      <td>AkincilarCW</td>
      <td>50</td>
      <td>991550849557127168</td>
      <td>2018-05-02 05:31:59</td>
      <td>Twitter Web Client</td>
      <td>62</td>
      <td>48</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Avrupa’da gündem AKINCILAR 🇹🇷\n\nhttps://t.co/...</td>
      <td>AkincilarCW</td>
      <td>54</td>
      <td>991330845431607296</td>
      <td>2018-05-01 14:57:46</td>
      <td>Twitter for iPhone</td>
      <td>62</td>
      <td>21</td>
    </tr>
    <tr>
      <th>26</th>
      <td>RT @CyberWarriorTIM: Türk Hackerler Yunan'ı Çı...</td>
      <td>AkincilarCW</td>
      <td>78</td>
      <td>991330506942726144</td>
      <td>2018-05-01 14:56:25</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>82</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Yunanistan’ın gündemi AKINCILAR 🇹🇷\n\nhttps://...</td>
      <td>AkincilarCW</td>
      <td>59</td>
      <td>991330464764919809</td>
      <td>2018-05-01 14:56:15</td>
      <td>Twitter for iPhone</td>
      <td>74</td>
      <td>21</td>
    </tr>
    <tr>
      <th>28</th>
      <td>RT @TSKGnkur: Siirt-Eruh’da icra edilen hava d...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>990917193696243712</td>
      <td>2018-04-30 11:34:03</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>266</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RT @CyberWarriorTIM: "Alt Yapı Çalışmalarımız"...</td>
      <td>AkincilarCW</td>
      <td>95</td>
      <td>989835193627369477</td>
      <td>2018-04-27 11:54:35</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>51</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>168</th>
      <td>@Sabah bünyeniz de Mevlüt Tezel denen müptezel...</td>
      <td>AkincilarCW</td>
      <td>106</td>
      <td>921695614315978752</td>
      <td>2017-10-21 11:12:13</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>169</th>
      <td>@AkinSaglamAS Whatsapp tan konum yollar mısın 👍</td>
      <td>AkincilarCW</td>
      <td>47</td>
      <td>921434159264026624</td>
      <td>2017-10-20 17:53:17</td>
      <td>Twitter for iPhone</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Ruhu şad mekanı cennet olsun\n\n #20EkimDünyaK...</td>
      <td>AkincilarCW</td>
      <td>78</td>
      <td>921352814676381697</td>
      <td>2017-10-20 12:30:03</td>
      <td>Twitter for iPhone</td>
      <td>31</td>
      <td>10</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Hayırlı cumalar\nAllah dualarınızı kabul etsin</td>
      <td>AkincilarCW</td>
      <td>45</td>
      <td>921332339359322112</td>
      <td>2017-10-20 11:08:41</td>
      <td>Twitter for iPhone</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>172</th>
      <td>@aydinerol20 @ilyassalmanfan @mervepekdemirrr ...</td>
      <td>AkincilarCW</td>
      <td>138</td>
      <td>921312111841685504</td>
      <td>2017-10-20 09:48:19</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>173</th>
      <td>@ilyassalmanfan @mervepekdemirrr @06melihgokce...</td>
      <td>AkincilarCW</td>
      <td>110</td>
      <td>921044588189667333</td>
      <td>2017-10-19 16:05:16</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>174</th>
      <td>@basaranalper Maalesef, saçmalamışsınız</td>
      <td>AkincilarCW</td>
      <td>39</td>
      <td>921042297596403713</td>
      <td>2017-10-19 15:56:10</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>175</th>
      <td>@DefacerDarkBat Tebrikler yiğidim\nAma aç gözü...</td>
      <td>AkincilarCW</td>
      <td>140</td>
      <td>921041874944741377</td>
      <td>2017-10-19 15:54:29</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>176</th>
      <td>Kişiliğini makamından alanlar\nMakamdan sonra ...</td>
      <td>AkincilarCW</td>
      <td>66</td>
      <td>921039326091399168</td>
      <td>2017-10-19 15:44:22</td>
      <td>Twitter for iPhone</td>
      <td>14</td>
      <td>3</td>
    </tr>
    <tr>
      <th>177</th>
      <td>RT @sorularlaislam: “Bütün Âdemoğulları günahk...</td>
      <td>AkincilarCW</td>
      <td>125</td>
      <td>921037284115451906</td>
      <td>2017-10-19 15:36:15</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>22</td>
    </tr>
    <tr>
      <th>178</th>
      <td>@TarikkBjk @ufukcc Bu bikini giyip sözünün eri...</td>
      <td>AkincilarCW</td>
      <td>73</td>
      <td>919633736135856129</td>
      <td>2017-10-15 18:39:03</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>179</th>
      <td>@Kwonmusti @TarikkBjk @ufukcc elem tere fiş ke...</td>
      <td>AkincilarCW</td>
      <td>61</td>
      <td>919620192120004608</td>
      <td>2017-10-15 17:45:14</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>180</th>
      <td>@TarikkBjk @sercanarga Bazen tehlikeli seyler ...</td>
      <td>AkincilarCW</td>
      <td>79</td>
      <td>919619256043655168</td>
      <td>2017-10-15 17:41:30</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>181</th>
      <td>@sercanarga Maç için değil Fenerbahçe için \n@...</td>
      <td>AkincilarCW</td>
      <td>83</td>
      <td>919611993132294145</td>
      <td>2017-10-15 17:12:39</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>182</th>
      <td>@sercanarga En son 160’la geliyordum https://t...</td>
      <td>AkincilarCW</td>
      <td>60</td>
      <td>919610945923665920</td>
      <td>2017-10-15 17:08:29</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>183</th>
      <td>@ufukcc @TarikkBjk @Kwonmusti daha geçen 2 haf...</td>
      <td>AkincilarCW</td>
      <td>109</td>
      <td>919607078313975810</td>
      <td>2017-10-15 16:53:07</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>184</th>
      <td>@TarikkBjk @Kwonmusti @ufukcc Bak bu 2 oldu :))</td>
      <td>AkincilarCW</td>
      <td>47</td>
      <td>919602813625348096</td>
      <td>2017-10-15 16:36:10</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>185</th>
      <td>@Kwonmusti @ufukcc @TarikkBjk Bi daha da Trabz...</td>
      <td>AkincilarCW</td>
      <td>67</td>
      <td>919589196330885120</td>
      <td>2017-10-15 15:42:04</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>186</th>
      <td>@ufukcc @Kwonmusti @TarikkBjk Bir fenerli, maç...</td>
      <td>AkincilarCW</td>
      <td>87</td>
      <td>919582089892294656</td>
      <td>2017-10-15 15:13:49</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>187</th>
      <td>@ufukcc @Kwonmusti @TarikkBjk ta sessiz sedası...</td>
      <td>AkincilarCW</td>
      <td>89</td>
      <td>919581212490063872</td>
      <td>2017-10-15 15:10:20</td>
      <td>Twitter for iPhone</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>188</th>
      <td>@Kwonmusti Akşam Fener de böyle hezimete uğruy...</td>
      <td>AkincilarCW</td>
      <td>79</td>
      <td>919578563493486592</td>
      <td>2017-10-15 14:59:49</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>189</th>
      <td>@Kwonmusti 5 oldu\nAllah’ını seven defansa gelsin</td>
      <td>AkincilarCW</td>
      <td>48</td>
      <td>919577652813549569</td>
      <td>2017-10-15 14:56:12</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>190</th>
      <td>@Kwonmusti Müstehak abi</td>
      <td>AkincilarCW</td>
      <td>23</td>
      <td>919576477926076416</td>
      <td>2017-10-15 14:51:31</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>191</th>
      <td>@CwLazmania61 Daha bu ne ki</td>
      <td>AkincilarCW</td>
      <td>27</td>
      <td>919569353556877312</td>
      <td>2017-10-15 14:23:13</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>192</th>
      <td>@ebubekir3425 Gereksizin biri, medya maymunu</td>
      <td>AkincilarCW</td>
      <td>44</td>
      <td>919532458323914752</td>
      <td>2017-10-15 11:56:36</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>193</th>
      <td>RT @misvakdergi: Mazlumun umudu, zalimin korku...</td>
      <td>AkincilarCW</td>
      <td>98</td>
      <td>919272196836651008</td>
      <td>2017-10-14 18:42:25</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>132</td>
    </tr>
    <tr>
      <th>194</th>
      <td>#DünyaHayatınınGerçeği \n\nÖlüm Var</td>
      <td>AkincilarCW</td>
      <td>33</td>
      <td>919099372897558528</td>
      <td>2017-10-14 07:15:41</td>
      <td>Twitter for iPhone</td>
      <td>16</td>
      <td>8</td>
    </tr>
    <tr>
      <th>195</th>
      <td>@ufukcc @TarikkBjk Negredo nun 35 gole ulaşmas...</td>
      <td>AkincilarCW</td>
      <td>70</td>
      <td>918914813438451713</td>
      <td>2017-10-13 19:02:18</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>196</th>
      <td>RT @BA_Yildirim: Ankara'nın başkent ilan edili...</td>
      <td>AkincilarCW</td>
      <td>103</td>
      <td>918780701276889089</td>
      <td>2017-10-13 10:09:23</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>364</td>
    </tr>
    <tr>
      <th>197</th>
      <td>Cumanız mübarek olsun https://t.co/DahoOEyrg6</td>
      <td>AkincilarCW</td>
      <td>45</td>
      <td>918726110267035649</td>
      <td>2017-10-13 06:32:28</td>
      <td>Twitter for iPhone</td>
      <td>7</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>198 rows × 8 columns</p>
</div>


    The user name is:
    @Anon_Norway



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @mydataorg: Just two days until the importa...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1062688720627924993</td>
      <td>2018-11-14 12:48:29</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @BarrettBrown_: Today a man threatened to b...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1062585415356563457</td>
      <td>2018-11-14 05:57:59</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>242</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @svaar: FpU ikke overbegeistret for overvåk...</td>
      <td>Anon_Norway</td>
      <td>135</td>
      <td>1062485266412249088</td>
      <td>2018-11-13 23:20:02</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @attackerman: I'm going to tweetstorm about...</td>
      <td>Anon_Norway</td>
      <td>139</td>
      <td>1062452962168053761</td>
      <td>2018-11-13 21:11:40</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>53</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @RayJoha2: "Check yourself next time you ac...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1061699842299383808</td>
      <td>2018-11-11 19:19:02</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @UptheCypherPunx: Today is #RemembranceDay ...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1061616300756230145</td>
      <td>2018-11-11 13:47:04</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>33</td>
    </tr>
    <tr>
      <th>6</th>
      <td>RT @RayJoha2: I just sorta bought 'The Fox' by...</td>
      <td>Anon_Norway</td>
      <td>144</td>
      <td>1050481539102969857</td>
      <td>2018-10-11 20:21:30</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RT @BarrettBrown_: I’ll be speaking via videoc...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1050174604675870725</td>
      <td>2018-10-11 00:01:51</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @BarrettBrown_: I’m pleased to announce tha...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1049324558967345152</td>
      <td>2018-10-08 15:44:05</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>80</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RT @BarrettBrown_: Pursuance’s director of ope...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1047951101029470209</td>
      <td>2018-10-04 20:46:27</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RT @AnnaBurkhart: Cops kettling and arresting ...</td>
      <td>Anon_Norway</td>
      <td>99</td>
      <td>1047937561338019842</td>
      <td>2018-10-04 19:52:39</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>85</td>
    </tr>
    <tr>
      <th>11</th>
      <td>RT @welltraveledfox: This person was Trevor Fi...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1045783480926437377</td>
      <td>2018-09-28 21:13:06</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>12</th>
      <td>RT @RayJoha2: "MI5 spied on @privacyint and st...</td>
      <td>Anon_Norway</td>
      <td>139</td>
      <td>1044604065001476096</td>
      <td>2018-09-25 15:06:31</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>13</th>
      <td>RT @AnnaBurkhart: took me 20 minutes to reach ...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1041837542469971970</td>
      <td>2018-09-17 23:53:21</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>25</td>
    </tr>
    <tr>
      <th>14</th>
      <td>RT @aka_Gb: Non dimentichiamo!\n              ...</td>
      <td>Anon_Norway</td>
      <td>109</td>
      <td>1041834876251660295</td>
      <td>2018-09-17 23:42:45</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>29</td>
    </tr>
    <tr>
      <th>15</th>
      <td>RT @PPInternational: NOTICE: #PPInt General As...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1041832507895963649</td>
      <td>2018-09-17 23:33:20</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>16</th>
      <td>RT @trevortimm: Revealed for the first time: W...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1041744510026436608</td>
      <td>2018-09-17 17:43:40</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>507</td>
    </tr>
    <tr>
      <th>17</th>
      <td>RT @RayJoha2: #FindArjen UPDATE\n\n- The searc...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1041645619272671232</td>
      <td>2018-09-17 11:10:43</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>48</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @RayJoha2: Noorse politie heeft drie scenar...</td>
      <td>Anon_Norway</td>
      <td>117</td>
      <td>1041112844593496064</td>
      <td>2018-09-15 23:53:39</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>19</th>
      <td>RT @MejiaSouth: Community members shut down I-...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1041059597363150848</td>
      <td>2018-09-15 20:22:04</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>1154</td>
    </tr>
    <tr>
      <th>20</th>
      <td>RT @RayJoha2: "There are a number of facts wel...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1040558639361261568</td>
      <td>2018-09-14 11:11:26</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>21</th>
      <td>RT @Matthijs85: "Er zijn een aantal feiten bek...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1040558597506260992</td>
      <td>2018-09-14 11:11:16</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>22</th>
      <td>RT @BarrettBrown_: When I was facing a mandato...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1039997113701199872</td>
      <td>2018-09-12 22:00:08</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>26</td>
    </tr>
    <tr>
      <th>23</th>
      <td>RT @ncilla: #FindArjen update; Confirmation to...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1039898624417832960</td>
      <td>2018-09-12 15:28:47</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>29</td>
    </tr>
    <tr>
      <th>24</th>
      <td>RT @RayJoha2: Horrific news of a cataclysmic n...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1039859345884893185</td>
      <td>2018-09-12 12:52:42</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>49</td>
    </tr>
    <tr>
      <th>25</th>
      <td>RT @RayJoha2: Showdown on upload filters and t...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1039214363717066753</td>
      <td>2018-09-10 18:09:46</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>26</th>
      <td>RT @StigSjoeberg: #findarjen Press release - S...</td>
      <td>Anon_Norway</td>
      <td>118</td>
      <td>1039169784175321088</td>
      <td>2018-09-10 15:12:38</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>27</th>
      <td>RT @RayJoha2: -Free women dethrone mullahs💪 \n...</td>
      <td>Anon_Norway</td>
      <td>110</td>
      <td>1037106596847996928</td>
      <td>2018-09-04 22:34:15</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>28</th>
      <td>RT @UptheCypherPunx: Woke up this morning to f...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>1037008116062273537</td>
      <td>2018-09-04 16:02:56</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RT @bjwinnerdavis: Reality Winner was finally ...</td>
      <td>Anon_Norway</td>
      <td>123</td>
      <td>1035141668385841157</td>
      <td>2018-08-30 12:26:20</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>42</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>168</th>
      <td>RT @birgittaj: I spent a few days with the peo...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>998641418221621248</td>
      <td>2018-05-21 19:07:22</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>31</td>
    </tr>
    <tr>
      <th>169</th>
      <td>RT @raif_badawi: #RaifBadawi the Honorary citi...</td>
      <td>Anon_Norway</td>
      <td>93</td>
      <td>996509556703465472</td>
      <td>2018-05-15 21:56:07</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>77</td>
    </tr>
    <tr>
      <th>170</th>
      <td>RT @bjwinnerdavis: Please share and retweet. W...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>994351973808918534</td>
      <td>2018-05-09 23:02:39</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>67</td>
    </tr>
    <tr>
      <th>171</th>
      <td>After World War II, the US and its allies pros...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>994255643640025088</td>
      <td>2018-05-09 16:39:52</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>172</th>
      <td>An interested-in-Pursuance Windows user spun u...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>992897112038891520</td>
      <td>2018-05-05 22:41:33</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>173</th>
      <td>Nearly half (48 percent) of Israeli Jews now s...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>992770105393246208</td>
      <td>2018-05-05 14:16:52</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>174</th>
      <td>RT @BarrettBrown_: DOJ just granted me permiss...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>992747941914988544</td>
      <td>2018-05-05 12:48:48</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>71</td>
    </tr>
    <tr>
      <th>175</th>
      <td>"Kill one thousand of us and 10 thousand more ...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>992554388068683783</td>
      <td>2018-05-04 23:59:41</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>176</th>
      <td>Today the most wonderful thing happened. Out o...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>992524081454084096</td>
      <td>2018-05-04 21:59:15</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>177</th>
      <td>"We'll never have a free world without a free ...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>992123644498399234</td>
      <td>2018-05-03 19:28:04</td>
      <td>TweetDeck</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>178</th>
      <td>RT @tvaddonsco: Despite an overwhelming amount...</td>
      <td>Anon_Norway</td>
      <td>139</td>
      <td>992103542927319046</td>
      <td>2018-05-03 18:08:11</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>257</td>
    </tr>
    <tr>
      <th>179</th>
      <td>MPAA-Seized Popcorn Time Domain Now Redirects ...</td>
      <td>Anon_Norway</td>
      <td>84</td>
      <td>992102468489302016</td>
      <td>2018-05-03 18:03:55</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>180</th>
      <td>RT @UptheCypherPunx: Good news on #WorldPressF...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>992068288111882241</td>
      <td>2018-05-03 15:48:06</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>181</th>
      <td>New @intercepted podcast: War Games https://t....</td>
      <td>Anon_Norway</td>
      <td>59</td>
      <td>991643057979314176</td>
      <td>2018-05-02 11:38:23</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>182</th>
      <td>RT @RayJoha2: THREAD! Important 2 part article...</td>
      <td>Anon_Norway</td>
      <td>139</td>
      <td>990946873480564738</td>
      <td>2018-04-30 13:32:00</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>183</th>
      <td>RT @micahflee: For the last two years I've car...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>990618257223835648</td>
      <td>2018-04-29 15:46:11</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>353</td>
    </tr>
    <tr>
      <th>184</th>
      <td>At @theindignants podcast today: The #TorontoA...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>989916458393178113</td>
      <td>2018-04-27 17:17:30</td>
      <td>TweetDeck</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>185</th>
      <td>RT @PursuanceProj: THREAD!\n\n- UPDATE: Pursua...</td>
      <td>Anon_Norway</td>
      <td>104</td>
      <td>989830001062227969</td>
      <td>2018-04-27 11:33:57</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Aussie Federal Court Orders ISPs to Block Pira...</td>
      <td>Anon_Norway</td>
      <td>85</td>
      <td>989787026810601473</td>
      <td>2018-04-27 08:43:11</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>187</th>
      <td>How U.S. guns sold to Mexico end up with secur...</td>
      <td>Anon_Norway</td>
      <td>133</td>
      <td>989558830571900928</td>
      <td>2018-04-26 17:36:24</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>188</th>
      <td>See you in court, BOP! There are a few more ru...</td>
      <td>Anon_Norway</td>
      <td>120</td>
      <td>989527332791693314</td>
      <td>2018-04-26 15:31:15</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>189</th>
      <td>MPAA Chief Says Fighting Piracy Remains “Top P...</td>
      <td>Anon_Norway</td>
      <td>78</td>
      <td>989470341952163840</td>
      <td>2018-04-26 11:44:47</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>190</th>
      <td>RT @UptheCypherPunx: The last thing Alek Minas...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>988884759593013251</td>
      <td>2018-04-24 20:57:53</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>191</th>
      <td>RT @UptheCypherPunx: Please share: If you are ...</td>
      <td>Anon_Norway</td>
      <td>139</td>
      <td>988474195985289218</td>
      <td>2018-04-23 17:46:27</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>13</td>
    </tr>
    <tr>
      <th>192</th>
      <td>RT @DaPeaple: #WEDAFiles "Solidarity for Reali...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>988138041150763008</td>
      <td>2018-04-22 19:30:42</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>26</td>
    </tr>
    <tr>
      <th>193</th>
      <td>RT @UptheCypherPunx: Very excited to be joinin...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>988102683683905536</td>
      <td>2018-04-22 17:10:12</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>29</td>
    </tr>
    <tr>
      <th>194</th>
      <td>Im truly stoked and looking forward to meeting...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>988091634452697089</td>
      <td>2018-04-22 16:26:18</td>
      <td>TweetDeck</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>195</th>
      <td>Led by El Pais and Hamilton 68, a series of de...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>988016859160117248</td>
      <td>2018-04-22 11:29:10</td>
      <td>TweetDeck</td>
      <td>5</td>
      <td>5</td>
    </tr>
    <tr>
      <th>196</th>
      <td>RT @bjwinnerdavis: @DaPeaple @WendyMeer11 @sta...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>987649727163518981</td>
      <td>2018-04-21 11:10:19</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>197</th>
      <td>RT @DaPeaple: #WEDAFiles "Solidarity for Reali...</td>
      <td>Anon_Norway</td>
      <td>140</td>
      <td>987381071951081474</td>
      <td>2018-04-20 17:22:46</td>
      <td>TweetDeck</td>
      <td>0</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
<p>198 rows × 8 columns</p>
</div>


    The user name is:
    @Anon2earth



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>@Out_Of_Context @TownesVanPlants I will - “Lib...</td>
      <td>Anon2earth</td>
      <td>117</td>
      <td>1062563376478539776</td>
      <td>2018-11-14 04:30:25</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@XpektusAnon @thorharris666 @jaykelly26 I know...</td>
      <td>Anon2earth</td>
      <td>71</td>
      <td>1062561406170619904</td>
      <td>2018-11-14 04:22:35</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@MyWhiteNinja_ Imma have to get this Soros card</td>
      <td>Anon2earth</td>
      <td>47</td>
      <td>1062559273828433920</td>
      <td>2018-11-14 04:14:06</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @MyWhiteNinja_: Next up...apparently there’...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062559222313959424</td>
      <td>2018-11-14 04:13:54</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@XpektusAnon @thorharris666 @jaykelly26 Oh I’m...</td>
      <td>Anon2earth</td>
      <td>58</td>
      <td>1062558637040766976</td>
      <td>2018-11-14 04:11:35</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>@Rabbi_Jamarcus Nah, given all the scientific ...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062558503414444033</td>
      <td>2018-11-14 04:11:03</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>I made right wingers mad lol 😂 \nNever tell ri...</td>
      <td>Anon2earth</td>
      <td>125</td>
      <td>1062557800545554432</td>
      <td>2018-11-14 04:08:15</td>
      <td>Twitter for iPhone</td>
      <td>8</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>@TownesVanPlants Although with all of the “me”...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062557385586282496</td>
      <td>2018-11-14 04:06:36</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>@TownesVanPlants Probably not.</td>
      <td>Anon2earth</td>
      <td>30</td>
      <td>1062556886896128000</td>
      <td>2018-11-14 04:04:37</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>@Rabbi_Jamarcus True colors always come out. :...</td>
      <td>Anon2earth</td>
      <td>95</td>
      <td>1062556246530838528</td>
      <td>2018-11-14 04:02:05</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>@TownesVanPlants Probably not</td>
      <td>Anon2earth</td>
      <td>29</td>
      <td>1062555833794510848</td>
      <td>2018-11-14 04:00:26</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Well you probably suck at your job https://t.c...</td>
      <td>Anon2earth</td>
      <td>58</td>
      <td>1062555698503053313</td>
      <td>2018-11-14 03:59:54</td>
      <td>Twitter for iPhone</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>12</th>
      <td>@ScleroticNeuro1 @ConscienceColl If you say so...</td>
      <td>Anon2earth</td>
      <td>99</td>
      <td>1062555391618359296</td>
      <td>2018-11-14 03:58:41</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>@Rabbi_Jamarcus Oh, cool so you should know ex...</td>
      <td>Anon2earth</td>
      <td>85</td>
      <td>1062554834971303937</td>
      <td>2018-11-14 03:56:28</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>@XpektusAnon @thorharris666 @jaykelly26 Actual...</td>
      <td>Anon2earth</td>
      <td>139</td>
      <td>1062554678284767232</td>
      <td>2018-11-14 03:55:51</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>@V_Kershaw I could blow it up make it famous</td>
      <td>Anon2earth</td>
      <td>44</td>
      <td>1062553996655841280</td>
      <td>2018-11-14 03:53:08</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>@Rabbi_Jamarcus Ever take a sociology class?</td>
      <td>Anon2earth</td>
      <td>44</td>
      <td>1062553717914955777</td>
      <td>2018-11-14 03:52:02</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>@MyWhiteNinja_ I was gonna tell you “sounds li...</td>
      <td>Anon2earth</td>
      <td>76</td>
      <td>1062550430222290944</td>
      <td>2018-11-14 03:38:58</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>@BetterOffTeddy @V_Kershaw But I don’t want to...</td>
      <td>Anon2earth</td>
      <td>57</td>
      <td>1062549735465250816</td>
      <td>2018-11-14 03:36:12</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>@LaurenceAvent @JustSikko Yeah I’d love to kno...</td>
      <td>Anon2earth</td>
      <td>139</td>
      <td>1062549056499736578</td>
      <td>2018-11-14 03:33:30</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>@JustSikko @ChristopherA_E @TheQueerCrimer Sou...</td>
      <td>Anon2earth</td>
      <td>107</td>
      <td>1062548607377833984</td>
      <td>2018-11-14 03:31:43</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>RT @MillenGenY: Fuck Israel #MillenGenY #MGY #...</td>
      <td>Anon2earth</td>
      <td>98</td>
      <td>1062548024994512900</td>
      <td>2018-11-14 03:29:25</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>22</th>
      <td>@JustSikko @MyWhiteNinja_ IKR? Fuck that guy.\...</td>
      <td>Anon2earth</td>
      <td>81</td>
      <td>1062547791111688192</td>
      <td>2018-11-14 03:28:29</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>@thorharris666 @jaykelly26 If that is what “su...</td>
      <td>Anon2earth</td>
      <td>81</td>
      <td>1062547583254573056</td>
      <td>2018-11-14 03:27:39</td>
      <td>Twitter for iPhone</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>RT @thorharris666: Those two dudes think they ...</td>
      <td>Anon2earth</td>
      <td>125</td>
      <td>1062547408272400385</td>
      <td>2018-11-14 03:26:57</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>25</th>
      <td>@MyWhiteNinja_ @fembotswana I’m a bit high rig...</td>
      <td>Anon2earth</td>
      <td>123</td>
      <td>1062547042298445824</td>
      <td>2018-11-14 03:25:30</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>@MyWhiteNinja_ Omg today is 11/13...</td>
      <td>Anon2earth</td>
      <td>36</td>
      <td>1062546765436669952</td>
      <td>2018-11-14 03:24:24</td>
      <td>Twitter for iPhone</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>@XpektusAnon May you always find a light to gu...</td>
      <td>Anon2earth</td>
      <td>56</td>
      <td>1062546086538162178</td>
      <td>2018-11-14 03:21:42</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>@XpektusAnon Probably half and half.  I know t...</td>
      <td>Anon2earth</td>
      <td>108</td>
      <td>1062545763400593408</td>
      <td>2018-11-14 03:20:25</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>@DrNikkiMartinez @anonymousv33 Thanks ✊</td>
      <td>Anon2earth</td>
      <td>39</td>
      <td>1062510726575730690</td>
      <td>2018-11-14 01:01:12</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>170</th>
      <td>@fiondavision Does she even have a job at this...</td>
      <td>Anon2earth</td>
      <td>97</td>
      <td>1062152028154138624</td>
      <td>2018-11-13 01:15:51</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>171</th>
      <td>RT @HuffPost: RACE CALL: After a delay in ball...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062150958757634048</td>
      <td>2018-11-13 01:11:37</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>995</td>
    </tr>
    <tr>
      <th>172</th>
      <td>The GOP is going to be done for in two generat...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062150857519779840</td>
      <td>2018-11-13 01:11:12</td>
      <td>Twitter for iPhone</td>
      <td>5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>173</th>
      <td>RT @pacelattin: BREAKING: White House confirms...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062149493284659203</td>
      <td>2018-11-13 01:05:47</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>2981</td>
    </tr>
    <tr>
      <th>174</th>
      <td>@ProbablyBreakz Too bad the plates are edited ...</td>
      <td>Anon2earth</td>
      <td>80</td>
      <td>1062147896303345665</td>
      <td>2018-11-13 00:59:26</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>175</th>
      <td>@JustSikko Be happy! Now the racists don’t kno...</td>
      <td>Anon2earth</td>
      <td>95</td>
      <td>1062125584376127488</td>
      <td>2018-11-12 23:30:47</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>176</th>
      <td>This is what fascism looks like. https://t.co/...</td>
      <td>Anon2earth</td>
      <td>56</td>
      <td>1062125135870726145</td>
      <td>2018-11-12 23:29:00</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>177</th>
      <td>@ferenzi_w @LiamSherborn @JulyWolfe @YourAnonC...</td>
      <td>Anon2earth</td>
      <td>104</td>
      <td>1062124681610883074</td>
      <td>2018-11-12 23:27:12</td>
      <td>Twitter for iPhone</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>178</th>
      <td>@JacekMaghnat @JulyWolfe @YourAnonCentral Read...</td>
      <td>Anon2earth</td>
      <td>70</td>
      <td>1062115003862577153</td>
      <td>2018-11-12 22:48:44</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>179</th>
      <td>@JacekMaghnat @JulyWolfe @YourAnonCentral Prov...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062114282782031875</td>
      <td>2018-11-12 22:45:52</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>180</th>
      <td>https://t.co/whh86RORwB</td>
      <td>Anon2earth</td>
      <td>23</td>
      <td>1062108198361067520</td>
      <td>2018-11-12 22:21:42</td>
      <td>Twitter for iPhone</td>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>181</th>
      <td>@JacekMaghnat @JulyWolfe @YourAnonCentral It i...</td>
      <td>Anon2earth</td>
      <td>139</td>
      <td>1062107187231764481</td>
      <td>2018-11-12 22:17:41</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>182</th>
      <td>@EliteAthGear @shittyIifetips Can you guys mak...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062069311458758656</td>
      <td>2018-11-12 19:47:10</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>183</th>
      <td>@MyWhiteNinja_ *hugs* we are with you.</td>
      <td>Anon2earth</td>
      <td>38</td>
      <td>1062067987157524480</td>
      <td>2018-11-12 19:41:55</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>184</th>
      <td>@nierozumie @MisMiodojad @JulyWolfe @YourAnonC...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062066076761186304</td>
      <td>2018-11-12 19:34:19</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>185</th>
      <td>Hmmmm 🧐\nIt does have Ryan Reynolds tho... htt...</td>
      <td>Anon2earth</td>
      <td>65</td>
      <td>1062065013849030656</td>
      <td>2018-11-12 19:30:06</td>
      <td>Twitter Web Client</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>186</th>
      <td>@vandman777 yeah, there are a bunch of gamer r...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062063602239180801</td>
      <td>2018-11-12 19:24:29</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>187</th>
      <td>Rest in Peace Stan Lee.  Thank you for bringin...</td>
      <td>Anon2earth</td>
      <td>120</td>
      <td>1062062078100418562</td>
      <td>2018-11-12 19:18:26</td>
      <td>Twitter Web Client</td>
      <td>13</td>
      <td>4</td>
    </tr>
    <tr>
      <th>188</th>
      <td>@BREAKABLESEELEN @LiamSherborn @JulyWolfe @You...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062058048942981120</td>
      <td>2018-11-12 19:02:25</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>189</th>
      <td>@vandman777 here check out what Angry Joe says...</td>
      <td>Anon2earth</td>
      <td>101</td>
      <td>1062057038644154368</td>
      <td>2018-11-12 18:58:24</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>190</th>
      <td>@vandman777 All the people playing the beta sa...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062055939908485121</td>
      <td>2018-11-12 18:54:02</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>191</th>
      <td>@BREAKABLESEELEN @LiamSherborn @JulyWolfe @You...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062055103765643266</td>
      <td>2018-11-12 18:50:43</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Well, I was looking forward to Fallout '76 but...</td>
      <td>Anon2earth</td>
      <td>139</td>
      <td>1062054044955525120</td>
      <td>2018-11-12 18:46:30</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>193</th>
      <td>@BREAKABLESEELEN @LiamSherborn @JulyWolfe @You...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062053725584396288</td>
      <td>2018-11-12 18:45:14</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>194</th>
      <td>@sevatividam23 lol I'm not all that - I just s...</td>
      <td>Anon2earth</td>
      <td>139</td>
      <td>1062052720335564800</td>
      <td>2018-11-12 18:41:15</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>195</th>
      <td>@SingingBullets It's working here in Chicago</td>
      <td>Anon2earth</td>
      <td>44</td>
      <td>1062052105324765185</td>
      <td>2018-11-12 18:38:48</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>196</th>
      <td>@ljalawless True that! I hear you! Been search...</td>
      <td>Anon2earth</td>
      <td>129</td>
      <td>1062051750142717952</td>
      <td>2018-11-12 18:37:23</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>197</th>
      <td>Trump didn't create the economy you're living ...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062050556213108737</td>
      <td>2018-11-12 18:32:39</td>
      <td>Twitter Web Client</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>198</th>
      <td>@BREAKABLESEELEN @LiamSherborn @JulyWolfe @You...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062047973339729920</td>
      <td>2018-11-12 18:22:23</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>199</th>
      <td>@ljalawless Because they've been told to have ...</td>
      <td>Anon2earth</td>
      <td>140</td>
      <td>1062046973090508800</td>
      <td>2018-11-12 18:18:24</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>200 rows × 8 columns</p>
</div>


    The user name is:
    @AnonGhost07



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @Giantsps: #MMM2017\n#OpVendetta\nCyber war...</td>
      <td>AnonGhost07</td>
      <td>126</td>
      <td>926274296032387072</td>
      <td>2017-11-03 02:26:16</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>24</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @Giantsps: #OpCatalunya\nLive Hacking Spain...</td>
      <td>AnonGhost07</td>
      <td>140</td>
      <td>923871635626713088</td>
      <td>2017-10-27 11:18:57</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>128</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @Giantsps: #OpCatalunya\nSpain Your Anti Ma...</td>
      <td>AnonGhost07</td>
      <td>139</td>
      <td>923629765621420033</td>
      <td>2017-10-26 19:17:50</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @Giantsps: https://t.co/Uy8fVOzSOh\n@Scode4...</td>
      <td>AnonGhost07</td>
      <td>132</td>
      <td>911220295184269313</td>
      <td>2017-09-22 13:27:02</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Guys pls follow this Twitter\n@Scode404 @AnonG...</td>
      <td>AnonGhost07</td>
      <td>130</td>
      <td>909311275242364928</td>
      <td>2017-09-17 07:01:16</td>
      <td>Twitter Lite</td>
      <td>8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @Giantsps: 10 israeli govt Websites hacked ...</td>
      <td>AnonGhost07</td>
      <td>140</td>
      <td>909307306206052352</td>
      <td>2017-09-17 06:45:30</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>#Opisrae\nMorethan 15 Israel Database\nhttps:/...</td>
      <td>AnonGhost07</td>
      <td>140</td>
      <td>906574422231859200</td>
      <td>2017-09-09 17:46:00</td>
      <td>Twitter for Android</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Twitter not allowing to post\nSo screenshot\n@...</td>
      <td>AnonGhost07</td>
      <td>130</td>
      <td>906090267639517186</td>
      <td>2017-09-08 09:42:08</td>
      <td>Twitter for Android</td>
      <td>10</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>@JRBops Can u target Israel ?</td>
      <td>AnonGhost07</td>
      <td>29</td>
      <td>903977754248454148</td>
      <td>2017-09-02 13:47:46</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>@AnonGhostCJ @YourAnonRevolt @Hkurrafizimrit @...</td>
      <td>AnonGhost07</td>
      <td>67</td>
      <td>901453339107438593</td>
      <td>2017-08-26 14:36:38</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>@YourAnonRevolt @AnonGhostCJ @Hkurrafizimrit @...</td>
      <td>AnonGhost07</td>
      <td>75</td>
      <td>901448785615892481</td>
      <td>2017-08-26 14:18:33</td>
      <td>Twitter for Android</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Facebook Down \nLol\n@YourAnonRevolt @AnonGhos...</td>
      <td>AnonGhost07</td>
      <td>80</td>
      <td>901447045105557505</td>
      <td>2017-08-26 14:11:38</td>
      <td>Twitter for Android</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>12</th>
      <td>@Chingona_Rasta @Scode404 @YourAnonRevolt @Ano...</td>
      <td>AnonGhost07</td>
      <td>116</td>
      <td>899685166347558913</td>
      <td>2017-08-21 17:30:33</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>#OpIsrael\n4 Israel govt secret email conversa...</td>
      <td>AnonGhost07</td>
      <td>131</td>
      <td>898959227884064768</td>
      <td>2017-08-19 17:25:56</td>
      <td>Twitter for Android</td>
      <td>9</td>
      <td>4</td>
    </tr>
    <tr>
      <th>14</th>
      <td>@AntiGlobalist_ @NASA @cnni @AlJazeera_World @...</td>
      <td>AnonGhost07</td>
      <td>93</td>
      <td>898392442822512643</td>
      <td>2017-08-18 03:53:44</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>NASA password leaked \n@NASA  contact me NASA\...</td>
      <td>AnonGhost07</td>
      <td>135</td>
      <td>898273752387002368</td>
      <td>2017-08-17 20:02:06</td>
      <td>Twitter for Android</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>#OpTrump\nSecret email conversation leaked\nPl...</td>
      <td>AnonGhost07</td>
      <td>136</td>
      <td>898257558124642304</td>
      <td>2017-08-17 18:57:45</td>
      <td>Twitter for Android</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Chinis govt website\nHacked\nhttps://t.co/DlXb...</td>
      <td>AnonGhost07</td>
      <td>135</td>
      <td>898071767800889344</td>
      <td>2017-08-17 06:39:29</td>
      <td>Twitter for Android</td>
      <td>6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @OpOccupyIsrael: Israel we are coming for y...</td>
      <td>AnonGhost07</td>
      <td>113</td>
      <td>897073383807918080</td>
      <td>2017-08-14 12:32:16</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>19</th>
      <td>@ajplus Don't tell them to come India \nBecaus...</td>
      <td>AnonGhost07</td>
      <td>103</td>
      <td>896351551106670593</td>
      <td>2017-08-12 12:43:57</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>@S0u1_HLoTW @TVJanam</td>
      <td>AnonGhost07</td>
      <td>20</td>
      <td>896293784496910336</td>
      <td>2017-08-12 08:54:25</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>#OpIndia\nHacked 2 Indian banks \nDefaced and ...</td>
      <td>AnonGhost07</td>
      <td>127</td>
      <td>896292862270111744</td>
      <td>2017-08-12 08:50:45</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>22</th>
      <td>RT @S0u1_HLoTW: Tango Down with @AnonGhost07  ...</td>
      <td>AnonGhost07</td>
      <td>110</td>
      <td>896255248800432128</td>
      <td>2017-08-12 06:21:17</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>23</th>
      <td>@S0u1_HLoTW @breachalarm Dude need help for dd...</td>
      <td>AnonGhost07</td>
      <td>87</td>
      <td>896251202479939584</td>
      <td>2017-08-12 06:05:12</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>@GhostSquadHack Dude pls participate on #OpIndia</td>
      <td>AnonGhost07</td>
      <td>48</td>
      <td>896029514844692481</td>
      <td>2017-08-11 15:24:18</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>#OpIndia\nAnother Govt Website Hacked\nhttps:/...</td>
      <td>AnonGhost07</td>
      <td>116</td>
      <td>895907198496251904</td>
      <td>2017-08-11 07:18:15</td>
      <td>Twitter for Android</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <th>26</th>
      <td>#OpIndia\nIndian Government Website Hacked\nht...</td>
      <td>AnonGhost07</td>
      <td>118</td>
      <td>895866233169485824</td>
      <td>2017-08-11 04:35:29</td>
      <td>Twitter for Android</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <th>27</th>
      <td>@S0u1_HLoTW My post too just above\nName:#opin...</td>
      <td>AnonGhost07</td>
      <td>51</td>
      <td>895285617138868224</td>
      <td>2017-08-09 14:08:19</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>#opindia\nIndian University Database Leaked\nP...</td>
      <td>AnonGhost07</td>
      <td>133</td>
      <td>895285058679980032</td>
      <td>2017-08-09 14:06:06</td>
      <td>Twitter for Android</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>29</th>
      <td>#OpIndia post is on trending list \nThanks to ...</td>
      <td>AnonGhost07</td>
      <td>127</td>
      <td>895111542949126144</td>
      <td>2017-08-09 02:36:36</td>
      <td>Twitter for Android</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>44</th>
      <td>@AnonGhostCJ @Scode404 @YourAnonRevolt @XxCybe...</td>
      <td>AnonGhost07</td>
      <td>62</td>
      <td>892035123448733696</td>
      <td>2017-07-31 14:52:01</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>10 thousand israeli IP address Leaked\nPls sha...</td>
      <td>AnonGhost07</td>
      <td>131</td>
      <td>892034698704068608</td>
      <td>2017-07-31 14:50:20</td>
      <td>Twitter for Android</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Latest message\nhttps://t.co/1u1SUew7vB\n@Scod...</td>
      <td>AnonGhost07</td>
      <td>138</td>
      <td>889865416863924224</td>
      <td>2017-07-25 15:10:22</td>
      <td>Twitter for Android</td>
      <td>4</td>
      <td>3</td>
    </tr>
    <tr>
      <th>47</th>
      <td>@OpSaudi2017 That's why Saudi citizens don't w...</td>
      <td>AnonGhost07</td>
      <td>97</td>
      <td>889530790895271936</td>
      <td>2017-07-24 17:00:41</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>48</th>
      <td>@OpSaudi2017 But I wand opsaudi because of the...</td>
      <td>AnonGhost07</td>
      <td>139</td>
      <td>889530525894852609</td>
      <td>2017-07-24 16:59:38</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>49</th>
      <td>@OpSaudi2017 I'm totally disagree with you \nS...</td>
      <td>AnonGhost07</td>
      <td>90</td>
      <td>889530485197504512</td>
      <td>2017-07-24 16:59:29</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>50</th>
      <td>@OpSaudi2017 How can my team support you ?</td>
      <td>AnonGhost07</td>
      <td>42</td>
      <td>889529782223818753</td>
      <td>2017-07-24 16:56:41</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>51</th>
      <td>1 Million Israeli Emails Leaked\n#opisrael\nht...</td>
      <td>AnonGhost07</td>
      <td>65</td>
      <td>889000422173679616</td>
      <td>2017-07-23 05:53:12</td>
      <td>Twitter for Android</td>
      <td>11</td>
      <td>7</td>
    </tr>
    <tr>
      <th>52</th>
      <td>#opisrael\n@Scode404 @XxCyberMentorxX @AnonGho...</td>
      <td>AnonGhost07</td>
      <td>139</td>
      <td>888986972433272832</td>
      <td>2017-07-23 04:59:45</td>
      <td>Twitter for Android</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>53</th>
      <td>@crashd4rkh4cker U r a theif</td>
      <td>AnonGhost07</td>
      <td>28</td>
      <td>888941375844081664</td>
      <td>2017-07-23 01:58:34</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>54</th>
      <td>@crashd4rkh4cker Hey it's leaked by me :-/ htt...</td>
      <td>AnonGhost07</td>
      <td>66</td>
      <td>888941346098102274</td>
      <td>2017-07-23 01:58:27</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>55</th>
      <td>#opisrael\n1800 users IP address leaked \nhttp...</td>
      <td>AnonGhost07</td>
      <td>131</td>
      <td>888756224727986177</td>
      <td>2017-07-22 13:42:50</td>
      <td>Twitter for Android</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Bangladesh Police Database Leaked\nhttps://t.c...</td>
      <td>AnonGhost07</td>
      <td>138</td>
      <td>888268246142431232</td>
      <td>2017-07-21 05:23:47</td>
      <td>Twitter for Android</td>
      <td>6</td>
      <td>2</td>
    </tr>
    <tr>
      <th>57</th>
      <td>@AnonGhostWorld Follow me dude</td>
      <td>AnonGhost07</td>
      <td>30</td>
      <td>887400366903447553</td>
      <td>2017-07-18 19:55:09</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>58</th>
      <td>UN failed to protect Palestine\nhttps://t.co/L...</td>
      <td>AnonGhost07</td>
      <td>54</td>
      <td>886598961598930944</td>
      <td>2017-07-16 14:50:39</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>59</th>
      <td>We got all UN employee list with Salary,phone,...</td>
      <td>AnonGhost07</td>
      <td>133</td>
      <td>886581458143371264</td>
      <td>2017-07-16 13:41:06</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>60</th>
      <td>@Scode404 @YourAnonRevolt @Hkurrafizimrit @Ano...</td>
      <td>AnonGhost07</td>
      <td>135</td>
      <td>886498444080103424</td>
      <td>2017-07-16 08:11:14</td>
      <td>Twitter for Android</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>61</th>
      <td>@Scode404 @YourAnonRevolt @Hkurrafizimrit http...</td>
      <td>AnonGhost07</td>
      <td>65</td>
      <td>886498136188796929</td>
      <td>2017-07-16 08:10:00</td>
      <td>Twitter for Android</td>
      <td>7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Some of "UN" important Data's Published on Pas...</td>
      <td>AnonGhost07</td>
      <td>131</td>
      <td>886498056526483457</td>
      <td>2017-07-16 08:09:41</td>
      <td>Twitter for Android</td>
      <td>8</td>
      <td>6</td>
    </tr>
    <tr>
      <th>63</th>
      <td>@AnonSabiduriA @AnonymousVene10 @Gato_Vzla Pls...</td>
      <td>AnonGhost07</td>
      <td>61</td>
      <td>886269266898468864</td>
      <td>2017-07-15 17:00:33</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>64</th>
      <td>@YourAnonRevolt @rul3r Follow me back dude\nNe...</td>
      <td>AnonGhost07</td>
      <td>57</td>
      <td>886269032155852800</td>
      <td>2017-07-15 16:59:38</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>65</th>
      <td>@XxCyberMentorxX Add minion Ghost on ur follows</td>
      <td>AnonGhost07</td>
      <td>47</td>
      <td>886263593615761408</td>
      <td>2017-07-15 16:38:01</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>66</th>
      <td>@XxCyberMentorxX Hahaha nice dude</td>
      <td>AnonGhost07</td>
      <td>33</td>
      <td>886263519175475201</td>
      <td>2017-07-15 16:37:43</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>67</th>
      <td>RT @XxCyberMentorxX: #opisrael2017 #FreePalest...</td>
      <td>AnonGhost07</td>
      <td>84</td>
      <td>886258564892774401</td>
      <td>2017-07-15 16:18:02</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>68</th>
      <td>We got United Nations Database control\nImport...</td>
      <td>AnonGhost07</td>
      <td>132</td>
      <td>886225360366346244</td>
      <td>2017-07-15 14:06:05</td>
      <td>Twitter for Android</td>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>69</th>
      <td>@AnonGhostCJ @Scode404 Bro I need some followe...</td>
      <td>AnonGhost07</td>
      <td>82</td>
      <td>886223298098454529</td>
      <td>2017-07-15 13:57:54</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>70</th>
      <td>We got lot of UN important data's\nWe will pub...</td>
      <td>AnonGhost07</td>
      <td>136</td>
      <td>886222631619276800</td>
      <td>2017-07-15 13:55:15</td>
      <td>Twitter for Android</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>71</th>
      <td>UN website hacked !\n#AnonymousGhost\n#MinionG...</td>
      <td>AnonGhost07</td>
      <td>133</td>
      <td>886114643252396033</td>
      <td>2017-07-15 06:46:08</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>72</th>
      <td>Korean govt removed our Pastebin\nKorea,you ha...</td>
      <td>AnonGhost07</td>
      <td>133</td>
      <td>885927679890825216</td>
      <td>2017-07-14 18:23:13</td>
      <td>Twitter for Android</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Greeting citizens of the world \nFinally we ar...</td>
      <td>AnonGhost07</td>
      <td>87</td>
      <td>885799194409218050</td>
      <td>2017-07-14 09:52:39</td>
      <td>Twitter for Android</td>
      <td>8</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>74 rows × 8 columns</p>
</div>


    The user name is:
    @anonghost387



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @ADnl: Tienduizenden jongeren weten precies...</td>
      <td>anonghost387</td>
      <td>114</td>
      <td>1062241320939515904</td>
      <td>2018-11-13 07:10:41</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @GhostSecGroup: Islamic State central propa...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1061262745507631104</td>
      <td>2018-11-10 14:22:10</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @delimburger: Een 40-jarige medewerker van ...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1057953923443687425</td>
      <td>2018-11-01 11:14:05</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @RTLnieuws: Politiemedewerker Limburg verda...</td>
      <td>anonghost387</td>
      <td>118</td>
      <td>1057952124103995393</td>
      <td>2018-11-01 11:06:56</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @intel_ghost: In every country, there is a ...</td>
      <td>anonghost387</td>
      <td>109</td>
      <td>1057895049504141312</td>
      <td>2018-11-01 07:20:09</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @awakenppl: Gab has spent the past 48 hours...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1057717131549306880</td>
      <td>2018-10-31 19:33:10</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>#opisis #hacked #defaced #ghostsecuritygroup #...</td>
      <td>anonghost387</td>
      <td>132</td>
      <td>1057712324012920832</td>
      <td>2018-10-31 19:14:04</td>
      <td>Twitter for Android</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RT @GhostSecGroup: Islamic State we never pose...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1057505679127273472</td>
      <td>2018-10-31 05:32:56</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @TheHackersNews: Hacker releases zero-day e...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1055138389387821056</td>
      <td>2018-10-24 16:46:10</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>340</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RT @twtbtat1: ask yourself why our goverment w...</td>
      <td>anonghost387</td>
      <td>88</td>
      <td>1054812181013979136</td>
      <td>2018-10-23 19:09:56</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RT @twtbtat1: Arrest one of us; two more will ...</td>
      <td>anonghost387</td>
      <td>72</td>
      <td>1054452248992985088</td>
      <td>2018-10-22 19:19:41</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>20</td>
    </tr>
    <tr>
      <th>11</th>
      <td>RT @x0rz: |￣￣￣￣￣￣￣￣￣￣￣|\n                Hacke...</td>
      <td>anonghost387</td>
      <td>139</td>
      <td>1054452045326045184</td>
      <td>2018-10-22 19:18:53</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>459</td>
    </tr>
    <tr>
      <th>12</th>
      <td>RT @YourAnonNews: 150,000 people demonstrated ...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1051167618789453825</td>
      <td>2018-10-13 17:47:44</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>79</td>
    </tr>
    <tr>
      <th>13</th>
      <td>RT @h0nk3r: #Anonymous,\nFor a Million Mask Ma...</td>
      <td>anonghost387</td>
      <td>138</td>
      <td>1051167343336931328</td>
      <td>2018-10-13 17:46:39</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>60</td>
    </tr>
    <tr>
      <th>14</th>
      <td>RT @For2000years: @twtbtat1 Here's another kid...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1050780697420554240</td>
      <td>2018-10-12 16:10:15</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>15</th>
      <td>RT @twtbtat1: Cops Who Tasered Handcuffed Teen...</td>
      <td>anonghost387</td>
      <td>127</td>
      <td>1050780636892581888</td>
      <td>2018-10-12 16:10:01</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>40</td>
    </tr>
    <tr>
      <th>16</th>
      <td>RT @Nieuwsuur: In zware zaken, waarbij wel ver...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1050485129964122118</td>
      <td>2018-10-11 20:35:46</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>17</th>
      <td>@Nieuwsuur Eindelijk echt nieuws, ook wel gang...</td>
      <td>anonghost387</td>
      <td>138</td>
      <td>1050412753033289729</td>
      <td>2018-10-11 15:48:10</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @Nieuwsuur: De Nederlandse #politie de poli...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1050412355555872768</td>
      <td>2018-10-11 15:46:35</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>19</th>
      <td>RT @AnonPurity: John Kiriakou blew the whistle...</td>
      <td>anonghost387</td>
      <td>139</td>
      <td>1049915384604880896</td>
      <td>2018-10-10 06:51:48</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>20</th>
      <td>RT @RTLnieuws: Criminelen achter porno-afpersi...</td>
      <td>anonghost387</td>
      <td>125</td>
      <td>1049683118523867136</td>
      <td>2018-10-09 15:28:52</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>21</th>
      <td>RT @YourAnonCentral: Victoria Marinova, a jour...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1049236809820135424</td>
      <td>2018-10-08 09:55:23</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>126</td>
    </tr>
    <tr>
      <th>22</th>
      <td>RT @OccuWorld: Bulgarian Journalist Investigat...</td>
      <td>anonghost387</td>
      <td>102</td>
      <td>1049236606232776704</td>
      <td>2018-10-08 09:54:35</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>23</th>
      <td>RT @SkyNewsBreak: Authorities in China say for...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1049191166627995648</td>
      <td>2018-10-08 06:54:01</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>24</th>
      <td>RT @HAghosta: Yes, You Should Still Change You...</td>
      <td>anonghost387</td>
      <td>117</td>
      <td>1049190787148333058</td>
      <td>2018-10-08 06:52:31</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>RT @AnonymousTdot: Hacktivists should be celeb...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1048582820480917504</td>
      <td>2018-10-06 14:36:40</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>41</td>
    </tr>
    <tr>
      <th>26</th>
      <td>RT @PalestinePR: Out of control Israel continu...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>1048255521382457345</td>
      <td>2018-10-05 16:56:06</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>467</td>
    </tr>
    <tr>
      <th>27</th>
      <td>RT @OpOccupyIsrael: Israel we are coming for y...</td>
      <td>anonghost387</td>
      <td>113</td>
      <td>1048255075590832128</td>
      <td>2018-10-05 16:54:20</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>28</th>
      <td>RT @RT_com: Police brutality in Germany caught...</td>
      <td>anonghost387</td>
      <td>91</td>
      <td>1048254914919649280</td>
      <td>2018-10-05 16:53:42</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>161</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RT @HAghosta: #OpSafeWinter\nReach out, help a...</td>
      <td>anonghost387</td>
      <td>104</td>
      <td>1048254258880163841</td>
      <td>2018-10-05 16:51:05</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>13</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>164</th>
      <td>Just the one new follower today found welcome ...</td>
      <td>anonghost387</td>
      <td>80</td>
      <td>977175841900605440</td>
      <td>2018-03-23 13:30:50</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>165</th>
      <td>RT @YourAnonNews: Yesterday, The Netherlands v...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>976816010044100608</td>
      <td>2018-03-22 13:40:59</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>49</td>
    </tr>
    <tr>
      <th>166</th>
      <td>Just the one new follower today found welcome ...</td>
      <td>anonghost387</td>
      <td>80</td>
      <td>976813417016119297</td>
      <td>2018-03-22 13:30:41</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>167</th>
      <td>Just the one new follower today found welcome ...</td>
      <td>anonghost387</td>
      <td>80</td>
      <td>976451073652137984</td>
      <td>2018-03-21 13:30:52</td>
      <td>UnFollowSpy</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>168</th>
      <td>2 people followed me today tracked by https://...</td>
      <td>anonghost387</td>
      <td>61</td>
      <td>975728002569945088</td>
      <td>2018-03-19 13:37:38</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Welcome to my new 1 followers and goodbye to 1...</td>
      <td>anonghost387</td>
      <td>98</td>
      <td>975365463285755905</td>
      <td>2018-03-18 13:37:02</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>170</th>
      <td>2 people unfollowed me today tracked by https:...</td>
      <td>anonghost387</td>
      <td>63</td>
      <td>975003013667434496</td>
      <td>2018-03-17 13:36:47</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Just the one unfollower today found tracked by...</td>
      <td>anonghost387</td>
      <td>70</td>
      <td>974640790772137985</td>
      <td>2018-03-16 13:37:27</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>172</th>
      <td>3 people unfollowed me today tracked by https:...</td>
      <td>anonghost387</td>
      <td>63</td>
      <td>973916357170360321</td>
      <td>2018-03-14 13:38:48</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>173</th>
      <td>2 people followed me today tracked by https://...</td>
      <td>anonghost387</td>
      <td>61</td>
      <td>973553641100525573</td>
      <td>2018-03-13 13:37:30</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>174</th>
      <td>Just the one new follower today found welcome ...</td>
      <td>anonghost387</td>
      <td>80</td>
      <td>972832537490817024</td>
      <td>2018-03-11 13:52:05</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>175</th>
      <td>2 people unfollowed me today tracked by https:...</td>
      <td>anonghost387</td>
      <td>63</td>
      <td>972470341913079808</td>
      <td>2018-03-10 13:52:51</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>176</th>
      <td>Gained 2 followers and lost 1 (stats by https:...</td>
      <td>anonghost387</td>
      <td>64</td>
      <td>972107458771496960</td>
      <td>2018-03-09 13:50:53</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>177</th>
      <td>2 people followed me today tracked by https://...</td>
      <td>anonghost387</td>
      <td>61</td>
      <td>971745314683039744</td>
      <td>2018-03-08 13:51:51</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>178</th>
      <td>RT @torproject: If your org’s site is blocked ...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>971427322321489920</td>
      <td>2018-03-07 16:48:16</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>70</td>
    </tr>
    <tr>
      <th>179</th>
      <td>RT @KimDotcom: What the Deep State means when ...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>971427214154592256</td>
      <td>2018-03-07 16:47:50</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>4302</td>
    </tr>
    <tr>
      <th>180</th>
      <td>RT @_DeleteTheElite: Tell me again how Faceboo...</td>
      <td>anonghost387</td>
      <td>132</td>
      <td>971426467023171584</td>
      <td>2018-03-07 16:44:52</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>181</th>
      <td>RT @gumby_24: https://t.co/hYlvCjNi3A</td>
      <td>anonghost387</td>
      <td>37</td>
      <td>971426432537669632</td>
      <td>2018-03-07 16:44:44</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>182</th>
      <td>RT @dzukirukoremoga: Leaked NSA Dump Also Cont...</td>
      <td>anonghost387</td>
      <td>140</td>
      <td>971426213867606016</td>
      <td>2018-03-07 16:43:52</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>183</th>
      <td>RT @xposefacts: What could possibly go wrong? ...</td>
      <td>anonghost387</td>
      <td>152</td>
      <td>971426146767089664</td>
      <td>2018-03-07 16:43:36</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>184</th>
      <td>RT @ReggaeMarleyBob: "Some will hate you, pret...</td>
      <td>anonghost387</td>
      <td>107</td>
      <td>971425946740785154</td>
      <td>2018-03-07 16:42:48</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>57</td>
    </tr>
    <tr>
      <th>185</th>
      <td>RT @Anonymous4571: https://t.co/awMsDNMYMM</td>
      <td>anonghost387</td>
      <td>42</td>
      <td>971425812497842176</td>
      <td>2018-03-07 16:42:16</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>21</td>
    </tr>
    <tr>
      <th>186</th>
      <td>RT @Anon6_NvrForget: We all make mistakes. We ...</td>
      <td>anonghost387</td>
      <td>143</td>
      <td>971425309068181504</td>
      <td>2018-03-07 16:40:16</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>187</th>
      <td>@PolitieHeerlen @POL_Michiels Corrupte agenten</td>
      <td>anonghost387</td>
      <td>46</td>
      <td>971424605180014592</td>
      <td>2018-03-07 16:37:28</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>188</th>
      <td>RT @NOS: Belastingdienst getroffen door DDos-a...</td>
      <td>anonghost387</td>
      <td>75</td>
      <td>971423752620617728</td>
      <td>2018-03-07 16:34:05</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>22</td>
    </tr>
    <tr>
      <th>189</th>
      <td>@NOS We are Anonymous.</td>
      <td>anonghost387</td>
      <td>22</td>
      <td>971423630067314688</td>
      <td>2018-03-07 16:33:36</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>190</th>
      <td>2 people followed me today tracked by https://...</td>
      <td>anonghost387</td>
      <td>61</td>
      <td>971382706964652032</td>
      <td>2018-03-07 13:50:59</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>191</th>
      <td>2 people followed me today tracked by https://...</td>
      <td>anonghost387</td>
      <td>61</td>
      <td>971020255559524352</td>
      <td>2018-03-06 13:50:44</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Gained 1 followers and lost 1 (stats by https:...</td>
      <td>anonghost387</td>
      <td>64</td>
      <td>970659189465886722</td>
      <td>2018-03-05 13:55:59</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>193</th>
      <td>1 Followed, 1 Unfollowed me (monitored by http...</td>
      <td>anonghost387</td>
      <td>66</td>
      <td>970296671153225729</td>
      <td>2018-03-04 13:55:28</td>
      <td>UnFollowSpy</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>194 rows × 8 columns</p>
</div>


    The user name is:
    @AnonGhostCJ



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @AliAdnanj: https://t.co/V45jjBzIy8\n\nHACK...</td>
      <td>AnonGhostCJ</td>
      <td>128</td>
      <td>1060372892062441472</td>
      <td>2018-11-08 03:26:12</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @delfinalek: Hacked by Polish Attacker http...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>1059358485417095169</td>
      <td>2018-11-05 08:15:19</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>#Anonghost\n#Anonymous\n#facebook\n#FB\n\nHack...</td>
      <td>AnonGhostCJ</td>
      <td>86</td>
      <td>1043212543911636992</td>
      <td>2018-09-21 18:57:07</td>
      <td>Twitter for iPhone</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @delfinalek: facebook account hacked by me ...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>1041945331921866752</td>
      <td>2018-09-18 07:01:40</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @S0u1_HLoTW: S0u1 Joined Cyber Genocide \n#...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>1036781575973658626</td>
      <td>2018-09-04 01:02:44</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>#AnonGhost\n#Palestine\n\n@Scode404 \n@Hkurraf...</td>
      <td>AnonGhostCJ</td>
      <td>106</td>
      <td>991298911707873281</td>
      <td>2018-05-01 12:50:52</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>6</th>
      <td>#AnonGhost\n#Anonymous\n\n@Hkurrafizimrit \n@A...</td>
      <td>AnonGhostCJ</td>
      <td>137</td>
      <td>989127507881295872</td>
      <td>2018-04-25 13:02:29</td>
      <td>Twitter for iPhone</td>
      <td>8</td>
      <td>8</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RT @delfinalek: My Facebook https://t.co/WSwn5...</td>
      <td>AnonGhostCJ</td>
      <td>139</td>
      <td>985310613269241856</td>
      <td>2018-04-15 00:15:31</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @f1004x: #Anonymous – #OpIsrael2018 (7 Apri...</td>
      <td>AnonGhostCJ</td>
      <td>75</td>
      <td>982821805265043456</td>
      <td>2018-04-08 03:25:52</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RT @PuckArks: #OpIsrael #OpIsrael2018 #Anonymo...</td>
      <td>AnonGhostCJ</td>
      <td>72</td>
      <td>982821604102033408</td>
      <td>2018-04-08 03:25:05</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RT @YourAnonNews: #Anonymous #OpIsrael2018 \n2...</td>
      <td>AnonGhostCJ</td>
      <td>87</td>
      <td>982821395901005837</td>
      <td>2018-04-08 03:24:15</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>11</th>
      <td>RT @TrollForums: #Anonymous #OpIsrael2018 \n20...</td>
      <td>AnonGhostCJ</td>
      <td>86</td>
      <td>982821220440666113</td>
      <td>2018-04-08 03:23:33</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>12</th>
      <td>#AnonGhost\n#Anonymous\n#OpIsrael\n#OpIsrael20...</td>
      <td>AnonGhostCJ</td>
      <td>94</td>
      <td>982815216214786048</td>
      <td>2018-04-08 02:59:42</td>
      <td>Twitter for Android</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>13</th>
      <td>RT @LulzLabas: #Anonymous Operation #OpSpain \...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>968656724474281985</td>
      <td>2018-02-28 01:18:54</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>33</td>
    </tr>
    <tr>
      <th>14</th>
      <td>RT @AnonsGR: #Anonymous #OpSpain #FreeXej #Shu...</td>
      <td>AnonGhostCJ</td>
      <td>133</td>
      <td>968656550607781888</td>
      <td>2018-02-28 01:18:13</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>49</td>
    </tr>
    <tr>
      <th>15</th>
      <td>RT @UnitedSecAnon: #OpSpain #Spain our targets...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>968656466046431232</td>
      <td>2018-02-28 01:17:52</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>16</th>
      <td>RT @palestine_bs: A little Palestinian girl wi...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>965125699244707843</td>
      <td>2018-02-18 07:27:52</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>52</td>
    </tr>
    <tr>
      <th>17</th>
      <td>RT @Giantsps: #Opisrael2018\nComing soon\nJust...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>960532603261497344</td>
      <td>2018-02-05 15:16:33</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @Giantsps: #Opisrael2018\n#Opisrael\nPls sh...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>960532474278305793</td>
      <td>2018-02-05 15:16:02</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>19</th>
      <td>RT @GhostPalestine: 🇵🇸 #AnonGhostPalestine 🇵🇸\...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>960532368237903872</td>
      <td>2018-02-05 15:15:37</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>20</th>
      <td>RT @tristanclaimed: @NASA\n@FBI\n@CIA\n@AnonGh...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>960498500139024384</td>
      <td>2018-02-05 13:01:02</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>21</th>
      <td>RT @tristanclaimed: @AnonGhostCJ \n@LulzsecZom...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>960275151559450625</td>
      <td>2018-02-04 22:13:31</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>22</th>
      <td>#AnonGhost\n#MinionGhost\n#Anonymous\n#Anonymo...</td>
      <td>AnonGhostCJ</td>
      <td>105</td>
      <td>960110192707817472</td>
      <td>2018-02-04 11:18:02</td>
      <td>Twitter Web Client</td>
      <td>7</td>
      <td>5</td>
    </tr>
    <tr>
      <th>23</th>
      <td>#AnonGhost\n#MinionGhost\n#Anonymous\n#Anonymo...</td>
      <td>AnonGhostCJ</td>
      <td>105</td>
      <td>960110035098464258</td>
      <td>2018-02-04 11:17:25</td>
      <td>Twitter Web Client</td>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>24</th>
      <td>RT @evadyugemos: #KnowledgeIsFree\nhttps://t.c...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>959758379739594752</td>
      <td>2018-02-03 12:00:03</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>#AnonGhost\n#MinionGhost\n#Anonymous\n#Anonymo...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>959754002450628608</td>
      <td>2018-02-03 11:42:40</td>
      <td>Twitter Web Client</td>
      <td>18</td>
      <td>11</td>
    </tr>
    <tr>
      <th>26</th>
      <td>RT @gelgoog99: 信金中央金庫 OpIcarusに狙われた？\nhttps://...</td>
      <td>AnonGhostCJ</td>
      <td>74</td>
      <td>959701454113071105</td>
      <td>2018-02-03 08:13:51</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>27</th>
      <td>RT @GhostPalestine: 🇵🇸 #AnonGhostPalestine 🇵🇸\...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>958505218647339009</td>
      <td>2018-01-31 01:00:26</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>28</th>
      <td>RT @Ammo81165266: https://t.co/4093FLP4fL</td>
      <td>AnonGhostCJ</td>
      <td>41</td>
      <td>958505107091472385</td>
      <td>2018-01-31 01:00:00</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RT @MrAstraX: #Anonymous\n#AnonymousNetwork \n...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>958504935603150849</td>
      <td>2018-01-31 00:59:19</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>156</th>
      <td>RT @FoxNews: BREAKING NEWS: Trump told Russian...</td>
      <td>AnonGhostCJ</td>
      <td>127</td>
      <td>865656146488475649</td>
      <td>2017-05-19 19:51:23</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>228</td>
    </tr>
    <tr>
      <th>157</th>
      <td>RT @Anonycast: https://t.co/wQmovGtn9H</td>
      <td>AnonGhostCJ</td>
      <td>38</td>
      <td>865622318671773697</td>
      <td>2017-05-19 17:36:57</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>158</th>
      <td>@LulzSecRB When?</td>
      <td>AnonGhostCJ</td>
      <td>16</td>
      <td>865598811858542593</td>
      <td>2017-05-19 16:03:33</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>159</th>
      <td>RT @FoxNews: .@JudgeJeanine: Trump Must Unders...</td>
      <td>AnonGhostCJ</td>
      <td>106</td>
      <td>865597467860926465</td>
      <td>2017-05-19 15:58:13</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>426</td>
    </tr>
    <tr>
      <th>160</th>
      <td>#Wannacry #Ransomeware \nhttps://t.co/iwh8vuS3XB</td>
      <td>AnonGhostCJ</td>
      <td>47</td>
      <td>865581074679607296</td>
      <td>2017-05-19 14:53:04</td>
      <td>Linkis: turn sharing into growth</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>161</th>
      <td>It is a file without Ransomware #Wannacry #Ran...</td>
      <td>AnonGhostCJ</td>
      <td>85</td>
      <td>865579393820704769</td>
      <td>2017-05-19 14:46:23</td>
      <td>Linkis: turn sharing into growth</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>162</th>
      <td>RT @ABC: Pres. Trump: "We need a great Directo...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>865307305058779136</td>
      <td>2017-05-18 20:45:12</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>121</td>
    </tr>
    <tr>
      <th>163</th>
      <td>RT @Anons_Worldwide: You only live once, but i...</td>
      <td>AnonGhostCJ</td>
      <td>103</td>
      <td>864976660218523648</td>
      <td>2017-05-17 22:51:20</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>164</th>
      <td>Awake #Anonymous https://t.co/W5M5ZnrjOa</td>
      <td>AnonGhostCJ</td>
      <td>40</td>
      <td>864976319863336960</td>
      <td>2017-05-17 22:49:59</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>165</th>
      <td>WTF https://t.co/nMxPjVutIe</td>
      <td>AnonGhostCJ</td>
      <td>27</td>
      <td>864975700192776192</td>
      <td>2017-05-17 22:47:32</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>166</th>
      <td>RT @Anons_Worldwide: It don't take much to see...</td>
      <td>AnonGhostCJ</td>
      <td>139</td>
      <td>864975343697797120</td>
      <td>2017-05-17 22:46:07</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>36</td>
    </tr>
    <tr>
      <th>167</th>
      <td>SQL Injection\n#Anonymous #Hacker # Hack #SQL ...</td>
      <td>AnonGhostCJ</td>
      <td>79</td>
      <td>864974393641844737</td>
      <td>2017-05-17 22:42:20</td>
      <td>Twitter for Android</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>168</th>
      <td>RT @GroupAnon: ISIS Twitter account has been h...</td>
      <td>AnonGhostCJ</td>
      <td>114</td>
      <td>864971998983737344</td>
      <td>2017-05-17 22:32:49</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>340</td>
    </tr>
    <tr>
      <th>169</th>
      <td>RT @PeterAlexander: NEW: Pres Trump spoke with...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>864852010121576448</td>
      <td>2017-05-17 14:36:02</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>338</td>
    </tr>
    <tr>
      <th>170</th>
      <td>RT @TheHackersNews: Researchers discover code ...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>864847710750351361</td>
      <td>2017-05-17 14:18:56</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>295</td>
    </tr>
    <tr>
      <th>171</th>
      <td>RT @TheHackersNews: BAD NEWS. The Shadow Broke...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>864847685844475904</td>
      <td>2017-05-17 14:18:51</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>427</td>
    </tr>
    <tr>
      <th>172</th>
      <td>RT @TheHackersNews: #Microsoft releases Visual...</td>
      <td>AnonGhostCJ</td>
      <td>88</td>
      <td>864846211861274625</td>
      <td>2017-05-17 14:12:59</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>64</td>
    </tr>
    <tr>
      <th>173</th>
      <td>@Anon_Ryd3r If so, always share and retwit inf...</td>
      <td>AnonGhostCJ</td>
      <td>90</td>
      <td>864831661921386496</td>
      <td>2017-05-17 13:15:10</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>174</th>
      <td>RT @TheHackersNews: Take a lesson from Microso...</td>
      <td>AnonGhostCJ</td>
      <td>139</td>
      <td>864828932780040192</td>
      <td>2017-05-17 13:04:19</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>78</td>
    </tr>
    <tr>
      <th>175</th>
      <td>RT @ABC: NEW: Chelsea Manning has left Fort Le...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>864828817956683777</td>
      <td>2017-05-17 13:03:52</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>168</td>
    </tr>
    <tr>
      <th>176</th>
      <td>Is this really going on yet?\n\nhttps://t.co/L...</td>
      <td>AnonGhostCJ</td>
      <td>98</td>
      <td>864827809235050496</td>
      <td>2017-05-17 12:59:52</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>177</th>
      <td>Is this operation still in progress?\n\nhttps:...</td>
      <td>AnonGhostCJ</td>
      <td>106</td>
      <td>864826605872467968</td>
      <td>2017-05-17 12:55:05</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>178</th>
      <td>RT @Anons_Worldwide: We are not scared https:/...</td>
      <td>AnonGhostCJ</td>
      <td>62</td>
      <td>864825902441480196</td>
      <td>2017-05-17 12:52:17</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>29</td>
    </tr>
    <tr>
      <th>179</th>
      <td>https://t.co/70Z3rkmTZR\n\nHow to hide our ip ...</td>
      <td>AnonGhostCJ</td>
      <td>72</td>
      <td>864825599512068097</td>
      <td>2017-05-17 12:51:05</td>
      <td>Twitter for Android</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>180</th>
      <td>Play! and Enjoy the life https://t.co/jwqB8A8XqK</td>
      <td>AnonGhostCJ</td>
      <td>48</td>
      <td>864824617684172800</td>
      <td>2017-05-17 12:47:11</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>181</th>
      <td>🌍🌎🌏🌐 https://t.co/ZSliQKjKL7</td>
      <td>AnonGhostCJ</td>
      <td>28</td>
      <td>864823704521293825</td>
      <td>2017-05-17 12:43:33</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>182</th>
      <td>We are looking for hackers, reporters, program...</td>
      <td>AnonGhostCJ</td>
      <td>139</td>
      <td>864822592288378880</td>
      <td>2017-05-17 12:39:08</td>
      <td>Twitter for Android</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>183</th>
      <td>Yeah.... https://t.co/74KKfpof8y</td>
      <td>AnonGhostCJ</td>
      <td>32</td>
      <td>864819305082376192</td>
      <td>2017-05-17 12:26:04</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>184</th>
      <td>RT @FoxNews: BREAKING: Russian President Vladi...</td>
      <td>AnonGhostCJ</td>
      <td>140</td>
      <td>864819141584146432</td>
      <td>2017-05-17 12:25:25</td>
      <td>Twitter for Android</td>
      <td>0</td>
      <td>2288</td>
    </tr>
    <tr>
      <th>185</th>
      <td>I didn't like Trump from the start.  \n\nhttps...</td>
      <td>AnonGhostCJ</td>
      <td>106</td>
      <td>864770166537584640</td>
      <td>2017-05-17 09:10:49</td>
      <td>Twitter for iPhone</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>186 rows × 8 columns</p>
</div>


    The user name is:
    @AnonRisingIRC



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>RT @judithpoe: #SerenaShim Thank for you conti...</td>
      <td>AnonRisingIRC</td>
      <td>139</td>
      <td>805410265802149892</td>
      <td>2016-12-04 13:55:45</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @OpGabon: #Gabon People cannot accept legis...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>802141926153658368</td>
      <td>2016-11-25 13:28:32</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @securityaffairs: How the Mirai botnet hack...</td>
      <td>AnonRisingIRC</td>
      <td>139</td>
      <td>800723094763663360</td>
      <td>2016-11-21 15:30:37</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>RT @OpGabon: «#Washington is filled with publi...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>792462567683854336</td>
      <td>2016-10-29 20:26:14</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>RT @OpGabon: @apiper848 Hi, whatever you can d...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>792462426507735040</td>
      <td>2016-10-29 20:25:40</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @judithpoe: @exiled_anon @4n0nc47 @4N0NC475...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>792462345050157060</td>
      <td>2016-10-29 20:25:20</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>RT @OpGabon: #OpGabon: Call for action to #Ano...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>791633067408551936</td>
      <td>2016-10-27 13:30:05</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>7</th>
      <td>RT @OpGabon: #OpGabon featured on @AnonIntelGr...</td>
      <td>AnonRisingIRC</td>
      <td>129</td>
      <td>791633037121421312</td>
      <td>2016-10-27 13:29:58</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @OpGabon: #Gabon-Crise postélectorale : «#A...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>791633019144638464</td>
      <td>2016-10-27 13:29:54</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>9</th>
      <td>RT @h0t_p0ppy: #SeaWorld =\nPsychology corrupt...</td>
      <td>AnonRisingIRC</td>
      <td>107</td>
      <td>791190588163026944</td>
      <td>2016-10-26 08:11:50</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RT @AnonData_: Join @AnonRisingSec IRC \nhttps...</td>
      <td>AnonRisingIRC</td>
      <td>86</td>
      <td>791190226140028928</td>
      <td>2016-10-26 08:10:24</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>11</th>
      <td>RT @strangegarden7: Read Latest Anonymous News...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>790558478188380160</td>
      <td>2016-10-24 14:20:03</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>RT @OpGabon: #OpGabon Call for action against ...</td>
      <td>AnonRisingIRC</td>
      <td>137</td>
      <td>790351665933217792</td>
      <td>2016-10-24 00:38:15</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>RT @OpGabon: Deface is around the corner... lo...</td>
      <td>AnonRisingIRC</td>
      <td>80</td>
      <td>788208146267463680</td>
      <td>2016-10-18 02:40:40</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>14</th>
      <td>RT @1EarlPsec: Come join us at AnonRising!!!\n...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>788207833976283136</td>
      <td>2016-10-18 02:39:26</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>15</td>
    </tr>
    <tr>
      <th>15</th>
      <td>RT @OpGabon: Supporting #Gabon dictatorship: #...</td>
      <td>AnonRisingIRC</td>
      <td>139</td>
      <td>788207713482399744</td>
      <td>2016-10-18 02:38:57</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>27</td>
    </tr>
    <tr>
      <th>16</th>
      <td>RT @snowlions: #OpGabon #Anonymous Call For Ac...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>788207694758998017</td>
      <td>2016-10-18 02:38:53</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>17</th>
      <td>RT @4thAnon: US, Corporate media have engaged ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>787429976714928128</td>
      <td>2016-10-15 23:08:30</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>40</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @basti12010: The people of #Gabon thank you...</td>
      <td>AnonRisingIRC</td>
      <td>123</td>
      <td>787429909471883264</td>
      <td>2016-10-15 23:08:14</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>19</th>
      <td>RT @OpGabon: #Gabon #NotJustYou https://t.co/d...</td>
      <td>AnonRisingIRC</td>
      <td>55</td>
      <td>787429897551613952</td>
      <td>2016-10-15 23:08:11</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>20</th>
      <td>RT @OpGabon: People of #Gabon are still standi...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>787429860746682368</td>
      <td>2016-10-15 23:08:03</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>17</td>
    </tr>
    <tr>
      <th>21</th>
      <td>#AnonRising Network Supporting #Anonymous Oper...</td>
      <td>AnonRisingIRC</td>
      <td>135</td>
      <td>787288312692219904</td>
      <td>2016-10-15 13:45:35</td>
      <td>Twitter Web Client</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>22</th>
      <td>RT @LatestAnonNews: The Latest Anonymous News ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>787132225926094848</td>
      <td>2016-10-15 03:25:21</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>23</th>
      <td>RT @Techworm_in: Anonymous hacker on hunger st...</td>
      <td>AnonRisingIRC</td>
      <td>138</td>
      <td>785915482599661568</td>
      <td>2016-10-11 18:50:27</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>22</td>
    </tr>
    <tr>
      <th>24</th>
      <td>RT @RJennromao: “@AnonRisingSec: #Cyber-ransom...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>785915445886922752</td>
      <td>2016-10-11 18:50:18</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>25</th>
      <td>RT @securityaffairs: Hacker Interviews – The A...</td>
      <td>AnonRisingIRC</td>
      <td>132</td>
      <td>785620073125781505</td>
      <td>2016-10-10 23:16:36</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>19</td>
    </tr>
    <tr>
      <th>26</th>
      <td>RT @CTIN_Global: RT securityaffairs "Hacker In...</td>
      <td>AnonRisingIRC</td>
      <td>138</td>
      <td>785619725833293824</td>
      <td>2016-10-10 23:15:13</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>27</th>
      <td>RT @OpAnimalCruelty: JUST IN: #OpAnimalCruelty...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>785100792369930241</td>
      <td>2016-10-09 12:53:10</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>16</td>
    </tr>
    <tr>
      <th>28</th>
      <td>RT @OpGabon: Thank you #Anonymous for continuo...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>784947090732552193</td>
      <td>2016-10-09 02:42:24</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RT @OpGabon: Criminal tyrant Ali Bongo and fri...</td>
      <td>AnonRisingIRC</td>
      <td>124</td>
      <td>784947057832423424</td>
      <td>2016-10-09 02:42:16</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>75</th>
      <td>RT @ccghost_: @Operation_KKK @Anon6k @AnonRisi...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>781090564456538113</td>
      <td>2016-09-28 11:17:57</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>76</th>
      <td>RT @OpGabon: «No Ambassador Cynthia Akuetteh, ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>781090527659823106</td>
      <td>2016-09-28 11:17:48</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>77</th>
      <td>RT @securityaffairs: Hacker Interviews – Anonr...</td>
      <td>AnonRisingIRC</td>
      <td>121</td>
      <td>781090389419823104</td>
      <td>2016-09-28 11:17:15</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>4</td>
    </tr>
    <tr>
      <th>78</th>
      <td>RT @4thAnon: The Internet Of Poorly Secured Th...</td>
      <td>AnonRisingIRC</td>
      <td>126</td>
      <td>781090297560301568</td>
      <td>2016-09-28 11:16:53</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>79</th>
      <td>RT @relombardo3: With So Many Things Coming Ba...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>781089964503236608</td>
      <td>2016-09-28 11:15:34</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>132</td>
    </tr>
    <tr>
      <th>80</th>
      <td>RT @OpGabon: @USEmbassyLibreville: The people ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>781089921104740352</td>
      <td>2016-09-28 11:15:23</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>81</th>
      <td>RT @OpGabon: #Gabon Tyrant Ali Bongo's atrocit...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>781089901475430401</td>
      <td>2016-09-28 11:15:19</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>82</th>
      <td>RT @OpGabon: #Gabon #FreeGabon https://t.co/de...</td>
      <td>AnonRisingIRC</td>
      <td>54</td>
      <td>781089598134968320</td>
      <td>2016-09-28 11:14:06</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>83</th>
      <td>RT @securityaffairs: Hacker Interviews – Anonr...</td>
      <td>AnonRisingIRC</td>
      <td>121</td>
      <td>780438513980477440</td>
      <td>2016-09-26 16:06:56</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>84</th>
      <td>RT @OpGabon: «People of #Gabon have no choice ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>780438494355292160</td>
      <td>2016-09-26 16:06:51</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>85</th>
      <td>RT @cjsienna55: @4n0nc47 @YourAnonNews @AnonRi...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>780438453473439744</td>
      <td>2016-09-26 16:06:41</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>86</th>
      <td>RT @OpGabon: @cjsienna55 @AnonRisingSec We nee...</td>
      <td>AnonRisingIRC</td>
      <td>109</td>
      <td>780438417872150529</td>
      <td>2016-09-26 16:06:33</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>87</th>
      <td>RT @OpGabon: #OpGabon really really need Anons...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>780438409567404037</td>
      <td>2016-09-26 16:06:31</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>88</th>
      <td>RT @OpGabon: C'est tjrs le psychopathe qui ne ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>780438359890100224</td>
      <td>2016-09-26 16:06:19</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>89</th>
      <td>RT @securityaffairs: Hacker Interviews – Anonr...</td>
      <td>AnonRisingIRC</td>
      <td>121</td>
      <td>780438323793977345</td>
      <td>2016-09-26 16:06:10</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>90</th>
      <td>RT @GroupAnon: Yahoo says it was hacked; ‘stat...</td>
      <td>AnonRisingIRC</td>
      <td>139</td>
      <td>779883151061557248</td>
      <td>2016-09-25 03:20:07</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>46</td>
    </tr>
    <tr>
      <th>91</th>
      <td>RT @GroupAnon: Hacker Who Helped #ISIS to Buil...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>779883139602800640</td>
      <td>2016-09-25 03:20:04</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>81</td>
    </tr>
    <tr>
      <th>92</th>
      <td>RT @securityaffairs: Hacker Interviews – Anonr...</td>
      <td>AnonRisingIRC</td>
      <td>121</td>
      <td>779784315689373696</td>
      <td>2016-09-24 20:47:23</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>93</th>
      <td>RT @securityaffairs: Today we will speak with ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>779784294839574528</td>
      <td>2016-09-24 20:47:18</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>94</th>
      <td>RT @OpGabon: Constitutional (Pisa) Court of #G...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>779784085673746432</td>
      <td>2016-09-24 20:46:28</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>8</td>
    </tr>
    <tr>
      <th>95</th>
      <td>RT @cjsienna55: @AnonRisingSec @OpGabon  We ha...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>779784075531915264</td>
      <td>2016-09-24 20:46:25</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6</td>
    </tr>
    <tr>
      <th>96</th>
      <td>RT @ItzQuauhtli: #OpGabon https://t.co/uPC6K82GbT</td>
      <td>AnonRisingIRC</td>
      <td>49</td>
      <td>779783649495572480</td>
      <td>2016-09-24 20:44:44</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>13</td>
    </tr>
    <tr>
      <th>97</th>
      <td>RT @FreeLauriLove: Lauri Love's father: US ext...</td>
      <td>AnonRisingIRC</td>
      <td>124</td>
      <td>779783596001337347</td>
      <td>2016-09-24 20:44:31</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>10</td>
    </tr>
    <tr>
      <th>98</th>
      <td>RT @FreeLauriLove: Sign these petitions!  No e...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>779783581786841088</td>
      <td>2016-09-24 20:44:28</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>99</th>
      <td>RT @ERLNCINAR: @N3mesiss @h0t_p0ppy @AnonRisin...</td>
      <td>AnonRisingIRC</td>
      <td>88</td>
      <td>779783158829113344</td>
      <td>2016-09-24 20:42:47</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>100</th>
      <td>RT @TeamSmokie: NO GUN until the officer with ...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>779782903794458625</td>
      <td>2016-09-24 20:41:46</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>6372</td>
    </tr>
    <tr>
      <th>101</th>
      <td>RT @h0t_p0ppy: @ERLNCINAR @N3mesiss @Anon4dolp...</td>
      <td>AnonRisingIRC</td>
      <td>146</td>
      <td>779782369867882496</td>
      <td>2016-09-24 20:39:39</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>102</th>
      <td>RT @FreeLauriLove: Scot Lauri Love to be extra...</td>
      <td>AnonRisingIRC</td>
      <td>139</td>
      <td>779781867100835840</td>
      <td>2016-09-24 20:37:39</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>103</th>
      <td>RT @FreeWestPapua: #Vinaka! To our #Fiji famil...</td>
      <td>AnonRisingIRC</td>
      <td>140</td>
      <td>779781838760017920</td>
      <td>2016-09-24 20:37:32</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>9</td>
    </tr>
    <tr>
      <th>104</th>
      <td>RT @freesecpower: https://t.co/Q4f6UyK9UO it`s...</td>
      <td>AnonRisingIRC</td>
      <td>60</td>
      <td>779768709229506560</td>
      <td>2016-09-24 19:45:22</td>
      <td>Twitter for iPhone</td>
      <td>0</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>105 rows × 8 columns</p>
</div>


    The user name is:
    @ANONSPAIN2



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Tweets</th>
      <th>Name</th>
      <th>Length</th>
      <th>ID</th>
      <th>Date</th>
      <th>Source</th>
      <th>Likes</th>
      <th>RTs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>No hay escaleras para tanto fascista.</td>
      <td>ANONSPAIN2</td>
      <td>37</td>
      <td>1062668662241484801</td>
      <td>2018-11-14 11:28:47</td>
      <td>Twitter Lite</td>
      <td>18</td>
      <td>11</td>
    </tr>
    <tr>
      <th>1</th>
      <td>RT @JonInarritu: ¿Es el “francotirador franqui...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062625350369325056</td>
      <td>2018-11-14 08:36:40</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>4414</td>
    </tr>
    <tr>
      <th>2</th>
      <td>RT @publico_es: La red destapa al afiliado de ...</td>
      <td>ANONSPAIN2</td>
      <td>125</td>
      <td>1062602633272274944</td>
      <td>2018-11-14 07:06:24</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>32</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Los dos documentalistas denunciados por tratar...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062598701842087943</td>
      <td>2018-11-14 06:50:47</td>
      <td>Twitter Lite</td>
      <td>8</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>El FRANCOtirador que quería matar a Sánchez er...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062597691190972416</td>
      <td>2018-11-14 06:46:46</td>
      <td>Twitter Lite</td>
      <td>9</td>
      <td>23</td>
    </tr>
    <tr>
      <th>5</th>
      <td>RT @X_net_: La nueva SGAE totalmente renovada ...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062460000562483200</td>
      <td>2018-11-13 21:39:38</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>39</td>
    </tr>
    <tr>
      <th>6</th>
      <td>RT @carnecrudaradio: Con un par: #Hevia, nuevo...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062459972301348864</td>
      <td>2018-11-13 21:39:31</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>43</td>
    </tr>
    <tr>
      <th>7</th>
      <td>@Aperopr @Coordinadora25S Un partido fundado p...</td>
      <td>ANONSPAIN2</td>
      <td>90</td>
      <td>1062455218670047233</td>
      <td>2018-11-13 21:20:38</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>RT @Coordinadora25S: El PP de Pablo Casado pro...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062454945369202688</td>
      <td>2018-11-13 21:19:33</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>61</td>
    </tr>
    <tr>
      <th>9</th>
      <td>@_Just_Madrid_ De hijos de PUTEROS</td>
      <td>ANONSPAIN2</td>
      <td>34</td>
      <td>1062429157236400131</td>
      <td>2018-11-13 19:37:04</td>
      <td>Twitter Lite</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>RT @isaacfcorrales: Os dais cuenta del chaval ...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062404124875071488</td>
      <td>2018-11-13 17:57:36</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>9451</td>
    </tr>
    <tr>
      <th>11</th>
      <td>#MiSímboloMiPaís https://t.co/i34iFJdWM1</td>
      <td>ANONSPAIN2</td>
      <td>40</td>
      <td>1062401269275463680</td>
      <td>2018-11-13 17:46:15</td>
      <td>Twitter Lite</td>
      <td>12</td>
      <td>9</td>
    </tr>
    <tr>
      <th>12</th>
      <td>#MiSímboloMiPaís https://t.co/NOsmRZWKid</td>
      <td>ANONSPAIN2</td>
      <td>40</td>
      <td>1062400979499470852</td>
      <td>2018-11-13 17:45:06</td>
      <td>Twitter Lite</td>
      <td>5</td>
      <td>8</td>
    </tr>
    <tr>
      <th>13</th>
      <td>RT @edugalan: Desde hace días me está escribie...</td>
      <td>ANONSPAIN2</td>
      <td>139</td>
      <td>1062400612359376896</td>
      <td>2018-11-13 17:43:39</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>14</th>
      <td>@Bernat_Deltell https://t.co/aZ0tNgX8Bw</td>
      <td>ANONSPAIN2</td>
      <td>39</td>
      <td>1062400355588366338</td>
      <td>2018-11-13 17:42:37</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>RT @Bernat_Deltell: 😂😂😂😂😂😂\n\nEl Rey ofrece a ...</td>
      <td>ANONSPAIN2</td>
      <td>133</td>
      <td>1062399937617502208</td>
      <td>2018-11-13 17:40:58</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>935</td>
    </tr>
    <tr>
      <th>16</th>
      <td>RT @Asil_Vestra0: Como desmontar al intoxicado...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062399419109253120</td>
      <td>2018-11-13 17:38:54</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>12304</td>
    </tr>
    <tr>
      <th>17</th>
      <td>RT @jpurias: Con Ustedes don Francisco Serrano...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062399183116779522</td>
      <td>2018-11-13 17:37:58</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>4820</td>
    </tr>
    <tr>
      <th>18</th>
      <td>RT @protestona1: ¿Garantizar el respeto a los ...</td>
      <td>ANONSPAIN2</td>
      <td>139</td>
      <td>1062398161166262272</td>
      <td>2018-11-13 17:33:54</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>1674</td>
    </tr>
    <tr>
      <th>19</th>
      <td>RT @PotiPotiInLove: "Oh, hola, persona descono...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062397798530859010</td>
      <td>2018-11-13 17:32:28</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>30</td>
    </tr>
    <tr>
      <th>20</th>
      <td>RT @ikaitor: La Policía Nacional permite a Jus...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062394735837491207</td>
      <td>2018-11-13 17:20:18</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>795</td>
    </tr>
    <tr>
      <th>21</th>
      <td>RT @Asil_Vestra0: - Willy Toledo? #MiSímboloMi...</td>
      <td>ANONSPAIN2</td>
      <td>74</td>
      <td>1062394426012680192</td>
      <td>2018-11-13 17:19:04</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>14</td>
    </tr>
    <tr>
      <th>22</th>
      <td>RT @SilenceofOthers: #16NRompamosElSilencio\n\...</td>
      <td>ANONSPAIN2</td>
      <td>107</td>
      <td>1062394029512515584</td>
      <td>2018-11-13 17:17:29</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>133</td>
    </tr>
    <tr>
      <th>23</th>
      <td>RT @YORYO25MD: @Frank_Cuesta dos idiotas NO..\...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062388170363428865</td>
      <td>2018-11-13 16:54:12</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>24</th>
      <td>RT @SaraRiveiro: El único nazi bueno es el naz...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062386159656022016</td>
      <td>2018-11-13 16:46:13</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>6350</td>
    </tr>
    <tr>
      <th>25</th>
      <td>RT @Cazatalentos: Alguien tiene que decirlo.\n...</td>
      <td>ANONSPAIN2</td>
      <td>129</td>
      <td>1062383152029667328</td>
      <td>2018-11-13 16:34:16</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>42</td>
    </tr>
    <tr>
      <th>26</th>
      <td>RT @YourAnonRise: we are a movement. An idea. ...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062381838973526016</td>
      <td>2018-11-13 16:29:03</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>27</th>
      <td>RT @Yo_Soy_Asin: QUE TODO EL MUNDO LOS VEA\n\n...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062381766411984896</td>
      <td>2018-11-13 16:28:45</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>1932</td>
    </tr>
    <tr>
      <th>28</th>
      <td>RT @grancocolio: El acosador de Willy Toledo d...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062378887517274112</td>
      <td>2018-11-13 16:17:19</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>1477</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RT @Yo_Soy_Asin: QUE GRANDE WILLY TOLEDO \n\nP...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1062378033598943232</td>
      <td>2018-11-13 16:13:55</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>4216</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>170</th>
      <td>RT @redvamas: @pablocasado_ Di NO al racismo P...</td>
      <td>ANONSPAIN2</td>
      <td>76</td>
      <td>1061323450751639552</td>
      <td>2018-11-10 18:23:23</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>29</td>
    </tr>
    <tr>
      <th>171</th>
      <td>RT @GeekIndignado: Por lo visto han agredido a...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061322952195620872</td>
      <td>2018-11-10 18:21:24</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <th>172</th>
      <td>Las compas de @La9deAnon están revisando la co...</td>
      <td>ANONSPAIN2</td>
      <td>95</td>
      <td>1061321800901111813</td>
      <td>2018-11-10 18:16:50</td>
      <td>Twitter Lite</td>
      <td>197</td>
      <td>156</td>
    </tr>
    <tr>
      <th>173</th>
      <td>RT @La9deAnon: Muy españoles mucho españoles p...</td>
      <td>ANONSPAIN2</td>
      <td>88</td>
      <td>1061320791692902400</td>
      <td>2018-11-10 18:12:49</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>185</td>
    </tr>
    <tr>
      <th>174</th>
      <td>RT @La9deAnon: ¿Lo de las transferencias de fo...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061320778015277057</td>
      <td>2018-11-10 18:12:46</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>196</td>
    </tr>
    <tr>
      <th>175</th>
      <td>RT @La9deAnon: ¿Informáticos informando? Pero ...</td>
      <td>ANONSPAIN2</td>
      <td>95</td>
      <td>1061320769140113409</td>
      <td>2018-11-10 18:12:44</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>188</td>
    </tr>
    <tr>
      <th>176</th>
      <td>RT @La9deAnon: Las campañas de Crowdfunding So...</td>
      <td>ANONSPAIN2</td>
      <td>95</td>
      <td>1061320759061176326</td>
      <td>2018-11-10 18:12:42</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>196</td>
    </tr>
    <tr>
      <th>177</th>
      <td>RT @La9deAnon: ¿No hay tecnológicas entre vues...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061320747698786305</td>
      <td>2018-11-10 18:12:39</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>149</td>
    </tr>
    <tr>
      <th>178</th>
      <td>RT @La9deAnon: ¿La Ejecutiva come gratis en la...</td>
      <td>ANONSPAIN2</td>
      <td>78</td>
      <td>1061320701414703104</td>
      <td>2018-11-10 18:12:28</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>358</td>
    </tr>
    <tr>
      <th>179</th>
      <td>RT @ANONSPAIN2: Cuando alguien te diga "ni de ...</td>
      <td>ANONSPAIN2</td>
      <td>138</td>
      <td>1061320425878237185</td>
      <td>2018-11-10 18:11:22</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>28</td>
    </tr>
    <tr>
      <th>180</th>
      <td>RT @Bernat_Castro: Curso de cómo blanquear a u...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061308842645114880</td>
      <td>2018-11-10 17:25:20</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>349</td>
    </tr>
    <tr>
      <th>181</th>
      <td>RT @EReflexionador: @jibar0back @antonisemu @A...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061293836050329600</td>
      <td>2018-11-10 16:25:43</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>182</th>
      <td>RT @Coordinadora25S: Hay alguna razón para que...</td>
      <td>ANONSPAIN2</td>
      <td>139</td>
      <td>1061288080181731328</td>
      <td>2018-11-10 16:02:50</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>114</td>
    </tr>
    <tr>
      <th>183</th>
      <td>RT @Bernat_Castro: "como las mariconas"\n\nNo ...</td>
      <td>ANONSPAIN2</td>
      <td>81</td>
      <td>1061284615640489985</td>
      <td>2018-11-10 15:49:04</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>671</td>
    </tr>
    <tr>
      <th>184</th>
      <td>RT @Houseof_CAT: La pérdida de la dignidad per...</td>
      <td>ANONSPAIN2</td>
      <td>139</td>
      <td>1061284516373938177</td>
      <td>2018-11-10 15:48:41</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>3214</td>
    </tr>
    <tr>
      <th>185</th>
      <td>RT @antonisemu: @ANONSPAIN2 @PadMediaFiles @An...</td>
      <td>ANONSPAIN2</td>
      <td>134</td>
      <td>1061268027751219202</td>
      <td>2018-11-10 14:43:09</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>5</td>
    </tr>
    <tr>
      <th>186</th>
      <td>Altos cargos de #Jusapol reconocen en privado ...</td>
      <td>ANONSPAIN2</td>
      <td>116</td>
      <td>1061262961065541632</td>
      <td>2018-11-10 14:23:01</td>
      <td>Twitter Lite</td>
      <td>11</td>
      <td>16</td>
    </tr>
    <tr>
      <th>187</th>
      <td>RT @PadMediaFiles: @Anonymus_ES Ya troleando @...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061261536193380355</td>
      <td>2018-11-10 14:17:22</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>44</td>
    </tr>
    <tr>
      <th>188</th>
      <td>@PadMediaFiles @Anonymus_ES @CiudadanosCs Vaya...</td>
      <td>ANONSPAIN2</td>
      <td>139</td>
      <td>1061261496431362049</td>
      <td>2018-11-10 14:17:12</td>
      <td>Twitter Lite</td>
      <td>26</td>
      <td>24</td>
    </tr>
    <tr>
      <th>189</th>
      <td>@redvamas Quitter, N1, rise... Etc</td>
      <td>ANONSPAIN2</td>
      <td>34</td>
      <td>1061248718354243584</td>
      <td>2018-11-10 13:26:26</td>
      <td>Twitter Lite</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>190</th>
      <td>🐘 Mastodon, así es la alternativa a Twitter qu...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061239015050039296</td>
      <td>2018-11-10 12:47:52</td>
      <td>Twitter Web Client</td>
      <td>24</td>
      <td>19</td>
    </tr>
    <tr>
      <th>191</th>
      <td>RT @MonicaLimoni: Los de #Xusmapol han llegado...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061234991307386880</td>
      <td>2018-11-10 12:31:53</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>11</td>
    </tr>
    <tr>
      <th>192</th>
      <td>Estudiantes de dos nuevas universidades se sum...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061233544423501824</td>
      <td>2018-11-10 12:26:08</td>
      <td>Twitter Web Client</td>
      <td>23</td>
      <td>18</td>
    </tr>
    <tr>
      <th>193</th>
      <td>🌍 A partir del 12 de noviembre, todos los mens...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061232758897414144</td>
      <td>2018-11-10 12:23:01</td>
      <td>Twitter Web Client</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>194</th>
      <td>RT @AnonValladolor: El presidente del PP borra...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061227687505809411</td>
      <td>2018-11-10 12:02:52</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>195</th>
      <td>RT @ANONSPAIN_3: #MillionMaskMarch2018 \n#Mill...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061221964512182273</td>
      <td>2018-11-10 11:40:07</td>
      <td>Twitter Web Client</td>
      <td>0</td>
      <td>7</td>
    </tr>
    <tr>
      <th>196</th>
      <td>Muy buenos días, Anons!\nCUENTA RECUPERADA!\nG...</td>
      <td>ANONSPAIN2</td>
      <td>122</td>
      <td>1061220086541885442</td>
      <td>2018-11-10 11:32:39</td>
      <td>Facebook</td>
      <td>22</td>
      <td>11</td>
    </tr>
    <tr>
      <th>197</th>
      <td>Muy buenos días, Anons!\nCUENTA RECUPERADA!\nG...</td>
      <td>ANONSPAIN2</td>
      <td>121</td>
      <td>1061219210947371008</td>
      <td>2018-11-10 11:29:11</td>
      <td>Twitter Web Client</td>
      <td>31</td>
      <td>10</td>
    </tr>
    <tr>
      <th>198</th>
      <td>Follow our new account ➡ @ANONSPAIN_3\n*#DDoS ...</td>
      <td>ANONSPAIN2</td>
      <td>140</td>
      <td>1061213412724760576</td>
      <td>2018-11-10 11:06:08</td>
      <td>Facebook</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>199</th>
      <td>Follow our new account ➡ @ANONSPAIN_3\nJiménez...</td>
      <td>ANONSPAIN2</td>
      <td>139</td>
      <td>1061200183105609728</td>
      <td>2018-11-10 10:13:34</td>
      <td>Facebook</td>
      <td>1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>200 rows × 8 columns</p>
</div>


    The user name is:
    @AnonyInfo
    Failed to run the command on that user, Skipping...
    User´s profile in a private mode, imposible extract tweets



```python
#Removing punctuation

#display(result['Tweets'])

natural_tweets = result['Tweets'].apply(lambda x: x.lower())


exclude = set(string.punctuation)
def remove_punctuation(x):
    """
    Helper function to remove punctuation from a string
    x: any string
    """
    try:
        x = ''.join(ch for ch in x if ch not in exclude)
    except:
        pass
    return x

#Apply the function to the DataFrame
single_words = ['rt', 'us', '1', '2', 'one', '🇵🇸', 'new']

fixed_tweets = natural_tweets.apply(remove_punctuation)
stop = stopwords.words('english') + stopwords.words('spanish') + single_words
result['Tweets_without_stopwords'] = fixed_tweets.apply(lambda x: ' '.join([word for word in x.split() if word not in (stop)]))
#print(fixed_tweets)
display(result['Tweets_without_stopwords'])
```


    0       ajmhashtag من اخترق موقع وكالة أنباء الشرق الأ...
    1       bbcarabic صورة قيادي بالإخوان تتصدر صفحة وكالة...
    2       elsanhory اختراق موقع وكالة أنباء الشرق الأوسط...
    3       bbcmonitoring egypts official news agency mena...
    4       elhady عاجل اختراق موقع وكالة أنباء الشرق الأو...
    5       3yyash middle east news agency state news agen...
    6       rassdnewsn هاكرز تركي يخترق وكالة أنباء الشرق ...
    7       rassdnewsn اخترق هاكر، وكالة أنباء الشرق الأوس...
    8       alkhames قالت الهيئة الوطنية للصحافة في مصر إن...
    9       cyberwarriortim türk hackerlar mısır resmi hab...
    10      cyberwarriortim türk hackerlar mısır resmi hab...
    11      cyberwarriortim türk hackerlar mısır resmi hab...
    12      cyberwarriortim türk hackerlar mısır resmi aja...
    13      cyberwarriortim türk hackerlar mısır resmi hab...
    14      cyberwarriortim türk hacker grubu cyberwarrior...
    15      cyberwarriortim kurban bayramının ülkemize mil...
    16      cyberwarriortim kendilerini kürdish hackers ol...
    17      yusufhanedangil bu ülkede vatanseverler alimle...
    18      tskgnkur 15 haziran 2018 tarihinde irak kuzeyi...
    19      cyberwarriortim cyberwarriortim akincilar terö...
    20      cyberwarriortim cyberwarriortim oku öğren payl...
    21      cyberwarriortim başta aziz şehitlerimizi rahme...
    22      dirilispostasi akıncılar hacker grubu diriliş ...
    23      enesvrol selamünaleyküm fsm hastanesinde yatan...
    24            çılgın türkler akincilar httpstcoxnrr6bbr1i
    25       avrupa’da gündem akincilar 🇹🇷 httpstcopi6ykcxfzz
    26      cyberwarriortim türk hackerler yunanı çıldırtt...
    27      yunanistan’ın gündemi akincilar 🇹🇷 httpstconzx...
    28      tskgnkur siirteruh’da icra edilen hava destekl...
    29      cyberwarriortim alt yapı çalışmalarımız tamaml...
                                  ...                        
    1325    redvamas pablocasado di racismo pablito httpst...
    1326    geekindignado visto agredido español bien metr...
    1327    compas la9deanon revisando contabilidad jusapo...
    1328    la9deanon españoles españoles cc banco sabadell 🤣
    1329    la9deanon ¿lo transferencias fondos directiva ...
    1330    la9deanon ¿informáticos informando oyes 🤣 http...
    1331    la9deanon campañas crowdfunding solidario dan ...
    1332    la9deanon ¿no tecnológicas uniformados vayan r...
    1333    la9deanon ¿la ejecutiva come gratis manis http...
    1334    anonspain2 alguien diga izquierdas derechas te...
    1335    bernatcastro curso cómo blanquear nazi apaliza...
    1336    ereflexionador jibar0back antonisemu anonspain...
    1337    coordinadora25s alguna razón jusapol manifiest...
    1338       bernatcastro mariconas diré httpstco1rfdfifuwc
    1339    houseofcat pérdida dignidad periodística resum...
    1340    antonisemu anonspain2 padmediafiles anonymuses...
    1341    altos cargos jusapol reconocen privado ciudada...
    1342    padmediafiles anonymuses troleando ciudadanosc...
    1343    padmediafiles anonymuses ciudadanoscs vaya fal...
    1344                         redvamas quitter n1 rise etc
    1345    🐘 mastodon así alternativa twitter evita censu...
    1346    monicalimoni xusmapol llegado larc triomf 10na...
    1347    estudiantes dos nuevas universidades suman ref...
    1348    🌍 partir 12 noviembre mensajes guardado plataf...
    1349    anonvalladolor presidente pp borra currículum ...
    1350    anonspain3 millionmaskmarch2018 millionmaskmar...
    1351    buenos días anons cuenta recuperada gracias to...
    1352    buenos días anons cuenta recuperada gracias to...
    1353    follow account ➡ anonspain3 ddos 👉su programa ...
    1354    follow account ➡ anonspain3 jiménez losantos i...
    Name: Tweets_without_stopwords, Length: 1355, dtype: object



```python
#Counter

counted = pd.Series(np.concatenate([x.split() for x in result['Tweets_without_stopwords']])).value_counts()

print(counted[0 : 30])
```

    anonymous             125
    opgabon                79
    scode404               72
    anonghost              66
    cyberwarriortim        55
    youranonrevolt         53
    pursuance              53
    people                 50
    anonghostcj            49
    httpstcobs4zlo3p0r     47
    today                  47
    gabon                  44
    minionghost            42
    barrettbrown           38
    pursuanceproj          38
    anonghost07            36
    tracked                35
    opisrael               33
    opicarus               30
    hacked                 29
    nazi                   28
    trump                  28
    hacker                 27
    israel                 27
    leaked                 26
    rayjoha2               25
    hkurrafizimrit         25
    news                   25
    opindia                24
    support                24
    dtype: int64



```python
#Hashtag

hashtag = pd.Series(np.concatenate([x.split() for x in natural_tweets])).value_counts()
instance1="#kyu"
for i in hashtag:
    if "#" in hashtag.index[i]:
        if instance1 != hashtag.index[i]:
            print(hashtag.index[i],hashtag[i])
            instance1=hashtag.index[i]
            
```

    #opicarus2017 21
    #gabon 40
    #anonghost 61
    #anonymous 128



```python
#Username

twuser = pd.Series(np.concatenate([x.split() for x in natural_tweets])).value_counts()
instance2="@kyu"
for i in twuser:
    if "@" in twuser.index[i]:
        if instance2 != twuser.index[i]:
            print(twuser.index[i],twuser[i])
            instance2=twuser.index[i]
            
```

    @foxnews: 4
    @ufukcc 12
    @its_rivet19 15
    @anonghost07: 19
    @anonghostcj 46
    @opgabon: 51
    @youranonrevolt 53
    @scode404 71



```python
#classifier dataset 1

#deleting username from counter


# add keywords to the file keywords.xlm


# re-run file (loop)


#make scatter plot (baseline)


# logistic regression

#how much users use the keywors

#Ylabel frequency uses of keywords

#Xlabel user
```
