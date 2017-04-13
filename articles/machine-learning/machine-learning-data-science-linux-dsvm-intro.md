<properties
    pageTitle="Valmistele Linux tietojen tiede virtuaalikoneen | Microsoft Azure"
    description="Määritä ja luoda Linux tietojen tiede Virtual Machine Azure analytics ja tietokoneen learning."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="bradsev" />

# <a name="provision-the-linux-data-science-virtual-machine"></a>Valmistele Linux tietojen tiede virtuaalikoneen

Linux tiede virtuaalikoneen on Azure virtual-tietokone, joka sisältää esiasennettu työkaluja. Näiden työkalujen käytetään yleisesti suorittamalla tietojen analytics ja tietokoneen learning. Sisällyttää avaimen ohjelmiston osat ovat seuraavat:

- Microsoft R Server Developer Edition
- Anaconda Python jakauma (versiot 2.7 ja 3.5), mukaan lukien Suositut tietojen analysointi kirjastot
- JupyterHub - yhteiskäyttö Jupyter muistikirjan palvelimen tukevat R, Python, Julia ytimet
- Azure-tallennustilan Explorer
- Azure komentorivivalitsimet interface (CLI) varten Azure resurssien hallinta
- PostgresSQL tietokanta
- Koneen learning Työkalut
    - [Laskennallinen verkon työkalujen (CNTK)](https://github.com/Microsoft/CNTK): syvä, kannattaa tutustua ohjelmiston työkalujen Microsoft Researchin kohteesta.
    - [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit): nopea konepohjaisten oppimistekniikoiden järjestelmän tukevat esimerkiksi online, hajautusfunktiota, allreduce, alennuksia, learning2search aktivointi tekniikoita ja vuorovaikutteisesti.
    - [XGBoost](https://xgboost.readthedocs.org/en/latest/): antamisen nopeasti ja tarkasti tehosti puun käyttöönoton työkalu.
    - [Rattle](http://rattle.togaware.com/) (R Analytical työkalua, katso helposti): työkalua, joka mahdollistaa tietojen analyysin ja konepohjaisten Koulujen R helposti Graafisen-pohjaisten tietojen tarkasteluun ja mallinnus automaattinen R-koodin luominen ja käytön aloittaminen.
- Azure SDK Java, Python, node.js, Ruby, PHP
- Kirjastojen R ja Python varten käyttäminen Azure koneen oppiminen ja muut Azure palvelut
- Kehitystyökalut ja editors (Pimennys, Emacs, gedit, vi)

Näin tietojen tiede on Iterointi Valitse sarja, tehtävät:

1. Etsiminen ja lataaminen esikäsittely tiedot
2. Luomiseen ja mallien testaaminen
3. Älykäs sovellusten kulutus mallit käyttöönotto

Tietoja tutkijoiden näiden tehtävien suorittaminen eri työkalujen avulla. Voit viedä aivan aikaa etsiä ohjelmiston haluamasi versiot ja käännä haluat ladata, ja asenna se seuraavia versioita.

Linux tiede virtuaalikoneen voi helpottaa Tätä rasitetta merkittävästi. Nopeuta analytics projektin sen avulla. Sen avulla eri kielillä, kuten R, Python, SQL, Java ja C++ tehtäviin. Pimennys on IDESTÄ kehittää ja testata koodi, joka on helppo käyttää. Azure-SDK AM sisältyvät avulla voit muodostaa sovellustesi käyttämällä eri palveluihin Linux Microsoft cloud-ympäristössä. Lisäksi voit käyttää muita kieliä kuten Ruby, Perl tai PHP node.js, jotka ovat myös valmiiksi asennettuna.

Veloituksia ei ole ohjelmiston tietojen tiede AM kuvan. Voit maksaa vain Azure laitteiston käyttömaksut, joka arvioidaan virtuaalikoneen, valmistella sekä AM kuvan koon perusteella. Lisätietoja Laske maksut löytyy [AM luettelo Azure Marketplace-sivulla ](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm/).


## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit luoda Linux tietojen tiede Virtual Machine, sinulla on oltava seuraavasti:

- **Azure tilaus**: hankkia-kohdassa [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/free/).
- **Azure-tallennustilan tilin**: Voit luoda luettelon artikkelissa [Azure-tallennustilan tilin luominen](storage-create-storage-account.md#create-a-storage-account). Voit myös tallennustilan tilin voi luoda osana luominen AM, jos et halua käyttää aiemmin luotu tili.


## <a name="create-your-linux-data-science-virtual-machine"></a>Luo Linux tietojen tiede virtuaalikoneen

Näin voit luoda esiintymää, Linux tietojen tiede virtuaalikoneen:

1.  Siirry [Azure portal](https://portal.azure.com/#create/microsoft-ads.linux-data-science-vmlinuxdsvm)-luettelon virtuaalikoneen.
2.   Valitse **Luo** (alareunassa) ja tuo näkyviin ohjatun toiminnon. ![määrittäminen-tiedot-tiede-AM](./media/machine-learning-data-science-linux-dsvm-intro/configure-linux-data-science-virtual-machine.png)
3.   Seuraavissa osissa on syötteiden avulla luodaan Microsoft tiede virtuaalikoneen kunkin (luetteloitu oikeassa laidassa olevasta edeltävässä kuvassa) ohjatun toiminnon ohjeita. Seuraavassa on syötteiden tarvittavat kunkin seuraavasti:

    a. **Perusteet**:

  - **Nimi**: luot tietojen tiede palvelimen nimi.
  - **Käyttäjänimi**: ensimmäisen tilin Kirjautumisvirheitä aiheuttavien ID-tunnuksellasi.
  - **Salasana**: ensimmäisen tilin salasana (Voit käyttää SSH julkisella avaimella salasanan sijaan).
  - **Tilaus**: Jos sinulla on useita tilauksia, valitse jokin, tietokoneessa on luotu ja laskutettu. Sinulla on resurssin luomisen oikeudet tähän tilaukseen.
  - **Resurssiryhmä**: Voit luoda uuden tai Käytä aiemmin luotua ryhmää.
  - **Sijainti**: Valitse tietokeskuksen, joka sopii parhaiten. Se on yleensä tietokeskuksen, joka on yleensä tietojen tai lähinnä nopein verkkokäyttö fyysinen sijaintisi.

    b. **Kokoa**:

  - Valitse jokin Palvelintyypit, joka täyttää toiminnallisten vaatimusten ja kustannukset rajoitukset. Valitse **Näytä kaikki** saa näkyviin lisää vaihtoehtoja AM kokoisille.

    c-näppäinyhdistelmää. **Asetukset**:

  - **Levyn tyyppi**: Valitse **Premium** , jos käytät mieluummin tasainen tila-asema (Suoritettaessa). Valitse muussa tapauksessa **Vakio**.
  - **Tallennustilan tilin**: Voit luoda uuden Azure-tallennustilan tilin tilaukseesi tai Käytä aiemmin luotua samassa sijainnissa, joka on valittu **perusteet** ohjatun toiminnon vaiheessa.
  - **Muut parametrit**: useimmissa tapauksissa vain käyttää oletusarvoja. Muun kuin oletusarvoisen arvoja, sinun kannattaa hiiren osoitinta tiedoksi linkki-kentistä.

    d. **Yhteenveto**:

  - Tarkista, että olet kirjoittanut kaikki tiedot ovat oikein.

    e. **Ostaminen**:

  - Voit aloittaa valmistelun valitsemalla **Osta**. Linkki on tapahtuman ehdot. AM ei ole muita kulut lisäksi Laske **koon** vaiheessa valitsemasi palvelimen koon.

Valmistelun olisi kestää noin 10 – 20 minuutin ajan. Valmistelun tila näkyy Azure-portaalissa.

## <a name="how-to-access-the-linux-data-science-virtual-machine"></a>Linux tiede virtuaalikoneen käyttäminen

AM luomisen jälkeen voit kirjautuminen siihen SSH avulla. Käytä tilin tunnistetiedot, jonka loit vaiheessa 3, teksti-käyttöliittymä **Perustiedot** -osassa. Windows voit ladata SSH-asiakasohjelma, kuten [painovärit, muste](http://www.putty.org). Jos käytät mieluummin graafinen työpöydän (X Windows-järjestelmä), voit käyttää X11 välittäminen painovärit, muste- tai asentaa X2Go.

>[AZURE.NOTE] X2Go asiakkaan suorittaa huomattavasti parempi kuin X11 välittäminen testauksessa. On suositeltavaa X2Go käyttävä graafinen työpöydän liittymän.


## <a name="installing-and-configuring-x2go-client"></a>Asentaminen ja määrittäminen X2Go asiakas

Linux AM on jo valmisteltu X2Go palvelimen kanssa ja valmis hyväksymään asiakasyhteydet. Muodostaa yhteyden Linux AM graafinen työpöydän asiakkaan käyttöön seuraavasti:

1. Lataa ja asenna asiakkaan ympäristö X2Go asiakkaan [X2Go](http://wiki.x2go.org/doku.php/doc:installation:x2goclient).    
2. Suorita X2Go asiakas ja valitse **Uuden istunnon**. Se avaa määritys ikkunaa on useita välilehtiä. Anna seuraavat määritykset parametrit:
    * **Istunnon-välilehdessä**:
        - **Host (isäntä)**: isäntänimi tai Linux tiedot-tiede-AM IP-osoite.
        - **Kirjaudu sisään**: Linux AM käyttäjänimi.
        - **SSH portti**: jättää 22 oletusarvoa.
        - **Istunnon tyyppi**: muuta arvoksi XFCE. Linux AM tukee tällä hetkellä vain XFCE työpöydälle.
    * **Media-välilehti**: Voit poistaa käytöstä äänen tuki- ja asiakkaan tulostaminen, jos sinun ei tarvitse käyttää niitä.
    * **Jaetut kansiot**: Jos haluat kansioiden oman asiakaskoneiden Linux AM asennettu, Lisää asiakkaan tietokoneen kansioita, jonka haluat jakaa Tässä välilehdessä AM.

Kirjautuminen AM käyttämällä joko SSH asiakas- tai XFCE graafinen työpöydän X2Go-asiakasohjelman kautta, kun olet valmis aloittamaan työkaluilla, jotka on asennettu ja määritetty AM. Valitse XFCE näet sovellusten valikon pikakuvakkeiden ja työpöydän kuvakkeet useita työkaluja.


## <a name="tools-installed-on-the-linux-data-science-virtual-machine"></a>Valitse Linux tietojen tiede virtuaalikoneen-Työkalut

### <a name="microsoft-r-open"></a>Avaa Microsoft R
R on tietojen analysointi ja tietokoneen oppiminen Suosituimmat kielet. Jos haluat käyttää R oman analytics AM on Microsoft R Avaa (MRO) matemaattiset ydin kirjaston (MKL) kanssa. MKL optimoi matemaattisten toimintojen yleisiä analytical algoritmeista. MRO on 100 prosenttia CRAN R-yhteensopiva ja jokin julkaistu CRAN R-kirjastot voidaan asentaa MRO. Voit muokata R-ohjelmien jossakin oletusarvon editors, kuten vi, Emacs tai gedit. Voit ladata ja käyttää muita IDEs, kuten [RStudio](http://www.rstudio.com). Ladattavana yksinkertaista komentosarjaa (installRStudio.sh) on annettu, joka asentaa RStudio **/dsvm/tools** hakemistossa. Jos käytössäsi on Emacs-editori, Huomaa, että Emacs pakata ESS (Emacs puhuu tilastot), joka helpottaa R-tiedostojen Emacs-editorissa on asennettu valmiiksi.

Käynnistää R, kirjoita **R** -käyttöliittymän. Tämä avaa vuorovaikutteisessa ympäristössä. Kehittää R-ohjelmaa käytetään yleensä muokkaajan, kuten Emacs tai vi tai gedit ja suorita sitten komentosarjat sisällä R. Jos asennat RStudio, sinulla on täydellinen graafinen IDE-ympäristön kehittää R-ohjelmaa.

On myös R-komentosarjan, voit asentaa [ylhäältä 20 R pakettien](http://www.kdnuggets.com/2015/06/top-20-r-packages.html) , voit halutessasi. Tämä komentosarja suorittamisen jälkeen R vuorovaikutteinen käyttöliittymä, joka voidaan kirjoittaa (kuten edellä) kirjoittamalla **R** -käyttöliittymän käyttämässä.  

### <a name="python"></a>Python
Käyttämällä Python kehittämiseen Anaconda Python jakauman 2.7 ja 3.5 on asennettu. Jako on noin 300 suosituimpia matemaattisten merkkien ja tekniikka analytics-pakettien sekä peruste Python. Voit käyttää oletusarvon tekstin editors. Lisäksi voit käyttää Spyder, Python IDE, joka on yhdistetty Anaconda Python jaot. Spyder tarvitsee graafinen pöytätietokoneessa tai X11 lähettäminen edelleen. Pikakuvakkeen Spyder annetaan graafinen työpöydälle.

Koska on Python 2.7 ja 3.5, sinun täytyy aktivoida erikseen haluamasi Python-version haluat käsitellä nykyisessä istunnossa. Aktivointi asettaa LIIKERATA muuttuja Python haluamasi versio.

Python 2.7 aktivoimiseksi pitäminen käyttöliittymän seuraavasti:

    source /anaconda/bin/activate root

Python 2.7 asennetaan */anaconda/bin*.

Python 3.5 aktivoimiseksi pitäminen käyttöliittymän seuraavasti:

    source /anaconda/bin/activate py35


Python 3.5 asennetaan */anaconda/envs/py35/bin*.

Käynnistää Python vuorovaikutteisen istunnon, kirjoita **python** -käyttöliittymän. Jos ovat graafisessa käyttöliittymässä tai niissä välittäminen set up X11, voit kirjoittaa **spyder** käynnistää Python IDE.

### <a name="jupyter-notebook"></a>Jupyter muistikirjan

Anaconda jakauman sisältyy myös Jupyter muistikirjan-ympäristössä, voit jakaa koodi ja analyysi. Jupyter muistikirjan käytetään JupyterHub kautta. Voit kirjautua sisään paikallisen Linux-käyttäjänimi ja salasana.

Jupyter muistikirjan palvelin on määritetty ennalta Python 2, Python 3 ja R ytimet. Työpöydän kuvakkeen nimeltä "Jupyter muistikirja-käynnistää selaimessa muistikirjan-palvelinta ei. Jos käytät AM SSH tai X2Go-asiakasohjelman kautta, voit myös käydä [https://localhost:8000 /](https://localhost:8000/) Jupyter muistikirjan palvelimen käyttämiseen.

>[AZURE.NOTE] Jatka, jos saat kaikki varmenteen varoitukset.

Voit käyttää kaikkien isäntien Jupyter muistikirjan palvelimeen. Kirjoita *https://\<AM DNS-nimen tai IP-osoite\>: 8000 /*

>[AZURE.NOTE] Portti 8000 on avattu oletusarvoisesti palomuurissa AM on valmisteltu.

Microsoft on pakattu otoksen muistikirjat--yksi tekstimuodossa Python ja toinen R. Näet mallit linkin muistikirjan aloitussivulla, kun todennetaan Jupyter muistikirjan paikallisen Linux-käyttäjänimen ja salasanan avulla. Voit luoda uuden muistikirjan valitsemalla **Uusi**ja valitsemalla sitten haluamasi kielen ydin. Jos et näe **Uusi** -painiketta, valitse Siirry aloitussivulle muistikirjan palvelimen vasemmalta ylhäältä **Jupyter** -kuvaketta.


### <a name="ides-and-editors"></a>IDEs ja editors

Voit valita useita koodin editors. Tämä sisältää vi/VIM, Emacs, gEdit ja Pimennys. gEdit ja Pimennys ovat graafinen editors, ja sinun on oltava kirjautuneena graafinen työpöydälle voit käyttää niitä. Nämä editors on työpöydän ja sovelluksen valikon pikakuvakkeet käynnistää niitä.

**VIM** ja **Emacs** ovat tekstipohjainen editors. Emacs emme on asennettu apuohjelma-paketti, kutsutaan Emacs puhuu tilastotiedot (ESS), joka helpottaa R käsitteleminen Emacs-editorissa. Lisätietoja on osoitteessa [ESS](http://ess.r-project.org/).

**Pimennys** on Avaa lähde extensible IDE, joka tukee useita kieliä. Java kehittäjät versio on asennettu AM esiintymä. Suositut kielet, jotka voidaan asentaa siirtämisestä Pimennys ympäristön ovat laajennukset. Laajennus asennettuna Pimennys **Azure työkalujen Pimennys,**jota kutsutaan myös on. Voit luoda, kehittää, Testaa ja Azure sovellusten käyttäminen Pimennys kehitysympäristö, joka tukee kieliä, kuten Java. On myös **Java Azure SDK-paketissa** , joka sallii käyttämisen eri Azure palvelut Java-ympäristössä. Saat lisätietoja Azure työkalujen Pimennys varten on osoitteessa [Azure työkalujen Pimennys varten](../azure-toolkit-for-eclipse.md).

**Lateksien** asennetaan texlive sekä Emacs-lisäosa [auctex](https://www.gnu.org/software/auctex/manual/auctex/auctex.html) paketti, mikä yksinkertaistaa authoring lateksien tiedostojen Emacs kuluessa paketin avulla.  

### <a name="databases"></a>Tietokannat

#### <a name="postgres"></a>Postgres
Avaa lähdetietokannan **Postgres** on käytettävissä, AM palveluita ja initdb jo valmis. Haluat luoda tietokantoja ja käyttäjille. Lisätietoja on artikkelissa [Postgres ohjeissa](https://www.postgresql.org/docs/).  

####  <a name="graphical-sql-client"></a>Graafisen SQL Clientin
**SQuirrel SQL**-graafinen SQL-asiakasohjelmaan on annettu, voit muodostaa yhteyden eri tietokannat (esimerkiksi Microsoft SQL Server, Postgres ja MySQL) ja Suorita SQL-kyselyjä. Voit suorittaa tämän graafinen istuntoon (käyttävä X2Go, kuten). Käynnistää SQuirrel SQL-Käynnistä työpöydän kuvakkeesta tai suorittamalla seuraavan komennon-käyttöliittymä.

    /usr/local/squirrel-sql-3.7/squirrel-sql.sh

Määritä ennen ensimmäisen käyttökerran ohjaimet ja tietokannan aliakset. JDBC ohjaimia osoitteessa:

*/USR/Share/Java/jdbcdrivers*

Lisätietoja on artikkelissa [SQuirrel SQL](http://squirrel-sql.sourceforge.net/index.php?page=screenshots).

#### <a name="command-line-tools-for-accessing-microsoft-sql-server"></a>Komentorivin Työkalut, joilla Microsoft SQL Server

SQL Server ODBC-ohjain paketti sisältää myös kaksi komentorivin työkaluja:

**BCP**: bcp apuohjelman usean kopioi Microsoft SQL Server-esiintymän ja datatiedoston tietojen käyttäjän määrittämä-muodossa. Bcp-apuohjelma voidaan käyttää paljon uusien rivien tuominen SQL Server-taulukoihin tai ulos taulukoiden tietojen tuominen datatiedostot. Voit tuoda tietoja taulukkoon, sinun on käyttää tiedostomuodossa, ohjelma luo taulukko tai tietoja, jotka ovat voimassa sarakkeiden tietotyyppien taulukon rakennetta.

Lisätietoja on artikkelissa [yhteyden bcp kanssa](https://msdn.microsoft.com/library/hh568446.aspx).

**SQLCMD**: voit syöttää Transact-SQL-lauseita sqlcmd apuohjelma sekä järjestelmän menettelyt ja komentosarja komentokehotteen tiedostot. Tämän apuohjelman käyttää ODBC suorittamiseen Transact-SQL-erissä.

Lisätietoja on artikkelissa [yhteyden sqlcmd kanssa](https://msdn.microsoft.com/library/hh568447.aspx).

>[AZURE.NOTE] On joitakin tämän apuohjelman erot Linux ja Windows-käyttöjärjestelmien työpöytäsovelluksiin. Lisätietoja ohjeissa.


#### <a name="database-access-libraries"></a>Tietokannan access-kirjastot

Kirjastojen ovat käytettävissä R ja Python access-tietokannat.

- **RODBC** paketti tai **dplyr** paketin avulla voit tehdä kyselyn tai suorittaa SQL-lauseet tietokantapalvelimen R.
- Python **pyodbc** kirjasto sisältää ODBC-tietokanta access kuin pohjana kerros.  

Voit käyttää **Postgres**seuraavasti:

- Käytä R: Paketin **RPostgreSQL**.
- Käytä Python: **Psycopg2** -kirjasto.


### <a name="azure-tools"></a>Azure Työkalut
Seuraavat Azure työkalut asennetaan AM:

- **Azure käyttöliittymä**: Azure CLI avulla voit luoda ja hallita Azure resursseja kautta shell-komennot. Käynnistää Azure Työkalut, kirjoita **azure Ohje**. Lisätietoja on artikkelissa [Azure CLI asiakirjat-sivu](../virtual-machines-command-line-tools.md).
- **Microsoft Azure-tallennustilan Explorer**: Microsoft Azure-tallennustilan Explorer on graafinen työkalu, jonka avulla voit selata objekteja, jotka olet tallentanut Azure-tallennustilan tilin ja lataa ja lataa tiedot ja Azure BLOB sieltä. Voit käyttää tallennustilan Explorer työpöydän pikakuvakkeen. Voit käynnistää sen-liittymän kehote kirjoittamalla **StorageExplorer**. Sinun on oltava kirjautuneena X2Go-asiakasohjelmassa tai X11 välittäminen set up.
- **Azure-kirjastot**: alla on lueteltu joitakin esiasennettu kirjastoihin.

 - **Python**: Python, jotka asennetaan Azure liittyvien kirjastojen ovat **azure**, **azureml**, **pydocumentdb**ja **pyodbc**. Kolme ensimmäistä kirjastot voit käyttää Azure liittyviä palveluja, Azure koneen oppiminen ja Azure DocumentDB (NoSQL tietokannan Azure). Neljäs kirjaston pyodbc (sekä Microsoft ODBC-ohjain SQL Server), mahdollistaa SQL Server Azure SQL-tietokantaan ja SQL Azure-tietovarasto käytöltä Python ODBC-käyttöliittymän avulla. Kirjoita **pip luettelon** ja tarkastella luettelossa kirjastoihin. Muista suorittaa tämän komennon Python 2.7 ja 3,5 ympäristössä.
 - **R**: Azure liittyvien kirjastojen R, jotka on asennettu ovat **AzureML** ja **RODBC**.
 - **Java**: Azure Java kirjastojen luettelo löytyy AM hakemisto- **/dsvm/sdk/AzureSDKJava** . Avaimen kirjastojen ovat Azure tallennus ja hallinta API, DocumentDB ja JDBC SQL Serveriä varten.  

Voit käyttää [Azure portal](https://portal.azure.com) esiasennettu Firefox-selaimessa. Azure-portaalissa voit luoda, hallita ja seurata Azure resurssit.

### <a name="azure-machine-learning"></a>Azure koneen oppiminen

Azure koneen oppiminen on täysin hallitun pilvipalvelussa, jonka avulla voit luoda, ottaminen käyttöön ja jakaa ennakoivan analytics-ratkaisuja. Voit luoda kokeissa ja mallien Azure koneen Learning Studio. Sitä voi käyttää selaimessa, valitse tietoja tiede virtuaalikoneen ohjesisältöä [Microsoft Azure koneen Learning](https://studio.azureml.net).

Sen jälkeen, kun kirjautuminen Azure Konepohjaisten Oppimistekniikoiden Studio, joihin sinulla on pääsy vuorovaikutteisuudesta piirtoalustan jossa voit luoda konepohjaisten oppimistekniikoiden algoritmit looginen työnkulku. Myös käyttämään isännöimät Azure koneen Learning Jupyter muistikirjan ja käyttää saumattomasti koneen Learning Studiossa kokeissa. Operationalize konepohjaisten oppimistekniikoiden mallit, jotka on luotu rivittämällä-sivustoissa. Näin asiakkaat, minkä tahansa kielellä käynnistää ennusteiden Koulujen mallit tietokoneesta. Lisätietoja [tietokoneen Learning ohjeissa](https://azure.microsoft.com/documentation/services/machine-learning/).

Voit myös luoda R tai Python mallit AM ja ottaa tuotannon Azure koneen oppiminen. On asennettu kirjastojen R (**AzureML**) ja Python (**azureml**), voit ottaa tämän toiminnon käyttöön.

Lisätietoja siitä, miten voit asentaa mallien R ja Python Azure koneen Learning on artikkelissa [kymmenen voit käyttää tietojen tiede virtuaalikoneen-toimintoja](machine-learning-data-science-vm-do-ten-things.md) (erityisesti osan "R tai Python mallien luominen ja Operationalize ne käyttämällä Azure koneen Learning").

>[AZURE.NOTE] Nämä ohjeet on kirjoitettu tietojen tiede AM Windows-versiota. Mutta tiedot-käyttöönotto mallien Azure koneen Learning ei koske Linux AM.

### <a name="machine-learning-tools"></a>Koneen learning Työkalut

AM sisältyy muutama konepohjaisten oppimistekniikoiden työkalut ja algoritmit, jotka on valmiiksi käännetty ja esiasennettuna paikallisesti. Näitä ovat:

* **CNTK** (Laskennallinen verkon Työkalut-Microsoft Researchin): syvä, kannattaa tutustua työkalujen.
* **Vowpal Wabbit**: nopeasti online learning algoritmin.
* **xgboost**: työkalua, joka on optimoitu, tehosti puun algoritmeista.
* **Python**: Anaconda Python on mukana koneen learning algoritmit kirjastoja, kuten Scikit tietoja. Voit asentaa muita kirjastoja avulla `pip install` komento.
* **R**: tietokoneen learning Funktiot monipuolisia valikoiman on käytettävissä R. Jotkin kirjastot, jotka on asennettu valmiiksi ovat lm glm, randomForest, rpart. Muita kirjastoja voidaan asentaa suorittamalla:

        install.packages(<lib name>)

Tässä on joitakin lisätietoja ensimmäisen kolme koneen learning työkaluista, luettelossa.

#### <a name="cntk"></a>CNTK
Tämä on Avaa lähde-laaja Koulujen työkalut. Se on komentorivin työkalu (cntk) ja on jo POLKU.

Suorita basic otoksen, suorita seuraavat komennot-käyttöliittymän:

    # Copy samples to your home directory and execute cntk
    cp -r /dsvm/tools/CNTK-2016-02-08-Linux-64bit-CPU-Only/Examples/Other/Simple2d cntkdemo
    cd cntkdemo/Data
    cntk configFile=../Config/Simple.cntk

Mallin tulos on *~/cntkdemo/Output/Models*.

Lisätietoja on kohdassa CNTK [GitHub](https://github.com/Microsoft/CNTK)ja [CNTK wiki](https://github.com/Microsoft/CNTK/wiki).


#### <a name="vowpal-wabbit"></a>Vowpal Wabbit

Vowpal Wabbit on esimerkiksi online, hajautusfunktiota, allreduce, alennuksia, learning2search aktivointi tekniikoita käyttävän järjestelmän Koulujen kone ja vuorovaikutteisesti.

Käyttämään työkalua hyvin basic esimerkiksi seuraavasti:

    cp -r /dsvm/tools/VowpalWabbit/demo vwdemo
    cd vwdemo
    vw house_dataset

Kansiota ei muiden, suuremman esittelyjen. Saat lisätietoja VW [GitHub tässä osassa](https://github.com/JohnLangford/vowpal_wabbit)ja [Vowpal Wabbit wiki](https://github.com/JohnLangford/vowpal_wabbit/wiki).

#### <a name="xgboost"></a>xgboost
Tämä on kirjastoon, joka on suunniteltu ja optimoitu tehosti (puun) algoritmeista. Tämä kirjasto tavoitteena on push koneet laskenta rajoissa suurissa puun kehittämällä, joita tarvitaan huomattavan on skaalattava, kannettavaan ja tarkkoja.

Se on saatavana komentorivin sekä R-kirjasto.

Käyttämään tämän kirjaston R vuorovaikutteinen R-istunnon aloittaminen (kirjoittamalla **R** -käyttöliittymän) ja ladata kirjastoon.

Tässä on esimerkki voit suorittaa R kehote:

    library(xgboost)

    data(agaricus.train, package='xgboost')
    data(agaricus.test, package='xgboost')
    train <- agaricus.train
    test <- agaricus.test
    bst <- xgboost(data = train$data, label = train$label, max.depth = 2,
                    eta = 1, nthread = 2, nround = 2, objective = "binary:logistic")
    pred <- predict(bst, test$data)

Suorita xgboost komentorivin seuraavien komentojen suorittamiseen-käyttöliittymän:

    cp -r /dsvm/tools/xgboost/demo/binary_classification/ xgboostdemo
    cd xgboostdemo
    xgboost mushroom.conf


.Model tiedoston kirjoitetaan määritettyyn kansioon. Tietoja esittely tässä esimerkissä löytyy [GitHub](https://github.com/dmlc/xgboost/tree/master/demo/binary_classification).

Saat lisätietoja xgboost [xgboost asiakirjat-sivu](https://xgboost.readthedocs.org/en/latest/), ja sen [Github säilöön](https://github.com/dmlc/xgboost).

#### <a name="rattle"></a>Rattle
( **R** **A**nalytical **T**Mustetyökalu **T**o **L**ansaita **E**asily) rattle käyttää Graafisen-pohjaisten tietojen tarkasteluun ja mallinnus. Sen tuottamat tiedot-muunnoksia tiedot, jotka voi helposti mallintaa, muodostaa varoitukseksi sekä valvonnan alaisena mallien tiedoista, esittää mallien suorituskyvyn graafisesti tilastollisten ja visual yhteenvetojen ja määrittää tulosten uudet tiedot. Se myös Luo R-koodia, replikoiminen käyttöliittymässä toiminnoista, joita voidaan suorittaa suoraan R tai käyttää pohjana lisäanalysointia varten.

Toimimaan Rattle sinun on oltava istunnossa graafinen työpöydän Kirjaudu sisään. Kirjoita terminaalissa, ```R``` R-ympäristöön. R tulee näkyviin Kirjoita seuraavat komennot:

    library(rattle)
    rattle()

Nyt graafisessa käyttöliittymässä avautuu välilehtiä. Seuraavassa on Rattle tarvitaan otoksen sää tietojoukon ja luoda mallin pikaopas ohjeita. Joissakin seuraavien vaiheiden kuvissa sinua kehotetaan asentaminen ja lataaminen joitakin tarvittavat R-paketteja, jotka eivät ole vielä järjestelmän automaattisesti.

>[AZURE.NOTE] Jos sinulla ei access Asenna paketti järjestelmän hakemistossa (oletusasetus), voit nähdä kehote R console-ikkunassa pakettien asentamisen oma kirjasto. Vastaa *y* , jos näet näitä ohjeita.

1. Valitse **Suorita**.
2. Valintaikkuna tulee, jossa kysytään, jos haluat käyttää esimerkki sää tietojoukon. Valitse **Kyllä** lataamaan esimerkissä.
3. Valitse **malli** -välilehti.
4. Valitse **Suorita** luonnissa Päätöspuun.
5. Valitse Näytä Päätöspuun **Piirrä** .
6. Valitse **metsää** -valintanappi ja valitse **Suorita** luonnissa satunnaisia metsää.
7. Valitse **Laske** -välilehti.
8. **Riski** -valintanappi ja sitten Näytä kaksi riski (kumulatiivinen) suorituskyvyn joudutaan **suoritus** .
9. **Log** -välilehdestä Näytä edellisen toimille Luo R-koodi.
(Vuoksi ohjelmavirhe Rattle nykyisessä versiossa, sinun on lisättävä *#* merkki eteen *lokitiedostoon... vieminen* tekstin lokin.)
10. Valitse Tallenna nimeltä *weather_script R-komentosarjatiedosto **Vie** -painiketta. R* koti-kansioon.

Voit poistua Rattle ja R. Voit nyt muokata luotu R-komentosarjan tai käyttää sitä voidaan suorittaa milloin tahansa toistuvan kaikki tiedot, jotka on tehty sisällä Rattle-Käyttöliittymä. Erityisesti aloittelijoille R-tämä on helppo tapa nopeasti analyysin suorittamiseen ja tietokoneen learning yksinkertainen graafisessa käyttöliittymässä, samalla luodaan automaattisesti koodin R muokkaaminen ja/tai Lue.


## <a name="next-steps"></a>Seuraavat vaiheet
Näin, kuinka voit jatkaa oppiminen ja siihen tarkemmin:

* [Tietoja tiede-Linux tietojen tiede virtuaalikoneen](machine-learning-data-science-linux-dsvm-walkthrough.md) vaiheittaisissa ohjeissa kerrotaan, miten voit suorittaa useita yleisiä tietoja tiede tehtäviä valmisteltu tähän Linux tietojen tiede AM. 
* Tässä artikkelissa kuvattujen työkalujen kokeilla tutkia tietoja tiede AM eri tiede Datatyökalut. Voit suorittaa *dsvm Lisää-tiedot* sisällä virtuaalikoneen basic johdanto ja osoittimet shell-työkalut asennetaan AM lisätietoihin.  
* Opettele luo lopusta loppuun analytical ratkaisuja järjestelmällisesti [Ryhmän tietojen tiede prosessin](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)avulla.
* Käy tietokoneen oppiminen ja tietojen analytics esimerkkejä, jotka käyttävät Cortana Analytics-ohjelmistopaketin [Cortana Analytics-valikoima](http://gallery.cortanaanalytics.com) .
