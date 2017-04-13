<properties 
    pageTitle="Suorittaa Python koneen learning komentosarjoja | Microsoft Azure" 
    description="Jäsennysten suunnitella periaatteiden pohjana tuki Python komentosarjojen Azure koneen oppiminen ja peruskäyttö skenaariot, ominaisuudet ja rajoitukset." 
    keywords="Python konepohjaisten oppimistekniikoiden, pandas, python pandas, python komentosarjoja suorittaa python komentosarjoja"
    services="machine-learning"
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Suorittaa Python koneen learning komentosarjoja Azure Konepohjaisten Oppimistekniikoiden Studiossa

Tässä ohjeaiheessa kerrotaan rakenne periaatteiden pohjana Python komentosarjat Azure koneen Learning nykyisen tuki. Tärkeimmät ominaisuudet ovat myös rajattu, mukaan lukien tuki tuominen aiemmin luotua koodia, vieminen visualisoinnit ja lopuksi rajoitukset ja meneillään olevan työn käsitellään.

[Python](https://www.python.org/) on paljon tietoja tutkijoiden tool-arkkuun välttämätöntä työkalussa. Se on:

-  Tyylikäs ja selkeä syntaksi 
-  Office kaikissa ympäristöissä tuki 
-  tehokas kirjastojen suuria kokoelma ja 
-  erääntyneiden Kehitystyökalut. 

Python parhaillaan käytetään kaikissa vaiheissa käytetään yleensä tietokoneen learning mallinnus työnkulun, tiedoista ingest ja käsittelyn ominaisuus rakentaminen ja mallin koulutus, ja valitse vahvistus ja käyttöönoton malleista. 

Azure Konepohjaisten Oppimistekniikoiden Studio tukee upottamisen Python komentosarjojen koneen Koulujen kokeessa ja julkaista ne myös saumattomasti skaalattava, operationalized web services Microsoft Azure eri osiin.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Suunnittele Python komentosarjat tietokoneen Learning periaatteet
Ensisijainen liitännän Python Azure koneen Learning Studiossa on [Python komentosarjan suorittaminen] [ execute-python-script] moduulin kuvassa 1.

![image1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Kuva 1. **Suorita Python komentosarja** -moduuli.

[Suorita Python komentosarjan] [ execute-python-script] moduulin hyväksyy enintään kolme syötteiden ja tuottaa R sen analogia [Suorittaa R Script] tavoin kaksi-tulostus (kuvattu alla),[ execute-r-script] moduuli. Python-koodi, joka suoritetaan yksikkö kuin erityisesti nimetty parametri-ruutuun aloituskohtaa funktiota `azureml_main`. Seuraavassa on käyttää toteuttamaan tämän moduulin tärkeimmät periaatteet:

1.  *On oltava idiomatic Python käyttäjille.* Useimmat Python käyttäjät huomioon niiden koodilla toimintojen sisällä moduulit suoritettavan lauseet paljon sijoittaminen ylimmän tason moduuli on suhteellisen harvoin. Tämän vuoksi komentosarja-ruutu tulee myös erityisesti nimetty Python-funktion eikä vain lauseet järjestyksessä. Funktion tarjoamia objektit on vakio Python kirjaston tyyppiä kuten [Pandas](http://pandas.pydata.org/) kehykset ja [NumPy](http://www.numpy.org/) matriisin.
2.  *Täytyy olla erittäin tarkan paikallinen välillä ja cloud suorituskerran.* Käyttää Python koodin Taustajärjestelmä perustuu [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1, yleisesti käytetty Office kaikissa ympäristöissä tieteellisen Python-jakauman. Se sisältyy lähellä 200 yleisimmät Python pakettien. Vuoksi tietojen tutkijoiden voit korjata ja oikeutettu niiden koodilla, niiden paikallisen Azure koneen Learning yhteensopivan Anaconda-ympäristöön. Suorita sen osana Azure koneen Learning kokeen luotettavan aiemmin kehittäminen-ympäristössä, kuten [IPython](http://ipython.org/) muistikirjan tai [Python Tools for Visual Studio](http://aka.ms/ptvs) avulla. Lisäksi `azureml_main` aloituskohtaa merkkiaine Python funktio ja olla kirjoitti ilman Azure koneen Learning koodin tai SDK asennettuna.
3.  *On oltava saumattomasti composable muiden Azure koneen Learning moduulit.* [Suorita Python komentosarjan] [ execute-python-script] moduulin hyväksyy-syötteiden ja tulostaa, vakio Azure koneen Learning tietojoukkoja. Pohjana framework läpinäkyvä ja tehokkaasti avaintekijä (tukevat ominaisuudet, kuten puuttuvia arvoja) Azure koneen oppiminen ja Python CRT Runtime. Python vuoksi voidaan käyttää yhdessä aiemmin Azure koneen Learning työnkulkujen, mukaan lukien, jotka kutsuvat R ja SQLite kanssa. Yksi vuoksi tehdään työnkulkuja, jotka:
  * Käytä Python ja Pandas esikäsittely ja puhdistus-tiedot 
  * syötteen tietoja SQL-muunnos, useita tietojoukkoja liittäminen lomakkeen ominaisuudet 
  * mallien algoritmit laajan valikoiman käyttäminen Azure koneen Learning kouluttaminen ja 
  * Arvioi ja käsitellä jälkeinen tulokset käyttämällä R.


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Peruskäyttö skenaariot koneen Learning Python komentosarjojen
Tässä osassa on kyselyn joitakin [Python komentosarjan suorittaminen] basic käyttää[ execute-python-script] moduuli.
Kuten edellä mainittiin, minkä tahansa Python moduulin syötteiden niitä julkaista Pandas kehykset. Lisätietoja Python Pandas ja kuinka sen avulla voidaan käsitellä tietoja helposti ja tehokkaasti löytyy *Python tietojen analysointiin* (O'Reilly 2012) W. McKinney. Funktion on palautettava Pandas tietojen kehyksen pakattu sisältyy esimerkiksi monikon, luettelo tai taulukko NumPy Python [järjestyksessä](https://docs.python.org/2/c-api/sequence.html) . Tässä järjestyksessä ensimmäinen elementti palautetaan sitten ensimmäisen output portti, moduulin. Tämä malli näkyy kuva 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Kuva 2. Kartoitus Lisää parametrit-portit ja palautusarvo tulostus-porttiin.

Yksityiskohtaisempia semantiikkaan liittyvien, miten syötteen portit yhdistetään parametrit `azureml_main` funktion näkyvät taulukko 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Taulukko 1. Funktioparametrit syötteen portit yhdistämismääritys.

Yhdistämisen syötteen portit ja funktioparametrit on sijainti. Ensimmäisen yhdistetyn syötteen portin on nyt yhdistetty-funktion ensimmäinen parametri ja toinen syöte (Jos yhdistettynä) on nyt yhdistetty toisen parametrin funktion.

## <a name="translation-of-input-and-output-types"></a>Käännöksen syöttö- ja tyypit
Aiemmin kuvatulla syötteen tietojoukkoja-Azure koneen Learning muunnetaan tietojen Pandas kehykset ja tulosteen kehykset muunnetaan takaisin Azure koneen Learning tietojoukkoja. Seuraavat muunnokset suoritetaan:

1.  Merkkijono ja numeeristen sarakkeiden muuntuvat-on ja DataSet puuttuvat arvot muunnetaan tyhjiksi Pandas "Puuttuu" arvoja. Saman muuntaminen tapahtuu siitä, miten takaisin (Pandas puuttuu arvot muunnetaan Azure koneen Learning puuttuvat arvot).
2.  Indeksi-Pandas vektorit ei tue Azure koneen Learning. Kaikki syöttötiedot kehykset Python-funktiossa on aina 64-bittinen numeerisia indeksi väliltä 0 – 1 rivien määrä. 
3.  Azure koneen Learning tietojoukkoja ei voi olla sarakkeiden nimet ja sarakkeiden nimet, jotka eivät ole merkkijonoja. Jos tulos-tietojen kehyksen on muu kuin numeerinen sarakkeita, puitteissa soittaa `str` -sarakkeiden nimet. Kaikki sarakkeiden nimet ovat vastaavasti automaattisesti mangled voit varmistaa, nimet ovat yksilöllisiä. Liitteen (2) lisätään ensimmäisen kaksoiskappale, (3) ja toinen kaksoiskappaleet, jne.

## <a name="operationalizing-python-scripts"></a>Operationalizing Python komentosarjat
[Suorita Python komentosarjan] [ execute-python-script] käytetään tulosmalli kokeessa moduulit nimistä, kun julkaistaan verkkopalvelun. Kuten kuvassa 3 näkyy tulosmalli koe, sisältävän yksittäisen Python lausekkeen koodi. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Kuva 3. WWW-palvelun arvioinnissa Python lauseke.

Tämän kokeen luotu verkkopalvelun kestää kuin Lisää Python-lauseke (merkkijonona), lähettää sen Python kääntäjän ja palauttaa taulukon, joka sisältää lausekkeen ja lasketun tuloksen.

## <a name="importing-existing-python-script-modules"></a>Olemassa olevan Python komentosarjan moduulit tuominen
Yleiset-Käyttötapaus monta tietojen tutkijoiden varten on voi liittää aiemmin Python komentosarjat Azure koneen Learning kokeissa. Ketjuttaa ja kaikki koodi liittäminen yksittäinen komentosarja-ruutuun [Suorita Python komentosarjan] sijaan[ execute-python-script] moduulin hyväksyy kolmannen syötteen portin, johon voidaan yhdistää Python moduulit sisältävä zip-tiedosto. Tiedoston sitten purettuna mukaan suorittamisen puitteissa suorituksen ja sisältö lisätään kirjastoon polun kääntäjän Python. `azureml_main` Aloituskohtaa funktio voi tuoda näitä moduuleja suoraan.

Esimerkkinä harkitse tiedoston Hello.py sisältävä yksinkertainen "Hei, maailman"-toimintoa.

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Kuva 4. Käyttäjän määrittämä funktio.

Seuraavaksi luodaan tiedosto, joka sisältää Hello.py Hello.zip:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Kuva 5. Käyttäjän määrittämä Python koodi sisältävä zip-tiedosto.

Lataa tämä tietojoukko kuin Azure koneen Learning Studio kyselyjä. Luoda ja suorittaa yksinkertaisen koe, joka käyttää Hello.zip tiedoston Python koodin liittämällä kolmannen syötteen porttiin komentosarjan Python suorittaa tässä kuvassa osoitetulla tavalla.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Kuva 6. Käyttäjän määrittämä Python koodi otoksen kokeile ladata zip-tiedostona.

Moduulin tulosteen näyttää zip-tiedosto on jo pakkaamattomat ja -funktion `print_hello` todellakin on suoritettu.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Kuva 7. Käyttäjän määrittämien funktioiden [Suorittaminen Python komentosarjan] sisällä käytössä[ execute-python-script] moduuli.

## <a name="working-with-visualizations"></a>Visualisointien käsitteleminen
Joudutaan luodut MatplotLib, joka voidaan näyttää selaimessa voi palauttaa [Python komentosarjan suorittaminen][execute-python-script]. Mutta joudutaan uudelleenohjaus ei ole automaattisesti kuvat kuin käytettäessä R. Niin käyttäjä on erikseen tallentaa minkä tahansa joudutaan PNG-tiedostoja jos ne ovat palautetaan takaisin Azure koneen Learning. 

Jotta voit luoda MatplotLib kuvia, on kilpailemaan seuraavasti:

* "AGG" taustaan siirtyminen oletusarvon Qt-pohjainen muunnin 
* Luo uusi kuva-objekti 
* Hae akselia ja luo kaikki joudutaan siihen 
* Tallenna luku PNG-tiedostoon 

Tämä toimenpide on esitetty seuraavassa kuvassa 8, joka luo pistekaaviot piirron matriisin Pandas scatter_matrix-toiminnon avulla.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Kuva 8. Kuvien tallentaminen MatplotLib luvut.



Kuva 9 näyttää, joka käyttää komentosarjaa osoitetulla palauttaa kokeen pisteitä toisen tulostusportin kautta.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Kuva 9. Kesäolympialaisten visualisointi joudutaan luotu Python koodi.

On mahdollista palauttaa useita luvut tallentamalla ne eri kuviksi Azure koneen Learning runtime vastataan kaikki kuvat, ja yhdistää ne visualisoinnille.


## <a name="advanced-examples"></a>Lisäasetukset-esimerkkejä
Asennettu Azure Konepohjaisten Oppimistekniikoiden Anaconda-ympäristössä on yleisiä pakettien, kuten NumPy, SciPy, ja Scikits Lue sekä nämä tehokkaasti voidaan eri tietojen käsittely tehtävien tyypillinen tietokoneen learning putkijohto. Esimerkkinä seuraavat kokeessa ja komentosarja on kuvattu ensemble oppijat-Scikits tietoja laskemiseen tietojoukko ominaisuus tärkeys pisteet käyttö. Tulokset voidaan suorittaa valvonnan alaisena ominaisuudet ennen käyttöä toisen tietokoneen learning malliin.

Laskemiseen tärkeys tulosten ja tilauksen perusteella sen ominaisuuksia Python-funktio on alla:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Kuva 10. Toimi arvon.mukaan-ominaisuuksien tulosten mukaan.
 Seuraavat kokeen laskee sitten ja palauttaa ominaisuuksien tärkeys-tulosten Azure koneen Learning "Pima Intian Diabetes"-tietojoukko:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Kuva 11. Kokeile arvon.mukaan-ominaisuuksien Pima Intian Diabetes tietojoukossa.

## <a name="limitations"></a>Rajoitukset 
[Suorita Python komentosarjan] [ execute-python-script] on tällä hetkellä seuraavat rajoitukset:

1.  *Eristetyn suorittamisen.* Python runtime on tällä hetkellä eristetyn ja tuloksena ei salli verkon tai paikallisen tiedostojärjestelmän pysyvä tavalla. Kaikki tiedostot, jotka on tallennettu paikallisesti eristää ja poistaa, kun moduuli on valmis. Python-koodia ei voi käyttää useimmissa kansioita tietokoneessa, se suoritetaan, nykyisen hakemiston ja sen alihakemistoista poikkeuksen.
2.  *Puuttuminen kehittyneitä kehittäminen ja virheenkorjaus tuki.* Python-moduulin ei tällä hetkellä tue IDE-ominaisuuksia, kuten intellisense ja virheiden. Myös, jos moduulin epäonnistuu suorituksen aikana, koko Python pinon jäljitys on käytettävissä, mutta on voi tarkastella moduulin lokiin. On suositeltavaa tällä hetkellä kehittää ja korjata niiden Python komentosarjoja ympäristössä, kuten IPython ja tuoda sitten moduulin koodi.
3.  *Yhtä kehyksen tulos.* Python aloituskohtaa on sallittu vain palauttaa tulosteen kuin yhtä kehykseksi. Ei ole tällä hetkellä mahdollista voit palata takaisin Azure koneen Learning runtime haluamaansa Python-objekteja, kuten suoraan koulutetun mallit. [Suorita R komentosarjan], kuten[execute-r-script], joka on saman rajoitus, se on kuitenkin mahdollista monissa tapauksissa pickle objekteihin DBCS-taulukko ja sitten return, tietojen kehyksen sisäpuolelle.
4.  *Ei voi mukauttaa Python asennusta*. Tällä hetkellä kautta edempänä zip-tiedoston järjestelmä on ainoa tapa lisätä mukautetun Python moduuleja. Kun tämä on mahdollista pieni moduulit, on hankalaa suuri moduulit (varsinkin alkuperäisen DLL: n kanssa) tai moduuleja suuri määrä. 


##<a name="conclusions"></a>Päätelmien
[Suorita Python komentosarjan] [ execute-python-script] moduulin mahdollistaa tietojen Tiedemies voi liittää aiemmin luotua Python koodia cloud isännöimä koneen learning työnkulut Azure Konepohjaisten Oppimistekniikoiden ja operationalize saumattomasti WWW-palvelun osana. Python komentosarja-moduulin käytetään yhdessä muiden Azure koneen Learning moduulit luonnollisesti ja avulla voidaan tehtävät tietojen tarkasteluun vanhat käsittelyn-ominaisuus poiminnan arviointi ja jälkeinen käsittelyn. Suorittamisen käytettävän taustatietokannan runtime perustuu Anaconda, hyvin testattu ja yleisesti käytetty Python-jakauman. Näin yksinkertainen puolestasi junassa aiemmin luotu koodi vastaavien pilveen kyselyjä.

Odotamme lisätoimintoja tarjota [Python komentosarjan suorittaminen] [ execute-python-script] moduuli, kuten mahdollisuus kouluttaminen ja operationalize Python mallit ja lisätä kehittäminen ja Azure koneen Learning Studiossa koodin virheenkorjaus parempi tuki.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Python Developer Center](/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
