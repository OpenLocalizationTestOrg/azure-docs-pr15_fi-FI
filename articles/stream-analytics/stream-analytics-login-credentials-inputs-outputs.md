<properties 
    pageTitle="Virta Analytics: Kierrä sisään tunnistetietojen syötteiden ja tulostaa | Microsoft Azure" 
    description="Opi päivittämään tunnistetietojen Stream Analytics-syötteiden ja tulostaa."
    keywords="kirjautumistiedot"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Kierrä kirjautumistietosi syötteiden ja tulostaa Stream Analytics töissä

##<a name="abstract"></a>Tiivistelmä
Azure Stream Analytics tänään ei salli korvaaminen tunnistetiedot i/o-työ on käynnissä.

Vaikka Azure Stream Analytics tue jatkaminen viimeisen tuloste työn, emme määrittää jakaa koko prosessin pienentäminen pysäyttäminen välinen viive työn käynnistäminen ja kiertäminen kirjautumisen tunnistetiedot.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Osa 1 – Valmistele uusi tunnistetietojoukko:
Tässä osassa on käytettävissä seuraavassa syötteiden ja tulostaa:

* Blob-objektien tallennustilaan
* Tapahtuman keskittimet
* SQL-tietokantaan
* Taulukkotallennus

Voit jatkaa muiden syötteiden/tulostus-, osa 2.

###<a name="blob-storagetable-storage"></a>BLOB storage/taulukkotallennus
1.  Tallennustilan tietty tunniste Siirry Azure hallinta-portaalissa:  
![graphic1][graphic1]
2.  Etsi työtäsi tallennustilaa ja siirry siihen:  
![graphic2][graphic2]
3.  Valitse Hallitse pikanäppäimet-komento:  
![graphic3][graphic3]
4.  Accessin perusavaimen ja toissijainen Access-näppäintä, **Valitse haluamasi ei käytetä työtäsi**.
5.  Luo viesti:  
![graphic4][graphic4]
6.  Kopioi juuri luotu avain.  
![graphic5][graphic5]
7.  Siirry osan 2.

###<a name="event-hubs"></a>Tapahtuman keskittimet
1.  Siirry Azure hallinta-portaalin palvelun Bus-tunniste:  
![graphic6][graphic6]
2.  Etsi Service-Bus Namespace työtäsi käyttämät ja siirry siihen:  
![graphic7][graphic7]
3.  Jos työsi käyttää jaettua käyttöoikeuskäytäntö palvelun Bus Namespace, siirry vaiheeseen 6  
4.  Avaa tapahtuma keskittimet-välilehti:  
![graphic8][graphic8]
5.  Etsi työtäsi käyttämä tapahtumaa-toiminnossa ja siirry siihen:  
![graphic9][graphic9]
6.  Siirry Määritä-välilehdessä:  
![graphic10][graphic10]
7.  Käytännön nimi avattavasta Etsi työtäsi käyttää jaettua Käyttöoikeuskäytäntö:  
![graphic11][graphic11]
8.  Perusavaimen ja toissijaisen avaimen, **Valitse haluamasi ei käytetä työtäsi**.  
9.  Luo viesti:  
![graphic12][graphic12]
10. Kopioi juuri luotu avain.  
![graphic13][graphic13]
11. Siirry osan 2.  

###<a name="sql-database"></a>SQL-tietokantaan

>[AZURE.NOTE] Huomautus: sinun on muodostaa yhteyden SQL-tietokanta-palvelu. Tarkastellaan näyttää, miten voit tehdä tämän hallintatoimintojen avulla Azure hallinta-portaalissa, mutta voi valita joitakin asiakkaan työkalun kuten SQL Server Management Studiossa sekä.

