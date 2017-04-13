<properties
    pageTitle="Twitter Apache rakenne HDInsight-tietojen analysoiminen | Microsoft Azure"
    description="Opettele Python avulla voit tallentaa Tweets, joka sisältää avainsanoja ja käyttää rakenne ja Hadoop HDInsight raaka TWitter-tietojen muuntamiseen etsittävän rakenne-taulukkoon."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Voit analysoida Twitter-tietoja käyttämällä HDInsight-rakenne

Tässä asiakirjassa saavat tweets käyttämällä Twitteriin, streaming API ja valitse Käytä Apache rakenne JSON-prosessi Linux-pohjaiset HDInsight-klusterissa muotoillut tiedot. Tulos on Twitter-käyttäjät, jotka lähetetään useimmat tweets, joka sisältää tietyn sanan luettelo.

> [AZURE.NOTE] Kun tämän asiakirjan yksittäiset osat voidaan käyttää Windows-pohjaisesta HDInsight varausyksiköt (esimerkiksi Python), jossa monessa vaiheessa perustuvat Linux-pohjaiset HDInsight-klusterin käyttämiseen. Windows-pohjaisesta klusteriin tiettyjä ohjeita on kohdassa [Twitter analysoida tietoja käyttämällä HDInsight-rakenne](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- __Linux-pohjaiset Azure HDInsight-klusterin__. Lisätietoja klusterin luomisesta on artikkelissa ohjeet klusterin luominen [Linux-pohjaiset HDInsight käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __SSH asiakas__. Lisätietoja Linux-pohjaiset HDInsight SSH käyttämisestä on seuraavissa artikkeleissa:

    * [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ ja [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Hae Twitter-syöte

Twitter avulla voit hakea [tietoja kunkin tweetin](https://dev.twitter.com/docs/platform-objects/tweets) REST API-Liittymän kautta JavaScript Object Notation (JSON)-tiedostona. [OAuth](http://oauth.net) tarvitaan ohjelmointirajapinnan todennusta varten. Sinun on luotava _Twitter-sovellusta_ , joka sisältää käyttää Ohjelmointirajapinnan asetukset.

###<a name="create-a-twitter-application"></a>Twitter-sovelluksen luominen

1. Web-selaimessa Kirjaudu [https://apps.twitter.com/](https://apps.twitter.com/). Valitse **Rekisteröi nyt** -linkki, jos sinulla ei ole Twitter-tili.
2. Valitse **Luo uusi sovellus**.
3. Kirjoita **nimi**, **kuvaus**- **sivustossa**. Voit tehdä **sivusto** -kenttä URL-Osoitteen määrittäminen. Seuraavassa taulukossa on joidenkin esimerkkiarvojen käyttäminen:

  	| Kenttä | Arvo |
  	|:----- |:----- |
  	| Nimi  | MyHDInsightApp |
  	| Kuvaus | MyHDInsightApp |
  	| Sivuston | http://www.myhdinsightapp.com |
    
4. Valitse **hyväksyn**ja valitse sitten **Luo Twitter-sovellusta**.
5. Valitse **käyttöoikeudet** -välilehti. Oletus-käyttöoikeus on **vain luku**. Tämä on riittävä Tässä opetusohjelmassa.
6. Valitse **näppäimet ja tunnusten Access** -välilehti.
7. Valitse **Luo oma käyttöoikeustietue**.
8. Valitse **Testaa OAuth** sivun oikeassa yläkulmassa.
9. Kirjoita muistiin **kuluttaja-näppäintä**, **kuluttaja salaisuus**tai **käyttöoikeustietue** **Access suojaustunnuksen salaisuus**. Tarvitset arvot myöhemmin.

>[AZURE.NOTE] Kun käytät Windowsin kääntö-komentoa, käyttää lainausmerkkejä sijaan puolilainausmerkkeihin vaihtoehto arvoista.

###<a name="download-tweets"></a>Lataa tweets

Seuraava Python koodi 10 000 tweets lataaminen Twitter ja tallentaa ne __tweets.txt__-tiedostoon.

> [AZURE.NOTE] Seuraavat vaiheet suoritetaan HDInsight-klusterin, koska Python on jo asennettu.

1. Yhdistä käyttäen SSH HDInsight-klusterin:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Jos olet käyttänyt suojaamiseen SSH käyttäjätilin salasanan, voit pyydetään antamaan sitä. Jos olet käyttänyt julkisella avaimella, saatat joutua käyttämään `-i` parametri, voit määrittää vastaava yksityinen avain. Esimerkiksi `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Lisätietoja Linux-pohjaiset HDInsight SSH käyttämisestä on seuraavissa artikkeleissa:
    
    * [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Oletusarvon mukaan __pip__ -apuohjelmaa ei ole asennettu pään HDInsight-solmu. Seuraavat avulla voit asentaa ja Päivitä tämän apuohjelman:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Voit ladata tweets koodi on riippuvainen [Tweepy](http://www.tweepy.org/) ja [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Asenna nämä seuraavalla komennolla:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Bittien python-openssl python keskihajonta, libffi keskihajonta, libssl keskihajonta, pyOpenSSL ja pyynnöt [suojaus]-asennuksen poistaminen on välttää InsecurePlatform varoitus, kun muodostaa yhteyden Python Twitter SSL kautta.
    >
    > Voit välttää [virheen](https://github.com/tweepy/tweepy/issues/576) , joita voi ilmetä käsiteltäessä tweets käytetään Tweepy v3.2.0.

4. Seuraavalla komennolla voit luoda uuden tiedoston nimeltä __gettweets.py__:

        nano gettweets.py

5. Määritä seuraavat __gettweets.py__ -tiedoston sisällön. Korvaa paikkamerkin tiedot __consumer\_salaisuus__, __consumer\_avaimen__, __access /\_suojaustunnuksen__, ja __access\_suojaustunnuksen\_salaisuus__ Twitter-sovellusta tiedoilla.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Tallenna tiedosto __Ctrl + X__ja __Y__ avulla.

7. Käytä Suorita-tiedosto ja lataa tweets seuraava komento:

        python gettweets.py

    Tilanneilmaisin tulee näkyviin, ja laskea enintään 100 % tweets ladataan ja tallennetaan tiedostoon.

    > [AZURE.NOTE] Jos kestää kauan tilanneilmaisin etenee, sinun on muutettava suodatin voi seurata tykkäysten aiheita Kun luettelossa on paljon tweets suodatettavan aiheesta, saat nopeasti 10000 tweets tarvitaan.

###<a name="upload-the-data"></a>Lataa tiedot

Tietojen lataaminen WASB (HDInsight käyttämä jaettu tiedostojärjestelmä), käytä seuraavia komentoja:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Tiedot tallennetaan sijaintiin, jossa kaikki solmut klusterin käyttöoikeus.

##<a name="run-the-hiveql-job"></a>Suorita HiveQL työ


1. Seuraavalla komennolla voit luoda HiveQL lauseet sisältävän tiedoston:

        nano twitter.hql
    
    Käyttää tiedoston sisällön seuraavasti:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        
3. Paina näppäinyhdistelmää __Ctrl + X__ja paina __Y__ tiedoston tallennuspaikka.

4. Käytä seuraavaa komentoa Suorita-tiedostossa HiveQL:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    Tämä ladata rakenne-liittymän, suorita HiveQL __twitter.hql__ tiedostossa ja lopuksi palauttaa `jdbc:hive2//localhost:10001/>` kehote.
    
5. Varmista, että voit valita tiedot __twitter.hql__ tiedostossa HiveQL luoma __tweets__ taulukosta beeline kehotettaessa seuraavat avulla:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Tämä palauttaa enintään 10 tweets olevien sanojen __Azure__ viestin tekstin.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa on tullut voit muuntaa rakenteeton JSON-tietojoukko rakenteellisia rakennetaulukko kyselyn, tutkimiseen ja analysoimiseen Twitter tiedot käyttämällä Azure Hdinsightista. Lisätietoja on artikkeleissa:

- [HDInsight käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Analysoi viive lentotietoihin käyttämällä Hdinsightiin](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
