<properties
    pageTitle="SQL-tietokantoja tietojen synkronointi käytön aloittaminen"
    description="Tässä opetusohjelmassa avulla pääset alkuun SQL Azure-tietojen synkronointi (esikatselu)."
    services="sql-database"
    documentationCenter=""
    authors="jennieHubbard"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/11/2016"
    ms.author="jhubbard"/>


#<a name="getting-started-with-azure-sql-data-sync-preview"></a>Käytön aloittaminen Azure SQL-tietojen synkronointi (ennakkoversio)
Tässä opetusohjelmassa kerrotaan Azure SQL-tietojen synkronointi Azure perinteinen portaalissa perusteet.

Tässä opetusohjelmassa olettaa mahdollisimman vähän edellisen kokemusta SQL Server- ja Azure SQL-tietokantaan. Tässä opetusohjelmassa hybridi (SQL Server ja SQL-tietokantaan esiintymät) Synkronoi ryhmän täysin määritetty ja synkronoidaan aikataulussa, voit määrittää luominen.

> [AZURE.NOTE] Valmis teknisten Azure SQL tietojen synkronointi-aiemmin tallennettu MSDN määrittäminen on käytettävissä .pdf-tiedoston. Lataa se [tähän](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

## <a name="step-1-connect-to-the-azure-sql-database"></a>Vaihe 1: Yhdistä Azure SQL-tietokanta

1. Kirjautuminen [perinteinen Portal](http://manage.windowsazure.com).

2. Valitse vasemmanpuoleisessa ruudussa **SQL-TIETOKANNAT** .

3. Valitse **synkronointi** sivun alareunassa. Kun valitset SYNKRONOI, näkyviin ilmestyy luettelon, jossa voit lisätä - mitä **Synkronoi uuteen ryhmään** tai **Uusi synkronointi-agentti**.

4. Käynnistää ohjatun uuden SQL-tietojen synkronointi Agent, valitse **Uusi synkronointi-agentti**.

5. Jos et ole lisännyt agentti ennen **valitsemalla Lataa se tähän**.

    ![Image1](./media/sql-database-get-started-sql-data-sync/SQLDatabaseScreen-Figure1.PNG)


## <a name="step-2-add-a-client-agent"></a>Vaihe 2: Lisää asiakas-agentti
Tämä vaihe on pakollinen vain, jos aiot on synkronointi-ryhmän sisältyvät paikallisen SQL Server-tietokantaan. Jos Synkronoi-ryhmällä on SQL-tietokanta-esiintymät, siirry vaiheeseen 4.

<a id="InstallRequiredSoftware"></a>
### <a name="step-2a-install-the-required-software"></a>Vaihe 2 a: Asenna tarvittavat ohjelmistot
Varmista, että sinulla on asennettu tietokoneeseen, jos olet asentanut Client Agent seuraavasti.

- **.NET framework 4.0**

 Asenna .NET Framework 4.0 [täältä](http://go.microsoft.com/fwlink/?linkid=205836).

- **Microsoft SQL Server 2008 R2 SP1 järjestelmän CLR tyypit (x86)**

 Asenna Microsoft SQL Server 2008 R2 SP1 järjestelmän CLR tyypit (x86) [tähän](http://www.microsoft.com/download/en/details.aspx?id=26728)

- **Microsoft SQL Server 2008 R2 SP1 jaetut hallinta-objektit (x86)**

 Asenna Microsoft SQL Server 2008 R2 SP1 jaettu hallinta objektien (x86) [tähän](http://www.microsoft.com/download/en/details.aspx?id=26728)



<a id="InstallClient"></a>
### <a name="step-2b-install-a-new-client-agent"></a>Vaihe 2 b: Asenna uusi asiakas-agentti

Noudata [Asenna Client Agent (SQL-tietojen synkronointi)](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf) asentaminen agentti.



<a id="RegisterSSDb"></a>
### <a name="step-2c-finish-the-new-sql-data-sync-agent-wizard"></a>Vaihe 2c: ohjatun uuden SQL-tietojen synkronointi Agent

1.  Palaa ohjattuun uusi SQL-tietojen synkronointi agentti.
2.  Anna agentti kuvaava nimi.
3.  Valitse avattavasta valikosta **alue** (tietokeskuksen) Tämä agentti isännöimiseen.
4.  Valitse avattavasta valikosta **TILAUKSEN** isännöimiseen Tämä agentti.
5.  Napsauta oikealle osoittavaa nuolta.



## <a name="step-3-register-a-sql-server-database-with-the-client-agent"></a>Vaihe 3: Rekisteröidä Client Agent SQL Server-tietokantaan

Kun Client Agent on asennettu, voit rekisteröidä jokaisen paikallisen SQL Server-tietokantaan, jotka haluat sisällyttää synkronointi-ryhmässä agentti.
Haluat rekisteröidä tietokannan agentti, noudata osoitteessa [rekisteröidä Client Agent ja SQL Server-tietokantaan](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).



## <a name="step-4-create-a-sync-group"></a>Vaihe 4: Synkronointi ryhmän luominen


<a id="StartNewSGWizard"></a>
### <a name="step-4a-start-the-new-sync-group-wizard"></a>Vaihe 4 a: Käynnistä uuden synkronointi-ryhmän ohjattu

1.  Palaa [perinteiseen Portal](http://manage.windowsazure.com).
2.  Valitse **SQL-TIETOKANNAT**.
3.  Sivun alareunassa **Lisää SYNKRONOINNIN** , valitse sitten Synkronoi uuteen ryhmään laatikko.

    ![Image2](./media/sql-database-get-started-sql-data-sync/NewSyncGroup-Figure2.png)



<a id=""></a>
### <a name="step-4b-enter-the-basic-settings"></a>Vaihe 4: Kirjoita perusasetuksia


1.  Kirjoita synkronointi-ryhmän kuvaava nimi.
2.  Valitse avattavasta valikosta **alue** (tietokeskuksen) isännöimiseen Synkronoi tämän ryhmän.
3. Napsauta oikealle osoittavaa nuolta.

    ![Image3](./media/sql-database-get-started-sql-data-sync/NewSyncGroupName-Figure3.PNG)


<a id="DefineHubDB"></a>
### <a name="step-4c-define-the-sync-hub"></a>Vaihe 4c: Määritä synkronointi-toiminnossa

1. Valitse avattavasta valikosta Synkronoi ryhmä-toiminnossa yhteyshenkilönä SQL-tietokanta-esiintymä.
2. Kirjoita tämä SQL-tietokannan esiintymä - **TOIMINNOSSA käyttäjänimi** ja **Salasana KESKITTIMEEN**tunnistetiedot.
3. Odota SQL-tietojen synkronointi vahvistamaan käyttäjänimi ja salasana. Näkyviin tulee näkyviin oikealla puolella salasana tunnistetiedot vahvistuksen vihreä valintamerkki.
4. Valitse avattavasta valikosta **RISTIRIITOJEN ratkaisu** -käytäntö.

 **Pääsivuston voittaa** - toiminnossa tietokannan Kirjoita viittaus-tietokantoihin kirjoitettu muutoksia, korvattaessa muutokset saman viitata tietokannan tietueen. Toiminnallisesti Tämä tarkoittaa, että ensimmäinen muutos valitsemalla kirjoitettu välittää muihin tietokantoihin.


 **Asiakkaan voittaa** - keskittimeen kirjoitettu muutokset korvataan viittaus tietokantojen muutokset. Toiminnallisesti Tämä tarkoittaa, että kirjoitettu keskittimeen viimeisin muutos on jotakin, mitä ja muut tietokannat välitetty.

5.  Napsauta oikealle osoittavaa nuolta.

    ![Image4](./media/sql-database-get-started-sql-data-sync/NewSyncGroupHub-Figure4.PNG)

<a id="AddRefDB"></a>
### <a name="step-4d-add-a-reference-database"></a>Vaihe 4d: Lisää viittaus-tietokanta


Toista tämä vaihe kunkin toisen tietokannan haluat synkronointi-ryhmään.

1. Valitse avattavasta valikosta Lisää tietokanta.

    Avattavan luettelon ominaisuudet ovat molemmat SQL Server-tietokannat, joka on rekisteröity agentti ja SQL-tietokantaan esiintymät.
2.  Anna tunnistetiedot tämä tietokanta - **käyttäjänimi** ja **salasana**.
3.  Valitse avattavasta valikosta tietokannan **SYNKRONOINNIN SUUNTA** .

    **Kaksisuuntaisen** - viittaus tietokannassa kirjoitetaan keskittimeen tietokantaan ja keskittimeen-tietokantaan tehdyt muutokset on kirjoitettu viittaus-tietokantaan.

    **Synkronoi-toiminnosta** - tietokannan vastaanottaa päivityksiä-toiminnosta. Se ei lähetä muutokset valitsemalla.

    **Valitsemalla Synkronoi** - tietokannan lähettää päivitykset valitsemalla. Muutokset toiminto ei ole kirjoitettu tähän tietokantaan.

4.  Viimeistele synkronointi-ryhmän luominen valitsemalla oikeassa alakulmassa ohjatun toiminnon valintamerkki. Odota SQL-tietojen synkronointi Vahvista tunnistetiedot. Vihreä valintamerkki osoittaa, että tunnistetiedot on vahvistettu.

5.  Valitse valintamerkki toisen kerran. Tämä palauttaa SQL-tietokantoja Valitse **synkronointi** -sivulle. Synkronoi tämä ryhmä näkyy nyt muiden synkronointi ryhmiä ja agenttien vuoksi.

    ![Image5](./media/sql-database-get-started-sql-data-sync/NewSyncGroupReference-Figure5.PNG)


## <a name="step-5-define-the-data-to-sync"></a>Vaihe 5: Määritä tiedot, synkronointi

Azure SQL-tietojen synkronointi avulla voit valita taulukoita ja sarakkeita, jos haluat synkronoida. Jos haluat suodattaa sarakkeen siten, että vain rivit-arvoilla (kuten ikä > = 65) Synkronoi, käytä Azure SQL-tietojen synkronointi-portaalin ja niitä, valitse taulukot, sarakkeet ja rivit Synkronoi olevien tietojen määrittämisessä synkronoimaan.

1.  Palaa [perinteiseen Portal](http://manage.windowsazure.com).
2.  Valitse **SQL-TIETOKANNAT**.
3.  Valitse **synkronointi** -välilehti.
4.  Valitse synkronointi-ryhmän nimi.
5.  Valitse **SYNKRONOI säännöt** -välilehti.
6.  Valitse haluat antaa synkronointi-ryhmä rakenne-tietokanta.
7.  Napsauta oikealle osoittavaa nuolta.
8.  Valitse **Päivitä rakenne**.
9.  Valitse synkronoinnin lisättävät sarakkeet-tietokannan taulukkoon.
    - Sarakkeiden yhdistäminen ei tueta ei voi valita.
    - Jos valittuna on taulukon sarakkeita, taulukko ei sisälly synkronointi-ryhmä.
    - Valitse tai poista valinta kaikki taulukot, valitse Valitse näytön alareunassa.
10. Valitse **Tallenna**ja valitse Synkronoi-ryhmän Lopeta valmistelu odota.
11. Palaa tietojen synkronointi-aloitussivu, valitse Edellinen-nuoli (edellä synkronointi ryhmän nimen) näytön vasemmassa yläkulmassa.

    ![Image6](./media/sql-database-get-started-sql-data-sync/NewSyncGroupSyncRules-Figure6.PNG)

## <a name="step-6-configure-your-sync-group"></a>Vaihe 6: Määritä synkronointi-ryhmä

Voit synkronoida synkronointi ryhmän aina valitsemalla SYNKRONOI tietojen synkronointi sivun alareunassa.
Synkronoimaan toistu määrittäminen synkronointi-ryhmä.

1.  Palaa [perinteiseen Portal](http://manage.windowsazure.com).
2.  Valitse **SQL-TIETOKANNAT**.
3.  Valitse **synkronointi** -välilehti.
4.  Valitse synkronointi-ryhmän nimi.
5.  Valitse **määritys** -välilehti.
6.  **AUTOMAATTINEN SYNKRONOINTI**
    - Voit määrittää synkronointi-ryhmä, johon haluat synkronoida määrittäminen korkojakso valitsemalla **käytössä**. Voit synkronoida edelleen pyydettäessä valitsemalla SYNKRONOI.
    - Valitse synkronointi-ryhmä, johon haluat synkronoida vain, kun valitset SYNKRONOI määrittäminen **ei käytössä** .
7.  **SYNKRONOI KORKOJAKSO**
    - Jos automaattinen synkronointi on, Määritä synkronointi päivitysväli. Korkojakso on oltava 5 minuuttia-1 kuukausi.
8.  Valitse **Tallenna**.

![Image7](./media/sql-database-get-started-sql-data-sync/NewSyncGroupConfigure-Figure7.PNG)

Onnittelen. Olet luonut synkronointi-ryhmä, joka sisältää SQL-tietokanta-esiintymän ja SQL Server-tietokantaan.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja SQL-tietokantaan ja SQL-tietojen synkronointi on:

* [Lataa täydellinen SQL-tietojen synkronointi teknisten](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf)
* [SQL-tietokannan yleiskatsaus](sql-database-technical-overview.md)
* [Tietokannan elinkaaren hallinta](https://msdn.microsoft.com/library/jj907294.aspx)
 

 
