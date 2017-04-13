<properties 
   pageTitle="Aloittaminen järvi tietovaraston käyttämällä REST API | Microsoft Azure" 
   description="Suorita järvi tietovaraston WebHDFS REST API avulla" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Azure Lake Tietosäilölle käyttämällä REST API käytön aloittaminen

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShellin](data-lake-store-get-started-powershell.md)
- [.NET SDK-PAKETISSA](data-lake-store-get-started-net-sdk.md)
- [Java SDK-paketissa](data-lake-store-get-started-java-sdk.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Tässä artikkelissa kerrotaan WebHDFS REST API ja tietojen järvi kaupan REST API käyttäminen suorittamaan tilinhallinta sekä Azure järvi tietovaraston tiedostojärjestelmän toimintoja. Azure järvi tietovaraston paljastaa omassa REST API tilin hallinta toimintoja varten. Kuitenkin järvi tietosäilö on yhteensopiva HDFS ja Hadoop ekosysteemiin, koska se tukee käyttämällä WebHDFS REST API tiedostojärjestelmän toimintoja varten.

>[AZURE.NOTE] Lisätietoja REST-Ohjelmointirajapinnalla Lake tietovaraston tuki on artikkelissa [Azure tietojen järvi kaupan REST API-viittaus](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Edellytykset

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure Active Directory-sovelluksen luominen**. Azure AD-sovelluksen avulla voit todentaa Azure AD järvi tietosäilö-sovellukseen. Useilla eri tavoilla Azure AD-todentamismenetelmä on **käyttäjän todennusta** tai **palvelu todennusta**. Ohjeita ja lisätietoja tarkistamiseen on artikkelissa [tietojen järvi kaupan Tarkista Azure Active Directoryn avulla](data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). Tässä artikkelissa käytetään kääntö miten REST API-puheluissa vastaan järvi tietosäilö-tili.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Miten Azure Active Directoryn avulla todennetaan?

Voit käyttää kahta tavoista tarkistamiseen Azure Active Directoryn avulla.

### <a name="end-user-authentication-interactive"></a>Käyttäjän todennusta (vuorovaikutteinen)

Tässä skenaariossa sovelluksen käyttäjää kirjautumaan ja kaikki toiminnot suoritetaan käyttäjän kontekstissa. Seuraavien toimien vuorovaikutteinen todennusta varten.

1. Sovelluksen kautta uudelleenohjata käyttäjän seuraava URL-osoite:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT URI > on koodattu URL-käyttöä varten. Tällöin https://localhost, käytä `https%3A%2F%2Flocalhost`)

    Tässä opetusohjelmassa varten URL-Osoitteen paikkamerkki-arvojen korvaaminen ja liitä se WWW-selaimen osoiterivillä. Sinut ohjataan todennuksen Azure kirjautumisnimi. Kun kirjaudut onnistuneesti, vastaus näkyy selaimen osoiteriville. Vastaus on seuraavanlainen:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Sieppaa vastauksen luvan koodin. Tässä opetusohjelmassa, voit kopioida luvan koodin selaimen osoiterivillä ja välittää sen JULKAISE-pyynnössä suojaustunnuksen päätepisteen alla kuvatulla tavalla:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] Tässä tapauksessa \<UUDELLEENOHJAUS URI > ei tarvitse koodattu.

3. Vastaus on JSON-objekti, joka sisältää access-tunnus (esimerkiksi `"access_token": "<ACCESS_TOKEN>"`) ja Päivitä-tunnus (esimerkiksi `"refresh_token": "<REFRESH_TOKEN>"`). Sovelluksen käyttää access-tunnuksen käytettäessä Azure tietojen järvi Tallenna ja Päivitä-tunnuksen hankkiminen toiseen käyttöoikeustietue, kun access-tunniste vanhentuu.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Kun access-tunnuksen vanhenee, voit pyytää uuden käyttöoikeustietue, Päivitä-tunnuksen käyttämällä alla kuvatulla tavalla:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Katso lisätietoja vuorovaikutteinen käyttöoikeuksista, [luvan koodin myöntää työnkulku](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Palvelun todennus (ei ole vuorovaikutteinen)

Tässä skenaariossa sovellus on oma tunnistetiedot toimintojen suorittaminen. Tähän on annettava alla kuvatun kaltaisia POST-pyynnön. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Pyyntö tulos sisältää todennus-tunnuksen (merkitty `access-token` tuloksissa alla), voit myöhemmin siirtää REST API soitettaessa. Todennus-tunnuksen tallennetaan tekstitiedostoon; tarvitset tätä tämän artikkelin.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tässä artikkelissa käyttää **ei ole vuorovaikutteinen** lähestymistapa. Lisätietoja ei ole vuorovaikutteinen (palvelu puhelut) on artikkelissa [palvelun palvelun puhelut-tunnistetiedoilla](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Tietosäilö järvi-tilin luominen

Tämä toiminto perustuu REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](https://msdn.microsoft.com/library/mt694078.aspx).

Käytä seuraavaa komentoa kääntö. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Yllä olevan komennon korvaa \< `REDACTED` \> luvan tunnuksen kanssa voit hakea aiemmassa versiossa. **Input.json** -tiedosto, joka on tarkoitettu sisältyy tämän komennon pyynnön-paketti `-d` parametrin yläpuolella. Input.json-tiedoston sisällön näyttää seuraavankaltaiselta:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Kansioiden luominen järvi tietosäilö-tili

Tämä toiminto perustuu WebHDFS REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Käytä seuraavaa komentoa kääntö. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Yllä olevan komennon korvaa \< `REDACTED` \> luvan tunnuksen kanssa voit hakea aiemmassa versiossa. Tämä komento luo kansio nimeltä **mytempdir** järvi tietosäilö-tilillesi juurikansiota-kohdassa.

Vastauksen tältä pitäisi näkyä, jos toiminto onnistuu:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Luettelon kansiot järvi tietovaraston tilillä

Tämä toiminto perustuu WebHDFS REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Käytä seuraavaa komentoa kääntö. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Yllä olevan komennon korvaa \< `REDACTED` \> luvan tunnuksen kanssa voit hakea aiemmassa versiossa.

Vastauksen tältä pitäisi näkyä, jos toiminto onnistuu:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Tietojen lataaminen järvi tietosäilö-tilille

Tämä toiminto perustuu WebHDFS REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

WebHDFS REST-Ohjelmointirajapinnan käyttäminen tietojen lataaminen on kaksivaiheinen prosessi, kuten alla on esitetty.

1. Lähetä pyyntö HTTP valitseminen lähettämättä ladattava tiedosto-tiedot. Korvaa seuraava komento ** \<yourstorename >** Lake tietovaraston nimesi.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Tämä komento tulosteen voi sisältää tilapäinen Uudelleenohjaus URL-osoite, kuten alla kuvattua.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. On nyt lähettää toiseen HTTP valitseminen pyynnön, vastaan vastauksen **sijainti** -ominaisuuden URL-Osoitteesta. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Tulos on seuraavanlainen:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Lue tietoja järvi tietovaraston tililtä

Tämä toiminto perustuu WebHDFS REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Tietojen lukeminen järvi Storesta tili on kaksivaiheinen prosessi.

* Voit lähettää ensin vastaan päätepisteen GET-pyynnössä `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Tämä palauttaa sijainti, Lähetä seuraava GET-pyyntö.
* Voit lähettää sitten GET-pyynnössä vastaan päätepisteen `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Tässä näkyvät tiedoston sisällön.

Koska ei ole syöttöparametrien ensimmäinen ja toinen vaihe välinen ero, voit käyttää `-L` Lähetä ensimmäinen parametri. `-L`vaihtoehto yhdistää kaksi pyynnöt jokin käytännössä ja saat kääntö uuteen sijaintiin pyynnön tekeminen uudelleen. Lopuksi pyynnön puhelut tulosteen näkyy, kuten alla olevan kuvan. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Näyttöön tulee tulos seuraavankaltaiselta:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Tietosäilö järvi tilillä tiedoston nimeäminen uudelleen

Tämä toiminto perustuu WebHDFS REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Seuraavat kääntö-komennon avulla voit nimetä tiedoston uudelleen. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Näyttöön tulee tulos seuraavankaltaiselta:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Tiedoston poistaminen järvi tietovaraston tililtä

Tämä toiminto perustuu WebHDFS REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Seuraavat kääntö-komennon avulla voit poistaa tiedoston. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Pitäisi näkyä tulos seuraavalla tavalla:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Poista järvi tietosäilö-tili

Tämä toiminto perustuu REST-Ohjelmointirajapinnalla määritetty puhelun [tähän](https://msdn.microsoft.com/library/mt694075.aspx).

Seuraavat kääntö-komennon avulla voit poistaa järvi tietovaraston tilin. Korvaa ** \<yourstorename >** Lake tietovaraston nimesi.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Pitäisi näkyä tulos seuraavalla tavalla:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Katso myös

- [Avaa Azure järvi säilön kanssa yhteensopivista suuri lähdetiedot-sovelluksista](data-lake-store-compatible-oss-other-applications.md)
 