1.  SQL-tietokantoja tunniste Siirry Azure hallinta-portaalissa:  
![graphic14][graphic14]
2.  Etsi samalle riville työn ja **sitten palvelimessa** linkin käyttämän SQL-tietokantaan:  
![graphic15][graphic15]
3.  Valitse hallinta-komento:  
![graphic16][graphic16]
4.  Kirjoita tietokannan perustyyli:  
![graphic17][graphic17]
5.  Kirjoita käyttäjänimi ja salasana, ja valitse loki:  
![graphic18][graphic18]
6.  Valitse uusi kysely:  
![graphic19][graphic19]
7.  Kirjoita seuraava kysely < login_name > tilalle käyttäjänimi- ja korvaamisvaihtoehtoja <enterStrongPasswordHere> uudella salasanalla:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Valitse Suorita:  
![graphic20][graphic20]
9.  Palaa vaiheeseen 2 ja tämän ajan napsauttamalla tietokanta:  
![graphic21][graphic21]
10. Valitse hallinta-komento:  
![graphic22][graphic22]
11. Kirjoita käyttäjänimi ja salasana, ja valitse kirjautumisen:  
![graphic23][graphic23]
12. Valitse uusi kysely:  
![graphic24][graphic24]
13. Kirjoita seuraava kysely < käyttäjätunnus > tilalle nimi, jonka haluat määrittää tämän kirjautumisen yhteydessä tietokannan (Voit antaa annoit < login_name >, kuten sama arvo) ja < login_name > tilalle uusi käyttäjänimi:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Valitse Suorita:  
![graphic25][graphic25]
15. Pitäisi nyt antaa uudelle käyttäjälle samaa rooleista ja oikeuksista alkuperäinen käyttäjä.
16. Siirry osan 2.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Osa 2: Pysäyttäminen Stream Analytics-työ
1.  Siirry Azure hallinta-portaalin Stream Analytics-tunniste:  
![graphic26][graphic26]
2.  Etsi työtäsi ja siirry siihen:  
![graphic27][graphic27]
3.  Siirry syötteiden-välilehti tai perusteella, onko tunnistetiedot syöte tai tulos on kiertäminen tulostus-välilehti.  
![graphic28][graphic28]
4.  Lopeta-komento ja vahvista työn lakkasi:  
![graphic29][graphic29] työn lopettavat odota.
5.  Etsi haluat kiertää tunnistetiedot ja siirry siihen input/output:  
![graphic30][graphic30]
6.  Siirry osan 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Osa 3: Muokkaamisen Stream Analytics työn tunnistetiedot

###<a name="blob-storagetable-storage"></a>BLOB storage/taulukkotallennus
1.  Etsi tallennustilan Tiliavain Tiliavain-kenttään ja liitä se juuri luotu avain:  
![graphic31][graphic31]
2.  Napsauta Tallenna-komentoa ja vahvista muutosten tallentaminen:  
![graphic32][graphic32]
3.  Yhteyden testaaminen käynnistyy automaattisesti, kun tallennat muutokset, varmista, että se on läpäisseet.
4.  Siirry osa 4.

###<a name="event-hubs"></a>Tapahtuman keskittimet
1.  Tapahtuman keskittimeen käytännön avain-kenttä ja liitä se juuri luotu avain:  
![graphic33][graphic33]
2.  Napsauta Tallenna-komentoa ja vahvista muutosten tallentaminen:  
![graphic34][graphic34]
3.  Yhteyden testaaminen käynnistyy automaattisesti, kun tallennat muutokset, varmista, että se on ohitettu onnistuneesti.
4.  Siirry osa 4.

###<a name="power-bi"></a>Power BI
1.  Valitse uusi luvan:  
* ![graphic35][graphic35]
* Näyttöön tulee seuraava vahvistus:  
* ![graphic36][graphic36]
2.  Napsauta Tallenna-komentoa ja vahvista muutosten tallentaminen:  
![graphic37][graphic37]
3.  Yhteyden testaaminen käynnistyy automaattisesti, kun tallennat muutokset, varmista, että se on läpäisseet.
4.  Siirry osa 4.

###<a name="sql-database"></a>SQL-tietokantaan
1.  Etsi käyttäjänimi ja salasana-kentissä ja liitä ne uuden tunnistetietojoukko:  
![graphic38][graphic38]
2.  Napsauta Tallenna-komentoa ja vahvista muutosten tallentaminen:  
![graphic39][graphic39]
3.  Yhteyden testaaminen käynnistyy automaattisesti, kun tallennat muutokset, varmista, että se on ohitettu onnistuneesti.  
4.  Siirry osa 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Osa 4: Lähtien työtäsi pysäytetty viimeksi
1.  Siirry poispäin/i:  
![graphic40][graphic40]
2.  Napsauta Käynnistä-komennon:  
![graphic41][graphic41]
3.  Valitse pysäytetty viimeksi ja valitse OK:  
 ![graphic42][graphic42]
4.  Siirry osan 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Osa 5: Poistamalla vanhat tunnistetietojoukko
Tässä osassa on käytettävissä seuraavassa syötteiden ja tulostaa:
* Blob-objektien tallennustilaan
* Tapahtuman keskittimet
* SQL-tietokantaan
* Taulukkotallennus

###<a name="blob-storagetable-storage"></a>BLOB storage/taulukkotallennus
Toista osa 1, joka on työtäsi aiemmin käyttänyt uusi nyt käyttämättömät pikanäppäin pikanäppäin.

###<a name="event-hubs"></a>Tapahtuman keskittimet
Toista osa 1 avainta, joka on työtäsi aiemmin käyttänyt uusi nyt käyttämättömät avain.

###<a name="sql-database"></a>SQL-tietokantaan
1.  Siirry takaisin kyselyikkunan osan 1 vaiheessa 7 ja kirjoita seuraavan kyselyn, < previous_login_name > tilalle käyttäjänimi, jolla käytettiin aiemmin työtäsi:  
`DROP LOGIN <previous_login_name>`  
2.  Valitse Suorita:  
    ![graphic43][graphic43]  

Näyttöön tulee seuraava vahvistus: 

    Command(s) completed successfully.

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
