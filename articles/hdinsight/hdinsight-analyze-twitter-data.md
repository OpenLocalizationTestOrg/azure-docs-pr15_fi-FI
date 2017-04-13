<properties
    pageTitle="Hadoop HDInsight Twitter tietojen analysoiminen | Microsoft Azure"
    description="Opettele käyttämään rakennetta, voit etsiä tietyn sanan käyttö taajuus HDInsight Hadoop Twitter-tietojen analysointia varten."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Voit analysoida Twitter-tietoja käyttämällä HDInsight-rakenne

Sosiaalisten sivustot ovat yhtä suuret ajo pakottaa big datasta hyväksyttäväksi. Julkisen API Twitter sivustoja varten on hyödyllinen tietolähde tietojen analysointiin ja tietoja Suositut trendejä. Tässä opetusohjelmassa Hae tweets Twitteriin, streaming API avulla ja käytä Apache rakenne Azure Hdinsightiin, Twitter-käyttäjät, jotka lähetetään useimmat tweets, joka sisältää tietyn sanan luettelo.

> [AZURE.NOTE] Tässä asiakirjassa vaiheet pätevät Windows-pohjaisesta HDInsight-klusterin. Ohjeita tietyn Linux-klusteriin on kohdassa [analysoida Twitter-tietoja käyttämällä rakenne HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Työasema** PowerShellin Azure on asennettu ja määritetty. 

    Windows PowerShell-komentosarjojen suorittamiseen PowerShellin Azure Suorita järjestelmänvalvojana ja suorittamisen käytännön asettaminen *RemoteSigned*. [Suorita Windows PowerShell-komentosarjoja]kohdassa[powershell-script].

    Ennen kuin suoritat Windows PowerShell-komentosarjojen, varmista, että olet muodostanut yhteyden Azure tilauksen seuraavat cmdlet-komennolla:

        Login-AzureRmAccount

    Jos sinulla on useita tilauksia Azure, seuraavan cmdlet-komennon avulla voit määrittää voimassa oleva tilaus:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Azure HDInsight-klusterin**. Saat ohjeita klusteri valmisteleminen [käyttäminen HDInsight] [ hdinsight-get-started] tai [säännöstä HDInsight klustereiden] [hdinsight-provision]. Tarvitset myöhemmin opetusohjelman-klusterinimeä.

Seuraavassa taulukossa on lueteltu tässä opetusohjelmassa käytetyt tiedostot:

Tiedostot|Kuvaus
---|---
/tutorials/Twitter/Data/tweets.txt|Rakenne-työ lähdetietoja.
/tutorials/Twitter/Output|Rakenne-työn Tulostekansio. Rakenteen työn tulosteen tiedoston oletusnimeä on **000000_0**.
tutorials/Twitter/Twitter.hql|HiveQL-komentosarjatiedosto.
/tutorials/Twitter/JobStatus|Hadoop projektin tila.


##<a name="get-twitter-feed"></a>Hae Twitter-syöte

Tässä opetusohjelmassa käytät [Twitter-streaming API][twitter-streaming-api]. Erityiset Twitter-Ohjelmointirajapinta, jota käytät streaming on [tilat tai suodatus][twitter-statuses-filter].

>[AZURE.NOTE] Tiedoston, jossa on 10 000 tweets ja rakenne-komentosarjatiedosto (kattaa seuraavan osion) on ladattu julkisen Blob-säilö. Voit ohittaa tämän osan, jos haluat käyttää ladattuja tiedostoja.

[Tweets tiedot](https://dev.twitter.com/docs/platform-objects/tweets) tallennetaan JavaScript Object Notation (JSON)-muodossa, joka sisältää sisäkkäisiä monimutkaisessa rakenteessa. Sen sijaan, että kirjoitat monta riviä koodin käyttämällä tavalliseen ohjelmointikieli, voit muuntaa sisäkkäisiä rakenteen rakenteen taulukoksi niin, että se voidaan suorittaa kysely mukaan SQL Structured Query Language ()-kieli, jota kutsutaan HiveQL, kuten.

Twitter käyttää OAuth valtuutettujen access sen API. OAuth on todennusprotokolla, jonka avulla käyttäjät voivat hyväksyä sovellukset toimimaan ilman jakaminen salasanansa heidän puolestaan. Lisätietoja löytyy [oauth.net](http://oauth.net/) vähintään erinomainen [Aloittelijan opas OAuth](http://hueniverse.com/oauth/) -Hueniverse.

Käyttämään OAuth ensimmäiseksi on luotava uusi sovellus Twitter-Kehitystyökalut-sivustossa.

**Twitter-sovelluksen luominen**

1. Kirjaudu sisään [https://apps.twitter.com/](https://apps.twitter.com/). Valitse **Rekisteröi nyt** -linkki, jos sinulla ei ole Twitter-tili.
2. Valitse **Luo uusi sovellus**.
3. Kirjoita **nimi**, **kuvaus**- **sivustossa**. Voit tehdä **sivusto** -kenttä URL-Osoitteen määrittäminen. Seuraavassa taulukossa on joidenkin esimerkkiarvojen käyttäminen:

    Kenttä|Arvo
    ---|---
    Nimi|MyHDInsightApp
    Kuvaus|MyHDInsightApp
    Sivuston|http://www.myhdinsightapp.com

4. Valitse **hyväksyn**ja valitse sitten **Luo Twitter-sovellusta**.
5. Valitse **käyttöoikeudet** -välilehti. Oletus-käyttöoikeus on **vain luku**. Tämä on riittävä Tässä opetusohjelmassa.
6. Valitse **näppäimet ja tunnusten Access** -välilehti.
7. Valitse **Luo oma käyttöoikeustietue**.
8. Valitse **Testaa OAuth** sivun oikeassa yläkulmassa.
9. Kirjoita muistiin **kuluttaja-näppäintä**, **kuluttaja salaisuus**tai **käyttöoikeustietue** **Access suojaustunnuksen salaisuus**. Tarvitset arvot myöhemmin-opetusohjelman.

Tässä opetusohjelmassa että Soita web-palvelu Windows PowerShellin avulla. Katso .NET C# otoksen [Analysoi reaaliaikainen Twitter markkinatunnelma kanssa HBase HDInsight][hdinsight-hbase-twitter-sentiment]. Muut Suositut työkalu web-palvelu soittamiseen on [*Curl*][curl]. Kääntö voi ladata [tätä][curl-download].

>[AZURE.NOTE] Kun käytät Windowsin kääntö-komentoa, käyttää lainausmerkkejä sijaan puolilainausmerkkeihin vaihtoehto arvoista.

**Saat tweets**

1. Avaa Windows PowerShellin integroitu komentosarjan ympäristön (ise:). (Windows 8: n Käynnistä näytössä, kirjoita **PowerShell_ISE** ja valitse sitten **Windows PowerShell ise:**. Katso [Windows PowerShellin Windows 8: ssa ja Windows][powershell-start].)

2. Kopioi seuraava komentosarja script-ruudussa.

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Määrittää ensimmäisen viiden-8 muuttujat komentosarja:


    Muuttujan|Kuvaus
    ---|---
    $clusterName|Tämä on HDInsight-klusterin kohtaa, johon haluat suorittaa sovelluksen nimen.
    $oauth_consumer_key|Tämä on teit aiemmin Twitter-sovellusta luotaessa Twitter sovelluksen **kuluttaja-näppäintä** .
    $oauth_consumer_secret|Tämä on teit aiemmin Twitter sovelluksen **kuluttaja salaisuus** .
    $oauth_token|Tämä on teit aiemmin Twitter sovelluksen **käyttöoikeustietue** .
    $oauth_token_secret|Tämä on teit aiemmin Twitter sovelluksen **access suojaustunnuksen salaisuus** .
    $destBlobName|Tämä on tulosteen blob-nimi. Oletusarvo on **tutorials/twitter/data/tweets.txt**. Jos muutat oletusarvoa, sinun on päivitettävä Windows PowerShell-komentosarjojen vastaavasti.
    $trackString|Web-palvelu palauttaa nämä avainsanat liittyvät tweets. Oletusarvo on **Azure, pilveen, Hdinsightista**. Jos muutat oletusarvoa, voit päivittää Windows PowerShell-komentosarjojen vastaavasti.
    $lineMax|Arvo määrittää, kuinka monta tweets komentosarja lukee. Kestää noin kolme minuuttia lukemaan 100 tweets. Voit määrittää on useita, mutta kestää kauemmin Lataa.

5. Painamalla **F5** komentosarjan suorittaminen. Jos käytössä ilmenee ongelmia, voit kiertää tämän ongelman valitsemalla kaikki rivit ja painamalla **F8-näppäintä**.
6. Näet "Valmis!" tulosteen lopussa. Virhesanomia näkyvät punaisena.

Kuin vahvistus menettely voit tarkistaa kohdetiedosto **/tutorials/twitter/data/tweets.txt**Azure tallennustilan explorer tai PowerShellin Azure Azure-Blob-tallennustilan. Tiedostojen luettelon otoksen Windows PowerShell-komentosarja, katso [Käytä Blob-objektien tallennustilaan kanssa HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>HiveQL komentosarjan luominen

Azure PowerShellin avulla voit suorittaa useita HiveQL lauseet yksi kerrallaan tai pakata HiveQL-lauseen komentosarja-tiedostoon. Tässä opetusohjelmassa luot HiveQL komentosarjan. Komentosarjatiedosto on ladattava Azure-Blob-säiliö. Seuraavan osion suoritetaan komentosarjatiedosto Azure PowerShell-toiminnolla.

>[AZURE.NOTE] Rakenne-komentosarjatiedosto ja 10 000 tweets sisältävä tiedosto on ladattu julkisen Blob-säilö. Voit ohittaa tämän osan, jos haluat käyttää ladattuja tiedostoja.

HiveQL komentosarja suorittaa seuraavasti:

1. **Pudota tweets_raw taulukon** tapauksessa taulukossa jo olemassa.
2. **Luo tweets_raw rakennetaulukko**. Tämän väliaikaisen rakenteen rakenteellisia taulukko sisältää tiedot edelleen Pura, muunna ja ladata käsittely (ETL). Lisätietoja osiot on artikkelissa [opetusohjelma rakenne][apache-hive-tutorial].  
3. **Lataa tiedot** lähde-kansiosta /tutorials/twitter/data. Suuri tweets dataset sisäkkäisiä JSON-muoto on nyt muuntaa tilapäinen rakenteen taulukon rakenteeseen.
3. **Pudota tweets taulukon** tapauksessa taulukossa jo olemassa.
4. **Luo tweets taulukon**. Ennen kuin voit tehdä kyselyn vastaan tweets tietojoukko käyttämällä rakenne, sinun täytyy suorittaa toinen ETL prosessi. Prosessia ETL määrittää tietoihin, jotka on tallennettu "twitter_raw" taulukon Lisää taulukko rakenne.  
5. **Lisää korvaa taulukko**. Tämä monimutkaisia rakenne-komentosarja aloituskokouksen pitkä MapReduce työt joukko Hadoop-klusterin mukaan. Oman tietojoukko ja yhteyttä klusterin koon mukaan tämä saattaa kestää noin 10 minuuttia.
6. **Lisää korvaa hakemisto**. Suorita kysely ja tulosteen tietojoukko tiedostoon. Tämä kysely palauttaa luettelon Twitter-käyttäjät, jotka lähetetään useimmat tweets, joka sisältää sanan "Azure".

**Voit luoda komentosarjan, rakenne ja lataa se Azure**

1. Avaa Windows PowerShell ise:.
2. Kopioi seuraava komentosarja script-ruudussa.

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
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
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Määritä ensin kahden muuttujan komentosarja:

    Muuttujan|Kuvaus
    ---|---
    $clusterName|Kirjoita HDInsight-klusterinimi, jossa haluat sovelluksen käyttämiseen.
    $subscriptionID|Kirjoita Azure tilauksen käyttämällä.
    $sourceDataPath|Kun rakenteen kyselyt luettu tiedot Azure-Blob-objektien tallennuspaikka. Ei tarvitse muuttaa muuttuja.
    $outputPath|Azure-Blob-tallennuspaikka missä rakenne-kyselyiden tulosteen tulokset. Ei tarvitse muuttaa muuttuja.
    $hqlScriptFile|Sijainti ja nimi HiveQL-komentosarjatiedosto Ei tarvitse muuttaa muuttuja.

5. Painamalla **F5** komentosarjan suorittaminen. Jos käytössä ilmenee ongelmia, voit kiertää tämän ongelman valitsemalla kaikki rivit ja painamalla **F8-näppäintä**.
6. Näet "Valmis!" tulosteen lopussa. Virhesanomia näkyvät punaisena.

Kuin vahvistus menettely voit tarkistaa kohdetiedosto **/tutorials/twitter/twitter.hql**Azure tallennustilan explorer tai PowerShellin Azure Azure-Blob-tallennustilan. Tiedostojen luettelon otoksen Windows PowerShell-komentosarja, katso [Käytä Blob-objektien tallennustilaan kanssa HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Prosessin Twitter-tietojen avulla rakenne

Olet tehnyt kaikki valmistelutyöt. Nyt voit käynnistää rakenteen komentosarjan ja Tarkista tulokset.

### <a name="submit-a-hive-job"></a>Lähetä rakenne-työ

Rakenne-komentosarjan suorittaminen seuraavaa Windows PowerShell-komentosarjaa avulla. Haluat määrittää ensimmäisen muuttuja.

>[AZURE.NOTE] Jos haluat käyttää tweets ja ladattujen HiveQL komentosarja kaksi osien, Määritä $hqlScriptFile "/ tutorials/twitter/twitter.hql". Jos haluat käyttää niistä, joita on ladattu julkisen Blob-objektien puolestasi, Määritä $hqlScriptFile "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Tarkista tulokset

Tarkistaa rakenteen työn tulosteen käyttämällä seuraavaa Windows PowerShell-komentosarjaa. Sinun on ensin kahden muuttujan määrittämiseen.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Rakenne-taulukon käyttää \001 kenttäerotin. Erottimen ei näy tulosteessa.

Azure-Blob-säiliö on sijoitettu analyysitulokset, kun voit Azure SQL-tietokannan tai SQL server tietojen vieminen, tietojen vieminen Exceliin Power Queryn avulla tai sovelluksen tietojen yhdistäminen rakenne ODBC-ohjain. Saat lisätietoja, [Käytä Sqoop kanssa HDInsight][hdinsight-use-sqoop], [Analysoi viive lentotietoihin käyttämällä HDInsight][hdinsight-analyze-flight-delay-data], [Hdinsightiin Power Queryn yhdistäminen Excel][hdinsight-power-query], ja [HDInsight Microsoft-rakenne-ODBC-ohjaimella yhdistäminen Excel][hdinsight-hive-odbc].

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa on tullut voit muuntaa rakenteeton JSON-tietojoukko rakenteellisia rakennetaulukko kyselyn, tutkimiseen ja analysoimiseen Twitter tiedot käyttämällä Azure Hdinsightista. Lisätietoja on artikkeleissa:

- [HDInsight käytön aloittaminen][hdinsight-get-started]
- [Analysoi reaaliaikainen Twitter-markkinatunnelma HBase HDInsight kanssa][hdinsight-hbase-twitter-sentiment]
- [Analysoi viive lentotietoihin käyttämällä Hdinsightiin][hdinsight-analyze-flight-delay-data]
- [Excelin yhdistäminen Hdinsightiin Power Queryn avulla][hdinsight-power-query]
- [Excelin yhdistäminen Microsoft rakenteen ODBC-ohjaimella Hdinsightiin][hdinsight-hive-odbc]
- [Sqoop käyttäminen Hdinsightiin][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
