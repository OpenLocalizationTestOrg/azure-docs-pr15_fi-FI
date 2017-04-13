<properties
    pageTitle="SQL Server-virtual tietokoneeseen IPython muistikirjan palvelimeksi määrittäminen | Microsoft Azure"
    description="Määritä määrittäminen tietojen tiede Virtual tietokoneessa, jossa SQL Server ja IPython Server."
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
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Määritä Azure SQL Server-virtual machine IPython muistikirjan palvelimeksi kehittyneen analyysin varten

Tässä ohjeaiheessa esitellään, miten voit valmistella ja SQL Server virtual-koneen pilvipohjainen tietojen tiede-ympäristön osana käytettävä. Windows-virtuaalikoneen on määritetty tukemaan työkaluilla, kuten IPython muistikirjan, Azure-tallennustilan Explorer ja AzCopy sekä muita apuohjelmia, jotka ovat hyödyllisiä tietoja tiede projektien. Azure tallennustilan Explorer ja AzCopy, esimerkiksi on kätevä tapoja tietojen lataaminen Azure-blob-säiliö paikallisesta tietokoneesta tai lataa se tietokoneeseesi Blob-objektien tallennustilaan.

Azure virtuaalikoneen-valikoima sisältää useita kuvia, jotka sisältävät Microsoft SQL Server. Valitse SQL Server AM-kuva, joka sopii tiedot tarpeitasi. Suositeltu kuvat ovat:

- SQL Server 2012 SP2: n Enterprise pienten Normaali tietojen koot
- SQL Server 2012 SP2: n yrityksen optimoitu DataWarehousing työmääriä suuri hyvin suuria tietomääriä

 > [AZURE.NOTE] SQL Server 2012 SP2: n Enterprise kuva **ei sisällä tietoja DVD-levyllä**. Tarvitset lisää ja/tai liittää vähintään yksi levyjä tietojen tallentamiseen. Luodessasi Azure virtuaalikoneen on yhdistetty C-asema käyttöjärjestelmän levyn ja yhdistetty D-aseman tilapäinen DVD-levyllä. Älä käytä D-aseman tietojen tallentamiseen. Nimensä se antaa vain väliaikaisten. Siinä on redundancy tai varmuuskopiointi ei, koska se ei sijaitsevat Azure-tallennustilan.


##<a name="Provision"></a>Yhdistä perinteinen Azure-portaaliin ja SQL Server-virtual machine valmistelu

