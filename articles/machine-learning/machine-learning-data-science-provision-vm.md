<properties 
    pageTitle="Valmistele Microsoft Data tiede virtuaalikoneen | Microsoft Azure" 
    description="Määritä ja luominen tietojen tiede Virtual koneen Azure analytics ja tietokoneen learning." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="bradsev" />


# <a name="provision-the-microsoft-data-science-virtual-machine"></a>Microsoft Data tiede virtuaalikoneen valmistelu

Microsoft tiede virtuaalikoneen on valmiiksi asennettu ja määritetty Suositut työkaluja tietojen analytics ja tietokoneen learning yleisimmin käytettyjä Azure virtuaalikoneen (AM) kuva. Työkaluja ovat seuraavat:

- Microsoft R Server Developer Edition
- Anaconda Python jakelu
- Jupyter muistikirjasta (ja R-Python ytimet)
- Visual Studio yhteisön-versioon
- Power BI Desktopin
- SQL Server 2016 Developer Edition
- Koneen learning Työkalut
    - [Laskennallinen verkon työkalujen (CNTK)](https://github.com/Microsoft/CNTK): syvä, kannattaa tutustua ohjelmiston työkalujen Microsoft Researchin kohteesta.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): nopea konepohjaisten oppimistekniikoiden järjestelmän tukevat esimerkiksi online, hajautusfunktiota, allreduce, alennuksia, learning2search aktivointi tekniikoita ja vuorovaikutteisesti.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): antamisen nopeiden ja tarkkojen tehosti puun käyttöönoton työkalu.
    - [Rattle](http://rattle.togaware.com/) (R Analytical työkalua, katso helposti): työkalua, joka mahdollistaa tietojen analyysin ja konepohjaisten Koulujen R helposti Graafisen-pohjaisten tietojen tarkasteluun ja mallinnus automaattinen R-koodin luominen ja käytön aloittaminen.
    - [mxnet](https://github.com/dmlc/mxnet): suunniteltu tehokkuuden ja joustavuutta laaja learning kehys
- Kirjastojen R ja Python varten käyttäminen Azure koneen oppiminen ja muut Azure palvelut
- Git, mukaan lukien Git Bash lähde koodin säilöjen tietoihin mukaan lukien GitHub, Visual Studio Team Services-käyttöä varten
- Windows-portit useita Suositut Linux komentorivin apuohjelmia (mukaan lukien awk, sed, perl, grep, Etsi, wget, kääntö jne) kautta komentokehote. 


Suorittamalla tietojen tiede liittyy Iterointi Valitse sarja, tehtävät: etsiminen lastaus ja valmiiksi käsittely tietojen luomisesta ja testaamalla mallit ja käyttöönotto kulutus älykkäät sovelluksissa mallit. Tietoja tutkijoiden käyttää erilaisia työkaluja näiden tehtävien suorittamiseen. Voi olla aivan aikaa, Etsi haluamasi versiot-ohjelmiston ja lataa ja asenna ne. Microsoft tiede virtuaalikoneen voi helpottaa Tätä rasitetta antamalla valmis avulla kuvan, joka on valmisteltu Azure kaikki useita Suositut työkaluja valmiiksi asennettu ja määritetty. 

Microsoft tiede virtuaalikoneen jump-starts analytics projektin. Sen avulla tehtäviä, kuten R, Python, SQL ja C# eri kielillä. Visual Studio on IDESTÄ kehittää ja testata koodi, joka on helppo käyttää. Azure-SDK AM sisältyvät avulla voit muodostaa sovellustesi eri Services-palvelujen avulla Microsoft cloud-ympäristössä. 

Veloituksia ei ole ohjelmiston tietojen tiede AM kuvan. Maksat vain Azure käyttömaksut mitä määräytyy voit valmistella virtuaalikoneen kokoa. Lisätietoja Laske maksut löytyy [tietojen tiede virtuaalikoneen](https://azure.microsoft.com/marketplace/partners/microsoft-ads/standard-data-science-vm/) sivulla hinnoittelu tiedot-osassa. 


## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit luoda Microsoft tietojen tiede Virtual Machine, sinulla on oltava seuraavasti:

- **Azure tilaus**: hankkia-kohdassa [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

*   **Azure-tallennustilan tilin**: Voit luoda luettelon artikkelissa [Azure-tallennustilan tilin luominen](storage-create-storage-account.md#create-a-storage-account). Voit myös tallennustilan tilin voi luoda osana luominen AM, jos et halua käyttää aiemmin luotu tili.


## <a name="create-your-microsoft-data-science-virtual-machine"></a>Luo Microsoft Data tiede virtuaalikoneen

Näin voit luoda esiintymää, Microsoft tietojen tiede virtuaalikoneen:

1.  Siirry [Azure](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm)-portaalissa luettelo virtuaalikoneen.
2.   Valitse ohjatun toiminnon huomioon otettavat alaosassa **Luo** -painiketta. ![määrittäminen-tiedot-tiede-AM](./media/machine-learning-data-science-provision-vm/configure-data-science-virtual-machine.png)
3.   Ohjatun toiminnon avulla luodaan Microsoft tiede virtuaalikoneen edellyttää **syötteiden** kunkin **viiden vaiheen** luetteloitu oikeassa laidassa olevasta Tässä kuvassa. Seuraavassa on syötteiden tarvittavat kunkin seuraavasti:
    
    1.   **Perusteet**
        1.   **Nimi**: luot tietojen tiede palvelimen nimi.
        2.   **Käyttäjänimi**: järjestelmänvalvojan tilin käyttäjätunnus.
        3.   **Salasana**: järjestelmänvalvojan tilin salasana.
        4.   **Tilaus**: Jos sinulla on useita tilauksia, valitse jokin, tietokoneessa on luotu ja laskutettu.
        5.   **Resurssiryhmä**: Voit luoda uuden tai Käytä aiemmin luotua ryhmää.
        6.   **Sijainti**: Valitse tietokeskuksen, joka sopii parhaiten. Se on yleensä tietokeskuksen, joka on yleensä tietojen tai lähinnä nopein verkkokäyttö fyysinen sijaintisi.
         
    2.   **Kokoa**: jompikumpi Palvelintyypit, joka täyttää toiminnallisten vaatimusten ja kustannukset rajoitukset. Saat lisää vaihtoehtoja AM kokoisille valitsemalla "Näytä kaikki".
    
    3.   **Asetukset**:
        1.   **Levyn tyyppi**: Valitse Premium halutessasi SSD asema (Suoritettaessa) muita valita "Vakio".
        2.   **Tallennustilan tilin**: Voit luoda uuden Azure-tallennustilan tilin tilauksen tai Käytä aiemmin luotua samaan *sijaintiin* , joka on valittu **perusteet** ohjatun toiminnon vaiheessa.
        3.   **Muut parametrit**: yleensä vain käyttää oletusarvoja. Tiedoksi linkki tiettyjen kenttien voit päälle, niin, sinun kannattaa harkita-oletusarvojen käyttö.

    4.   **Yhteenveto**: Tarkista, että olet kirjoittanut kaikki tiedot ovat oikein.
    5.   **Ostaminen**: Valitse Käynnistä valmistelun **Osta** . Linkki on tapahtuman ehdot. AM ei ole muita kulut lisäksi Laske **koon** vaiheessa valitsemasi palvelimen koon. 


>[AZURE.NOTE] Valmistelun olisi kestää noin 10 – 20 minuutin ajan. Valmistelun tila näkyy Azure-portaalissa.

## <a name="how-to-access-the-microsoft-data-science-virtual-machine"></a>Voit käyttää Microsoft tiede virtuaalikoneen

Kun AM on luotu, voit etätyöpöydän siihen käyttämällä järjestelmänvalvojan tilin tunnistetiedot, jonka määritit **perusteet** edellisen osion. 

Oman AM luodaan ja valmistelun yhteydessä, kun olet valmis aloittamaan käyttämällä työkaluja, joilla on asentanut ja määrittänyt sen. On Käynnistä-valikosta ruudut ja työpöydän kuvakkeiden useita työkaluja. 

## <a name="how-to-create-a-strong-password-on-the-jupyter-notebook-server"></a>Vahvan salasanan luominen Jupyter muistikirjan palvelimessa 

Jos haluat luoda oman vahva salasana Jupyter muistikirjan palvelimen asennettu tietokoneeseen, suorita seuraava komento komentoriviltä--tietojen tiede virtuaalikoneen.

    c:\anaconda\python.exe -c "import IPython;print IPython.lib.passwd()"

Valitse pyydettäessä vahva salasana.

Näet salasanan hash muodossa "sha1:xxxxxx" tulosteessa. Kopioi tämä salasana hash ja korvaa olemassa oleva muistikirja config tiedosto sijaitsee hash: **C:\ProgramData\jupyter\jupyter_notebook_config.py** parametrin nimi ***c.NotebookApp.password***kanssa.

Korvaa vain olemassa olevaan arvoon hash lainausmerkkejä sisältyvä (xxxxxx)-osa. Tarjoukset ja ***sha1:*** etuliite molemmat on säilytettävä parametriarvo.

Lopuksi sinun on käynnistettävä uudelleen, Jupyter palvelimessa, jossa on käytössä AM kuin windows ajoitettu tehtävä, jota kutsutaan **Start_IPython_Notebook**. Jos uusi salasana ei hyväksytä tämän tehtävän uudelleen, kokeile lopettamiseen kaikki käynnissä olevat python prosessit Tehtävienhallinta- ja Käynnistä ajoitettu tehtävä. Vaihtoehtoisesti voit kokeilla uudelleenkäynnistyksen virtuaalikoneen.

## <a name="tools-installed-on-the-microsoft-data-science-virtual-machine"></a>Valitse Microsoft tietojen tiede virtuaalikoneen-Työkalut

### <a name="microsoft-r-server-developer-edition"></a>Microsoft R Server Developer Edition
Jos haluat käyttää R oman analytics AM on Microsoft R Server Developer-versio. Microsoft R on laajasti esikunnista yritysluokan analytics käyttöympäristön mukaan R, jota tuetaan, skaalattava ja turvallinen. Tukemaan erilaisia big datasta tilastotiedot, ennakoivan mallinnus ja konepohjaisten oppimistekniikoiden ominaisuuksia, R-palvelin tukee monia analytics – tarkasteluun, analyysi, visualisointi ja mallinnus. Avaa lähde R laajentaminen ja käyttämällä Microsoft R Server on täysin yhteensopiva R komentosarjoja, toiminnot ja CRAN pakettien yrityksen tasolla tietojen analysointia varten. Se myös korjaa Avaa lähde R ladatun rajoituksista lisäämällä rinnakkais- ja artikkeli tietojen käsittely. Näin voit suorittaa tietojen analytics paljon suurempi kuin mitä mahtuu muistia.  Visual Studio yhteisön Edition-levyllä AM sisältää R Tools for Visual Studio-tunniste, joka sisältää koko IDE käsittelyyn R. Voit ladata ja käyttää muita IDEs sekä esimerkiksi [RStudio](http://www.rstudio.com). 

### <a name="python"></a>Python
Käyttämällä Python kehittämiseen Anaconda Python jakauman 2.7 ja 3.5 on asennettu. Jako on noin 300 suosituimpia matemaattisten merkkien ja tekniikka analytics-pakettien sekä peruste Python. Voit käyttää Python Tools for Visual Studio (PTVS), johon on asennettu Visual Studio 2015 yhteisön edition tai jokin Anaconda, kuten vapaa- tai Spyder kanssa yhteen IDEs. Voit käynnistää jokin näistä hakemalla etsintäpalkin (**Win** + **S** -näppäintä).

>[AZURE.NOTE] Osoittavan Python Tools for Visual Studio Anaconda Python 2.7 ja 3.5 haluat luoda mukautetun eri ympäristöissä. Siirry Määritä ympäristön polut Visual Studio 2015 yhteisön Edition- **Työkalut** -> **Python Työkalut** -> **Python ympäristöissä** ja valitse sitten **+ Mukautettu**. 

Anaconda Python 2.7 on asennettu kohdassa C:\Anaconda ja Anaconda Python 3.5 on asennettu c:\Anaconda\envs\py35-kohdassa. Yksityiskohtaisia ohjeita [PTVS](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) ohjeissa. 

### <a name="jupyter-notebook"></a>Jupyter muistikirjan
Anaconda jakauman sisältyy myös Jupyter muistikirjan-ympäristössä, voit jakaa koodi ja analyysi. Jupyter muistikirjan palvelin on määritetty ennalta Python 2, Python 3 ja R ytimet. Ei työpöydän kuvakkeen nimeltä "Jupyter muistikirjan käynnistää muistikirjan palvelinta selaimessa. Jos käytät AM etätyöpöydän kautta, voit myös käydä [https://localhost:9999 /](https://localhost:9999/) Jupyter muistikirjan palvelinta kirjautuessa AM.
 
>[AZURE.NOTE] Jatka, jos saat kaikki varmenteen varoitukset. 

Microsoft on pakattu otoksen muistikirjat Python ja R. Jupyter-muistikirjassa käsittelemisestä Microsoft R Server, SQL Server 2016 R Services (tietokanta-analyysin), Python ja Azure muita tekniikoita, kun kirjaudut sisään Jupyter. Näet mallit linkki muistikirjan aloitussivulla, kun todennetaan käyttämällä aiemmassa vaiheessa luotu salasana Jupyter muistikirjaan. 

### <a name="visual-studio-2015-community-edition"></a>Visual Studio 2015 yhteisön edition
Visual Studio yhteisön versio asennettuna AM. Se on ilmainen versio Suositut IDE Microsoftilta, jota voit käyttää arviointiin ja pieniä ryhmiä. Voit tarkistaa käyttöoikeusehdot [tähän](https://www.visualstudio.com/support/legal/mt171547).  Avaa Visual Studio kaksoisnapsauttamalla työpöydällä olevaa kuvaketta tai **Käynnistä** -valikko. Voit myös hakea ohjelmien **Win** + **S** ja syöttäminen "Visual Studion". Kun on projektien luominen kielissä, kuten C#, Python, R tai node.js. Laajennukset myös asennetaan, joiden avulla on helppo kuten Azure tietoluettelon Azure Hdinsightiin (Hadoop, ohjattu) ja Azure tietojen järvi Azure palveluiden kanssa. 

>[AZURE.NOTE] Näyttöön voi tulla sanoma mukaan arvioinnin kausi on vanhentunut. Kirjoita Microsoft-tilin tunnistetiedot tai Luo uusi vapaa tili, voit käyttää Visual Studio yhteisön Edition. 

### <a name="sql-server-2016-developer-edition"></a>SQL Server 2016 Developer edition
SQL Server 2016 R Services-tietokannan analyysin suorittamiseen developer versio on käytettävissä AM. R-palvelut ovat kehittämiseen ja käyttöönottoon älykkäät sovellusten alusta. Voit käyttää monipuolisen ja tehokkaan R-kieli- ja monta pakettien yhteisön mallien luominen ja luo ennusteiden SQL Server-tiedot. Voit pitää analytics lähellä tiedot koska R Services (n-tietokanta) integroida R kielen SQL Serverin kanssa. Näin kustannuksia ja tietojen siirto liittyvät tietoturvauhkia.

>[AZURE.NOTE] SQL Server 2016 developer edition voi käyttää vain kehittäminen ja testaus. Tarvitset Suorita tuotannon käyttöoikeuden. 

Voit käyttää SQL server, **SQL Server Management Studiossa**avaamista. AM nimesi lisätään palvelimen nimi. Käytä Windows-todennusta käyttäjä on kirjautunut sisään järjestelmänvalvojana Windows. Kun olet avannut SQL Server Management Studiossa voit muiden käyttäjien luominen, luoda tietokantoja, tuoda tietoja ja suorittaa SQL-kyselyjä. 

Jotta tietokanta-analyysin avulla Microsoft R, suorita seuraava komento yhtä kuin aikaa SQL Server management Studiossa toiminnon jälkeen kirjautuminen palvelimen järjestelmänvalvojana. 

        CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS 
        
        (Please replace the %COMPUTERNAME% with your VM name)


### <a name="azure"></a>Azure 
Azure työkaluilla on asennettu AM:

- Tällä työpöydän pikakuvakkeen käyttää Azure SDK-ohjeista. 
- **AzCopy**: Voit siirtää ja siitä uloskirjautuminen Microsoft Azure-tallennustilan tilin tiedot. Jos haluat nähdä käyttö, kirjoita komentokehotteeseen Nähdäksesi käyttö **Azcopy** . 
- **Microsoft Azure-tallennustilan Explorer**: käytettävä selata objekteja, jotka on tallennettuna Azure-tallennustilan tilin ja siirtää tiedot ja sieltä pois Azure-tallennustilan. Voit kirjoittaa **Tallennustilan Explorer** hakutoiminnossa tai etsi se tämä työkalu käyttää Windowsin Käynnistä-valikko. 
- **Adlcopy**: Siirry Azure tietojen järvi tietoja käytetään. Kirjoita **adlcopy** käyttö näkyviin komentorivi-ikkuna. 
- **dtui**: tietojen siirtäminen ja sieltä pois Azure DocumentDB pilveen NoSQL tietokannan avulla. Kirjoita **dtui** komentokehote. 
- **Microsoftin Data Management Gateway**: mahdollistaa tietojen siirto paikallisen tietolähteet ja cloud välillä. Sitä käytetään työkaluja, kuten Azure Data Factory. 
- **Microsoft Azure Powershell**: työkalu, jolla komentosarjakielen on asennettu myös oman AM PowerShellin Azure resurssien hallinta. 

###<a name="power-bi"></a>Power BI

Avulla voit täydentää raporttinäkymien ja hyvien visualisoinnit **Power BI Desktopin** on asennettu. Tällä työkalulla salaus puretaan eri lähteistä, raporttinäkymiä ja raporttien luominen ja julkaiseminen pilveen. Lisätietoja on artikkelissa [Power BI](http://powerbi.microsoft.com) -sivustossa. Löydät Power BI Desktopin Käynnistä-valikon. 

>[AZURE.NOTE] Sinun on käyttää Power BI Office 365-tili. 

## <a name="additional-microsoft-development-tools"></a>Lisätietoja Microsoft-Kehitystyökalut
[**Microsoftin WWW-ympäristö asennusohjelma**](https://www.microsoft.com/web/downloads/platform.aspx) avulla voidaan tutkia ja lataa muiden Microsoft-Kehitystyökalut. On myös työkalun annettujen Microsoft tietojen tiede Virtual Machine työpöydän pikakuvakkeen.  

## <a name="important-directories-on-the-vm"></a>Tärkeää AM kansiot

| Kohteen                          | Hakemisto |
| ------------------------------| ---------------- |
|Jupyter muistikirjan palvelimen määrityksiä | C:\ProgramData\jupyter |
|Esimerkkejä, joiden Jupyter muistikirjan pääkansion| c:\dsvm\notebooks|
|Muut objektit | c:\dsvm\samples|
| Anaconda (oletus: Python 2.7) | c:\Anaconda |
| Anaconda Python 3.5 ympäristössä | c:\Anaconda\envs\py35|
|R palvelimen yksittäisen esiintymän directory (oletus R esiintymän) | C:\Program Files\Microsoft SQL Server\130\R_SERVER |
| R Server-tietokannan esiintymä-hakemisto | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES |
| Muut työkalut | c:\dsvm\tools|

>[AZURE.NOTE] Esiintymät, Microsoft tietojen tiede virtuaalikoneen luotu ennen 1.5.0 (ennen 2016, 3 syyskuu) käytetään hieman eri tavalla kansiorakenne kuin edellisessä taulukossa. 

## <a name="next-steps"></a>Seuraavat vaiheet
Seuraavassa on seuraavia vaiheita voit jatkaa oppiminen ja tarkasteluun. 

* Tutustu eri tiede Datatyökalut tiedot tiede AM-valitsemalla Käynnistä-valikko ja valitsemalla Työkalut valikossa ulos.
* Siirry **C:\Program Files\Microsoft SQL Server\130\R_SERVER\library\RevoScaleR\demoScripts** näytteiden käyttämällä R, joka tukee tietojen analytics yrityksen tasolla RevoScaleR kirjastossa.  
* Tutustu artikkeliin: [10 asiaa, jotka voit tehdä tietojen tiede virtuaalikoneen](http://aka.ms/dsvmtenthings)
* Lue, miten [Ryhmän tietojen tiede prosessin](https://azure.microsoft.com/documentation/learning-paths/data-science-process/)käyttämällä järjestelmällisesti analytical lopusta loppuun-ratkaisuja.
* Käy tietokoneen oppiminen ja tietojen analytics esimerkkejä, jotka käyttävät Cortana Liiketoimintatieto-ohjelmistopaketin [Cortana Liiketoimintatieto-valikoima](http://gallery.cortanaintelligence.com) . On myös annettu kuvakkeen, **Käynnistä** -valikon ja virtuaalikoneen tämän valikoiman työpöydälle.