1.  Kirjaudu sisään käyttämällä tiliä [Azure perinteinen Portal](http://manage.windowsazure.com/) .
    Jos sinulla ei ole Azure-tili, käy [vapaa kokeiluversion Azure](https://azure.microsoft.com/pricing/free-trial/).

2.  Azure perinteinen-portaalissa, web-sivun vasemmassa alareunassa valitsemalla **+ Uusi**, valitse **Laske**, valitse **VIRTUAALIKONEEN**ja valitse **Lähettäjä-VALIKOIMA**.

3.  Valitse **Luo Virtual Machine** -sivulla Valitse virtuaalikoneen-kuva, joka sisältää SQL Server data käyttötarkoitukseen ja valitse sitten sivun oikeassa alareunassa seuraava-nuoli. Saat uusimmat tiedot Azure tuetut SQL Server-kuvista aiheessa [Käytön aloittaminen SQL Server Azuren näennäiskoneiden](http://go.microsoft.com/fwlink/p/?LinkId=294720) [SQL Serverin Azuren näennäiskoneiden](http://go.microsoft.com/fwlink/p/?LinkId=294719) ohjeissa määrittäminen.

    ![Valitse SQL Server AM][1]

4.  Anna ensimmäinen **Virtuaalikoneen määritys** -sivulla seuraavat tiedot:

    -   Anna **VIRTUAALIKONEEN nimi**.
    -   Kirjoita **Uusi nimi** -ruutuun käyttäjätunnuksesta AM paikallisen järjestelmänvalvojan tilin.
    -   Kirjoita **Uusi salasana** -ruutuun vahva salasana. Lisätietoja on artikkelissa [Vahvan salasanan](http://msdn.microsoft.com/library/ms161962.aspx).
    -   Kirjoita salasana uudelleen **VAHVISTA salasana** -ruutuun.
    -   Valitse avattavasta luettelosta haluamasi **koon** .

     > [AZURE.NOTE] Virtuaalikoneen kokoa on määritetty valmistelun aikana: solun A2 arvo pienin koko suositus tuotannon toiminnoista. Virtual machine Suositeltu vähimmäiskoko on A3, kun SQL Server Enterprise-versioon. Valitse A3 tai uudempi versio, kun SQL Server Enterprise-versioon. Valitse A4, kun käytössä on SQL Server 2012 tai 2014 yrityksen optimoitu tapahtumien työmääriä kuville.
Valitse A7, kun käytössä on SQL Server 2012 tai 2014 yrityksen optimoitu tietojen Warehousing työmääriä kuville. Valittu kokoa rajoittaa tietojen levyjä, voit määrittää. Uusimpia tietoja käytettävissä virtuaalikoneen kokojen ja tietoja-levyjä, voit liittää virtual machine määrä on artikkelissa [Azure virtuaalikoneen koot](http://msdn.microsoft.com/library/azure/dn197896.aspx). Alla on tietoja, katso [VIrtual Macines hinnat](https://azure.microsoft.com/pricing/details/virtual-machines/).

    Valitse Jatka oikeassa alakulmassa seuraava-nuoli.

    ![AM määritys][2]

5.  Toinen **virtuaalikoneen määritys** -sivulla Määritä verkko, varasto ja käytettävyyden resurssit:

    -   Valitse **Luo uusi pilvipalvelussa** **Pilvipalvelussa** -ruudussa.
    -   **Cloud palvelun DNS-nimi** -ruutuun antaa valittua, DNS-nimen ensimmäinen osa, niin, että se on valmis nimi **TESTNAME.cloudapp.net** -muodossa
    -   Valitse alue, jossa virtual kuvan isännöidään **Alue/AFFINITEETTI ryhmän/VIRTUAL verkko** -ruudussa.
    -   **Tallennustilan tilin**käytössä olevan tallennustilan-tilin tai valitsemalla automaattisesti luotua.
    -   Valitse **Määritä KÄYTETTÄVYYS** -kentässä **(ei mitään)**.
    -   Lue ja hyväksy hinnoittelutiedot.

6.  **PÄÄTEPISTEET** -osassa napsauttamalla tyhjä avattavan luettelon **nimi**ja valitse **MSSQL** sitten Kirjoita porttinumero esiintymän Database Engine (**1433** oletusesiintymää varten).

7.  SQL Server-AM voit myös käyttää palvelimeksi IPython muistikirjan, joka on määritetty myöhemmin.
    Lisää uusi päätepiste määrittäminen käyttämään IPython muistikirjan palvelimen portti. Kirjoita nimi **nimi** -sarakkeen, valitse julkisen portin valittua ja 9999 yksityinen portin porttinumero.

    Valitse Jatka oikeassa alakulmassa seuraava-nuoli.

    ![Valitse MSSQL ja IPython portit][3]

8.  Hyväksy oletusarvoinen **asentaa AM agent** -asetus valittuna ja valitse valmistelu AM ohjatun toiminnon oikeassa alakulmassa valintamerkki.

    `![AM näitä vaihtoehtoja][4]

9.  Odota, kun Azure valmistellaan virtuaalikoneen. Pitäisi virtuaalikoneen tilaksi käy läpi:

    -   Aloitetaan (valmistelu)
    -   Pysäytetty
    -   Aloitetaan (valmistelu)
    -   Käynnissä (valmisteleminen)
    -   Käynnissä


##<a name="RemoteDesktop"></a>Avaa virtuaalikoneen Etätyöpöytä ja Viimeistele määritys

1.  Kun valmistelu on valmis, valitse virtuaalikoneen DASHBOARD-sivun nimi. Valitse **Yhdistä**-sivun alareunassa.

2.  Valitse Windows Etätyöpöytä-ohjelmalla rpd tiedoston avaamiseen (`%windir%\system32\mstsc.exe`).

3.  **Windowsin suojaus** -valintaikkunaan avulla aiemmassa vaiheessa määritetyn paikallisen järjestelmänvalvojatilin salasana.
    (Sinulta saatetaan pyytää vahvistamiseksi virtuaalikoneen tunnistetietojen.)

4.  Voit kirjautua virtual tämän tietokoneen ensimmäistä kertaa useita prosesseja ehkä Viimeistele määritys työpöydän ja Windows-päivitysten ja valmistuminen Windows alkuperäinen määritystehtävät (sysprep) mukaan lukien. Windowsin sysprep päätyttyä SQL Serverin asennus on valmis määritystehtävät. Tehtäviä voi aiheuttaa viiveen muutaman minuutin, kun toimia. `SELECT @@SERVERNAME`oikea nimi ei voi palauttaa, kunnes SQL Serverin asennus on valmis, ja SQL Server Management Studiossa ei ehkä ole näkyvän aloitussivulla.

Kun olet muodostanut yhteyden Windows etätyöpöydän kanssa virtuaalikoneen, virtuaalikoneen toimii samankaltaisesti kuin toiseen tietokoneeseen. Yhteyden muodostaminen SQL Serverin kanssa (käytössä virtuaalikoneen) SQL Server Management Studiossa oletusesiintymää normaalisti.


##<a name="InstallIPython"></a>Asenna IPython muistikirjan ja muiden tukitiedostojen Työkalut

Voit määrittää oman uuden palvelimen SQL-AM, yhteyshenkilönä IPython muistikirjan-palvelin ja asenna hyödyt Lisätyökalut tällaisten AzCopy, Azure-tallennustilan Explorer tai hyödyllisiä tietoja tiede Python pakettien mukautusta komentosarjan annetaan sinulle. Asentaminen:

- **Windowsin Käynnistä** -kuvaketta hiiren kakkospainikkeella ja valitse **komentokehote (järjestelmänvalvojat)**
- Kopioi seuraavat komennot ja liitä komentokehotteen.

        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

- Kirjoita pyydettäessä salasana valittua IPython muistikirjan palvelimen.
- Mukauttaminen-komentosarjan automatisoi useita asennuksen jälkeinen ohjeita, jotka sisältävät:
    + Asennus ja IPython muistikirjan palvelimen asetukset
    + Avata aiemmin luotu päätepisteiden Windowsin palomuuri TCP-portit:
    + SQL Server etäyhteyksien varten
    + IPython muistikirjan remote palvelinyhteyden varten
    + Esimerkki IPython muistikirjojen ja SQL-komentosarjat haetaan
    + Lataaminen ja asentaminen hyödyllisiä tietoja tiede Python-paketit
    + Lataaminen ja asentaminen Azure työkaluilla, kuten AzCopy ja Azure-tallennustilan Explorer  
<br>
- Voi käyttää ja suorittaa IPython muistikirjan lomakkeen URL-Osoitteen avulla paikallisen tai etätietokoneen selaimen `https://<virtual_machine_DNS_name>:<port>`, jossa portti on valittuna varattaessa virtuaalikoneen IPython Julkinen portti.
- IPython muistikirjan palvelin on käynnissä taustalla ja käynnistetään automaattisesti, kun käynnistät virtuaalikoneen.

##<a name="Optional"></a>Liitä tiedot levyn tarpeen mukaan

AM kuva ei sisällä tietoja levyjä, eli levyjen kuin C-asema (OS levy) ja D-aseman (tilapäinen levy), jos haluat lisätä vähintään yhden tietojen levyjä tietojen tallentamiseen. SQL Server 2012 SP2: n yrityksen optimoitu DataWarehousing työmääriä AM kuva tulee esimääritettyjä SQL Serverin tietojen ja lokitiedostojen muita levyjen kanssa.

 > [AZURE.NOTE] Älä käytä D-aseman tietojen tallentamiseen. Nimensä se antaa vain väliaikaisten. Siinä on redundancy tai varmuuskopiointi ei, koska se ei sijaitsevat Azure-tallennustilan.

Jos haluat liittää tiedot levyjä, [miten liittäminen tietojen DVD-levyllä, Windows Virtual Machine](virtual-machines-windows-classic-attach-disk.md), joka opastaa kuvattu seuraavasti:

1. Liittäminen tyhjä valmisteltu aiemmissa vaiheissa virtuaalikoneen suorittamiseksi
2. Uusi näiden levyjen asemasta virtuaalikoneen alustus


##<a name="SSMS"></a>Yhteyden muodostaminen SQL Server Management Studiossa ja Monijärjestelmätila todennuksen käyttöön ottaminen

SQL Server-tietokantamoduuli ei voi käyttää Windows-todennuksen ilman toimialueen käyttäjät. Muodostaa tietokantamoduuli toisesta tietokoneesta määrittäminen SQL Server Monijärjestelmätila todennusta varten. Monijärjestelmätila todennuksen avulla SQL Server-todennus- ja Windows-todennusta. SQL-todennustila vaaditaan, jotta tiedot suoraan SQL Server AM tietokantoja [Azure koneen Learning Studio](https://studio.azureml.net) tietojen tuominen-moduulin ingest.

1.  Yhdistettynä virtuaalikoneen Etätyöpöydän avulla, käytä Windows **Search** -ruutua ja kirjoita **SQL Server Management Studiossa** (SMSS). Käynnistä SQL Server Management Studiossa (SSMS) napsauttamalla. Haluat ehkä lisätä pikakuvakkeen SSMS työpöytäsi myöhempää käyttöä varten.

    ![Käynnistä SSMS][5]

    Voit avata Management Studiossa ensimmäistä kertaa se on luotava käyttäjien Management Studiossa. Tämä voi kestää hetken.

2.  Avattaessa, Management Studiossa näkyy **Muodosta yhteys palvelimeen** -valintaikkunassa. Kirjoita **palvelimen nimi** -ruutuun virtuaalikoneen muodostaa objektin Resurssienhallinnassa tietokantamoduuli nimi.
    (Virtuaalikoneen nimen sijaan voit käyttää myös **(paikallinen)** tai piste **palvelimen nimi**. Valitse **Windows-todennusta**ja jätä ** *oman\_AM\_nimi*\\oman\_paikallisen\_järjestelmänvalvojan** **käyttäjänimi** -ruutuun. Valitse **Muodosta yhteys**.

    ![Muodostaa yhteyttä palvelimeen][6]

    <br>

     > [AZURE.TIP] SQL Server-todennustila Windows rekisteriavaimen muutoksesta tai SQL Server Management Studiossa voivat muuttua. Voit muuttaa todennustila käyttämällä rekisteriavaimen muutoksesta, aloita **Uusi kysely** ja suorita seuraavaa komentosarjaa:

        USE master
        go

        EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
        go


    Voit muuttaa käyttäen SQL Server management Studiossa todennustila seuraavasti:

3.  **SQL Server Management Studiossa Object Explorerissa**SQL Server (virtuaalikoneen nimi)-esiintymän nimi, napsauta hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**.

    ![Ominaisuudet:][7]

4.  Valitse **Suojaus** -sivulla **palvelintodennuksen**Valitse **SQL Server- ja Windows-todennusta-tilassa**ja valitse sitten **OK**.

    ![Valitse todennustila][8]

5.  Valitse **SQL Server Management Studiossa** -valintaikkunan toki vaatimus Käynnistä SQL Server uudelleen **OK** .

6.  **Objektin Explorer**palvelimellesi hiiren kakkospainikkeella ja valitse sitten **uudelleen**. (Jos SQL Server Agent on käynnissä, se on myös käynnistettävä.)

    ![Käynnistä uudelleen][9]

7.  Valitse **SQL Server Management Studiossa** -valintaikkunassa **Kyllä** , jos haluat samaa mieltä, että haluat käynnistää uudelleen SQL Server.

##<a name="Logins"></a>SQL Server-todennus kirjautumiset luominen

Voit muodostaa yhteyden tietokantamoduuli toisesta tietokoneesta, sinun on luotava vähintään yksi SQL Server-todennus kirjautuminen.  

Voit luoda uuden SQL Server-kirjautumiset ohjelmallisesti tai SQL Server Management Studiossa. Jos haluat luoda uuden sysadmin käyttäjän ohjelmallisesti SQL-todennusta, aloita **Uusi kysely** ja suorita seuraavaa komentosarjaa. Korvaa < uuden käyttäjänimi\> ja < uusi salasana\> kanssa valittua *käyttäjänimi* ja *salasana*. 


    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Säädä Salasanakäytäntö tarvittaessa (Esimerkki koodi poistaa käytöstä käytännön tarkistaminen ja salasanan vanheneminen). Katso lisätietoja SQL Server-kirjautumiset [Luo kirjautumisen](http://msdn.microsoft.com/library/aa337562.aspx).  

Voit luoda uuden SQL Server-kirjautumiset SQL Server Management Studiossa seuraavasti:

1.  **SQL Server Management Studiossa Object Explorerissa**Laajenna kansio server-esiintymän, johon haluat luoda uuden kirjautuminen.

2.  Napsauta **Suojaus** -kansiota hiiren kakkospainikkeella, valitse **Uusi**ja valitse **Login...**.

    ![Uusi kirjautuminen][10]

3.  Kirjoita **Kirjaudu sisään - uusi** -valintaikkunan **Yleiset** -sivulla uuden käyttäjän **Kirjautuminen nimi** -ruutuun nimi.

4.  Valitse **SQL Server-todennus**.

5.  Kirjoita salasana **salasana** -ruutuun uuden käyttäjän. Kirjoita salasana uudelleen **Vahvista salasana** -ruutuun.

6.  Voit pakottaa salasanan käytäntöasetukset monimutkaisuuden ja käyttäminen valitsemalla **Pakota Salasanakäytäntö** (suositus). Tämä on oletusarvo-vaihtoehto, kun SQL Server-todennus on valittuna.

7.  Jos haluat säilyttää käytäntöasetukset salasanan vanhentuvaksi, valitse **Pakota salasanan vanhentumisesta** (suositus). Pakota Salasanakäytäntö käyttöön tämän valintaruudun valitsemista. Tämä on oletusarvo-vaihtoehto, kun SQL Server-todennus on valittuna.

8.  Pakota käyttäjä voi luoda uuden salasanan käytetään kirjautuminen ensimmäisen kirjautumiskerran jälkeen, valitse **käyttäjän on muutettava salasana seuraavan kirjautumisen** (suositus sopiiko jonkun muun käyttää tätä kirjautuminen. Jos kirjautuminen on esimerkiksi omaa käyttöä, älä valitse tämä vaihtoehto.) Pakota salasanan vanhentumisesta käyttöön tämän valintaruudun valitsemista. Tämä on oletusarvo-vaihtoehto, kun SQL Server-todennus on valittuna.

9.  **Oletusarvo-tietokanta** -luettelosta oletusarvon tietokantaan sisäänkirjautumista varten. tämän asetuksen oletusarvo on **perusmuodon** . Jos et ole vielä luonut käyttäjä tietokannan, jätä tämän kohdan arvoksi **perustyyliin**.

10. Valitse **oletuskieli** -luettelosta jätä arvona **oletusarvon** .

    ![Kirjaudu sisäänkirjausominaisuudet][11]

11. Jos olet luomassa ensimmäinen sisäänkirjautuminen, haluat ehkä määrittää tämän kirjautumisen SQL Server-järjestelmänvalvojana. Tarkista tässä tapauksessa **sysadmin** **Palvelinrooleista** -sivulla.

    > [AZURE.IMPORTANT] Kiinteä palvelimen järjestelmänvalvojaryhmään jäsenillä on täydet hallintaoikeudet tietokantamoduulin. Tietoturvasyistä olisi huolellisesti rajoittaa tämän roolin jäsenyyttä.

    ![sysadmin][12]

12. Valitse OK.

##<a name="DNS"></a>Virtuaalikoneen DNS nimen määrittämiseen

Jos haluat muodostaa yhteyden SQL Server-tietokantamoduuli toisesta tietokoneesta, sinun on tiedettävä virtuaalikoneen (DNS, Domain Name System)-nimi.

(Tämä on Internetin käyttää tunnistamiseen virtuaalikoneen nimi. Voit käyttää IP-osoite, mutta IP-osoite saattavat muuttua, kun Azure siirtää redundancy tai resurssit. DNS-nimi on vakaata koska Voit uudelleenohjata IP-osoitteeseen.)

1.  Perinteinen Azure-portaalissa (tai valitse edellisessä vaiheessa) Valitse **NÄENNÄISKONEIDEN**.

2.  Etsi ja kopioi DNS-nimi, joka näkyy preceded mukaan **http://**virtuaalikoneen **VIRTUAALIKONEEN ESIINTYMÄT** -sivun **DNS-nimi** -sarakkeessa. (Käyttöliittymän eivät näy koko nimen, mutta voit napsauttamalla sitä hiiren kakkospainikkeella ja valitse Kopioi.)

##<a name="cde"></a>Yhteyden muodostaminen tietokantamoduuli toisesta tietokoneesta

1.  Avaa tietokoneessa, joka on yhteydessä Internetiin, SQL Server Management Studiossa.

2.  Kirjoita **Muodosta yhteys palvelimeen** tai **Database Engine Yhdistä** -valintaikkunan **palvelimen nimi** -ruutuun (määritetään Edellinen tehtävä) virtuaalikoneen ja julkisen päätepisteen porttinumero muodossa *DNSName, PortinNumero* , kuten **tutorialtestVM.cloudapp.net,57500**DNS-nimi.

3.  Valitse **todennus** -ruudussa **SQL Server-todennusta**.

4.  Kirjoita **Kirjaudu sisään** -ruutuun nimi, jonka loit aiempi tehtävä kirjautuminen.

5.  Kirjoita **salasana** -ruutuun salasana, jonka luot aiempi tehtävä kirjautumisnimi.

6.  Valitse **Muodosta yhteys**.

##<a name="amlconnect"></a>Yhteyden muodostaminen tietokantamoduuli Azure koneen oppiminen

Ryhmän tietojen tiede prosessin myöhemmässä vaiheessa voit käyttää [Azure Konepohjaisten Oppimistekniikoiden Studio](https://studio.azureml.net) voivat laatia ja kehittää koneen learning mallit. SQL Server AM tietokantojen ingest suoraan Azure koneen Learning koulutusta tai **Tuontitiedot** -moduulin näkyvissä pistemäärä, käyttää uuden [Azure koneen Learning Studio](https://studio.azureml.net) kokeessa. Tässä artikkelissa käsitellään lisätietoja ryhmän tietojen tiede prosessin opas linkkien kautta. Katso esittely, [Azure koneen Learning Studio ominaisuudet?](machine-learning-what-is-ml-studio.md).

2.  Valitse [tuontitiedot-moduulin](https://msdn.microsoft.com/library/azure/dn905997.aspx) **Ominaisuudet** -ruudussa **Azure SQL-tietokannan** **Tietolähde** avattavasta luettelosta.

3.  Kirjoita **Tietokantapalvelimen nimi** -tekstiruutuun`tcp:<DNS name of your virtual machine>,1433`

4.  Kirjoita SQL-käyttäjänimi **palvelimen käyttäjänimi** -tekstiruutuun.

5.  Kirjoita sql-käyttäjän salasanan **palvelimen käyttäjätilin salasana** -tekstiruutuun.

    ![Azure ML tuo tiedot][13]

##<a name="shutdown"></a>Sulkeminen ja poistaa varauksen virtual machine, kun ne eivät ole käytössä

Azure-Virtuaalikoneissa laitteiden hinnoittelu kuin **maksaa vain käyttötarkoitus**. Jos haluat varmistaa, että sinulla on ei laskutettava käytettäessä ei virtuaalikoneen, sen on oltava **pysäytetty (Deallocated)** -tilassa.

> [AZURE.NOTE] Suljetaan virtuaalikoneen sisällä (käyttämällä Windows power asetukset)-AM lopetetaan mutta pysyy varattu. Jotta sinun ei ole laskutettava pysäyttämällä näennäiskoneiden [Azure perinteinen Portal](http://manage.windowsazure.com/). Voit myös lopettaa soittamalla ShutdownRoleOperation "PostShutdownAction" ja "StoppedDeallocated" yhtä AM PowerShellin kautta.

Sammuta ja poista virtuaalikoneen varaus:

1. Kirjaudu sisään käyttämällä tiliä [Azure perinteinen Portal](http://manage.windowsazure.com/) .  

2. Valitse vasemman reunan siirtymispalkissa **NÄENNÄISKONEIDEN** .

3. Valitse näennäiskoneiden luettelo virtuaalikoneen nimeä Valitse Siirry **RAPORTTINÄKYMÄ** -sivulle.

4. Valitse sivun alareunassa **Sammuta**.

![AM sulkeminen][15]

Virtuaalikoneen Varaus poistettu, mutta sitä ei poisteta. Virtuaalikoneen saattaa uudelleen milloin tahansa perinteinen Azure-portaalista.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Azure SQL-palvelin-AM käyttövalmiuden: Mitä seuraavaksi?

Virtuaalikoneen on nyt valmis käytettäväksi tietojen tiede-harjoituksissa. Virtuaalikoneen on myös valmis käytettäväksi IPython muistikirjan palvelimeksi tarkasteluun ja tiedot ja yhdessä muiden tehtävien käsittely Azure koneen oppiminen ja ryhmän tietojen tiede prosessi (TDSP).

Seuraavat vaiheet tietojen tiede prosessin yhdistetään [Ryhmän tietojen tiede prosessin](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja voi sisältää ohjeet, joita tietojen siirtäminen HDInsight, käsitellä ja kuuntele sinne valmistelussa learning tiedoista Azure Konepohjaisten Oppimistekniikoiden.


[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png
 
