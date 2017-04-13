<properties
   pageTitle="Excelin yhdistäminen Hadoop rakenteen ODBC-ohjaimella | Microsoft Azure"
   description="Opettele määrittäminen ja käyttäminen Microsoft rakenne ODBC-ohjain Excelin kyselyn tiedot HDInsight-klusterin."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Hadoop Excelin yhteyttä Microsoftin rakenne ODBC-ohjain

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Microsoftin Big datasta ratkaisu integroitu Microsoft Business Intelligence (BI) osat, jotka on otettu mukaan Azure Hdinsightiin Apache Hadoop klustereiden. Esimerkki integrointi on mahdollisuus yhdistää Excelin rakenne-tietovarasto Hadoop-klusterin sisään käyttämällä Microsoft rakenne Open Database Connectivity (ODBC)-ohjain Hdinsightista.

On myös mahdollista muodostaa HDInsight-klusterin ja muista lähteistä, mukaan lukien muiden (-HDInsight) Hadoop-klustereiden, Excel, Microsoft Power Query-apuohjelman avulla Excel liittyviä tietoja. Lisätietoja asentamisesta ja Power Queryn avulla on artikkelissa [Hdinsightiin Power Queryn yhdistäminen Excel][hdinsight-power-query].

> [AZURE.NOTE] Tässä artikkelissa kuvattuja voidaan käyttää Linux- tai Windows-pohjaisesta HDInsight-klusterin, kun Windows tarvitaan asiakastietokoneen.

**Edellytykset**:

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **HDInsight-klusterin**. Luoda, on artikkelissa [Azure Hdinsightiin käytön aloittaminen][hdinsight-get-started].
- Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013: n erillisversio tai Office 2010 Professional Plus **A työasema** .


##<a name="install-microsoft-hive-odbc-driver"></a>Asenna Microsoft rakenne ODBC-ohjain

Lataa ja asenna Microsoft rakenne ODBC-ohjain [Download Centeristä][hive-odbc-driver-download].

Tämä ohjain voidaan asentaa 32-bittisen tai 64-bittiset versiot Windows 7: ssä, Windows 8, Windows 10: ssä, Windows Server 2008 R2 tai Windows Server 2012 ja antaa yhteyden Azure Hdinsightiin (1,6 ja uudempi versio) ja Azure Hdinsightista emulaattorin (v.1.0.0.0 ja sitä uudemmissa versioissa). Asenna versio, joka vastaa sovelluksen, jossa voit käyttää ODBC-ohjain version. Tässä opetusohjelmassa ohjaimen käytetään Office Excelistä.

##<a name="create-hive-odbc-data-source"></a>Luo rakenne ODBC-tietolähde

Seuraamalla näitä ohjeita noudattamalla voit luoda rakenne ODBC-tietolähde.

1. Windows 8: ssa tai Windows 10 Paina Windows-näppäin avaa aloitusnäytön ja kirjoita sitten **tietolähteet**.
2. Valitse Office-version mukaan **määrittäminen ODBC-tiedot lähteistä (32-bittinen)** tai **ODBC-tietolähteiden (64-bittinen) määrittäminen** . Jos käytössäsi on Windows 7, valitse **Valvontatyökalut** **ODBC-tietolähteiden (32-bittinen)** tai **ODBC-tietolähteiden (64-bittinen)** . Tämä käynnistää **ODBC-tietolähteiden hallinta** -valintaikkuna.

    ![OBDC tietolähteiden hallinta][img-hdi-simbahiveodbc-datasource-admin]

3. Napsauta Avaa **Luo uusi tietolähde** ohjattu **Lisää** käyttäjän DNS.
4. Valitse **Microsoft ODBC-ohjain rakenne**ja valitse sitten **Valmis**. Tämä käynnistää **Microsoft rakenne ODBC ohjaimen DNS-asetukset** -valintaikkunassa.

5. Kirjoita tai valitse seuraavat arvot:

    Ominaisuus|Kuvaus
    ---|---
    Tietolähteen nimi|Kirjoita tietolähteen nimi
    Host (isäntä)|Kirjoita &lt;HDInsightClusterName >. azurehdinsight.net. Esimerkiksi myHDICluster.azurehdinsight.net
    Port (portti)|Käytä <strong>443</strong>. (Tämä portti on vaihdettu-563 443.)
    Tietokannan|Käytä <strong>Oletus</strong>.
    Rakenne-palvelintyyppi|Valitse <strong>rakenne palvelimen 2</strong>
    Järjestelmä|Valitse <strong>Azure HDInsight-palvelu</strong>
    HTTP-polku|Jätä se tyhjäksi.
    Käyttäjänimi|Kirjoita HDInsight-klusterin käyttäjänimen. Tämä on käyttäjänimi, luodaan klusteri valmisteleminen aikana. Jos olet käyttänyt nopeasti Luo, oletusarvo käyttäjänimi on <strong>järjestelmänvalvoja</strong>.
    Salasana|Kirjoita HDInsight klusterin käyttäjän salasana.
    </table>

    On joitakin tärkeitä parametreja pitää mielessä, kun valitset **Lisäasetukset**:

    Parametri|Kuvaus
    ---|---
    Käyttää alkuperäisiä kyselyjä|Kun se valitaan, ODBC-ohjain ei yritä TSQL muuntaminen HiveQL. On käytettävä vain, jos olet 100 %: n että lähetät täysin HiveQL lauseita. Kun muodostaa yhteyden SQL Server tai Azure SQL-tietokantaan, sinun tulee jättää valitsematta.
    Rivit on noudettu kohti estäminen|Kun haetaan tietueiden suuria määriä säätäminen parametriä saattaa edellyttää Varmista paras suorituskyky.
    Oletusarvoinen merkkijonon sarakkeen pituus, binaarisarakkeen pituus, desimaaleja mittakaavan|Tietotyyppi pituudet ja precisions voi vaikuttaa siihen, miten tiedot palautetaan. Ne aiheuttavat virheellisten tietojen vuoksi menetyksiä tarkkuutta ja/tai katkaisu palautetaan.


    ![Lisäasetukset][img-HiveOdbc-DataSource-AdvancedOptions]

6. Valitse Testaa tietolähteen **testaaminen** . Kun tietolähde on määritetty oikein, se näyttää *testien onnistui!*.
7. Valitse **OK** ja sulje Testaa-valintaikkuna. Uusi tietolähde nyt listattu **ODBC-tietolähteiden hallinta**.
8. Valitse **OK** , sulje ohjattu toiminto.

##<a name="import-data-into-excel-from-hdinsight"></a>Tietojen tuominen Excel-HDInsight

Seuraavia ohjeita kuvaavat tapa rakenne-taulukon tietojen tuominen Excel-työkirjan käyttämällä ODBC-tietolähde, jonka loit edellä kuvatut vaiheet.

1. Avaa uusi tai aiemmin luotu työkirja Excelissä.
2. Valitse **tiedot** -välilehdessä **muista tietojen**lähteistä ja valitse sitten **Ohjattu tietoyhteyden muodostaminen** käynnistää **Ohjatun tietoyhteyden muodostamisen**.

    ![Avaa ohjattu tietoyhteyden muodostaminen][img-hdi-simbahiveodbc.excel.dataconnection]

3. Valitse **ODBC DSN** tietolähteenä, ja valitse sitten **Seuraava**.
4. ODBC-tietolähteistä Valitse tietolähteen nimi, jonka loit edellisessä vaiheessa ja valitse sitten **Seuraava**.
5. Kirjoita ohjatun toiminnon klusterin salasana uudelleen ja valitse sitten Tarkista määritykset uudelleen, **Testaa** tarvittavat.
6. Valitse **OK** ja sulje Testaa-valintaikkuna.
7. Valitse **OK**. Odota, **Valitse tietokanta ja taulukko** -valintaikkunassa Avaa. Tämä voi kestää joitakin sekunteja.
8. Valitse taulukko, jonka haluat tuoda, ja valitse sitten **Seuraava**. *Hivesampletable* on rakenteen esimerkkitaulukko HDInsight klustereiden mukana tulevaa.  Voit valita se, jos et ole vielä luonut. Lisätietoja Suorita rakenne kyselyt ja rakenteen taulukoiden luominen, katso [Käytä rakenne ja HDInsight][hdinsight-use-hive].
8. Valitse **Valmis**.
9. **Tietojen tuominen** -valintaikkunassa voit muuttaa tai määrittää kyselyn. Voit tehdä sen valitsemalla **Ominaisuudet**. Tämä voi kestää joitakin sekunteja.
10. Valitse **määritys** -välilehden ja liittää sitten **raja 200** **komennon teksti** tekstiruutuun rakenteen select-lauseessa. Muutoksen rajoittaa palautettujen tietueiden arvoksi 200.

    ![Yhteyden ominaisuudet][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Valitse Sulje yhteyden ominaisuudet-valintaikkunassa **OK** .
12. Valitse **OK** ja sulje **Tuontitiedot** -valintaikkuna.  
13. Kirjoita salasana uudelleen ja valitse sitten **OK**. Kestää joitakin sekunteja, ennen kuin tiedot saa tuotu Exceliin.

##<a name="next-steps"></a>Seuraavat vaiheet

Tämän artikkelin opit käyttämään Microsoft rakenne ODBC-ohjain HDInsight-palvelu noutaa tietoja Exceliin. Vastaavasti voit hakea tietoja HDInsight-palvelusta SQL-tietokantaan. On myös mahdollista ladata tietojen HDInsight-palvelun esittely. Lisätietoja on artikkeleissa:

- [Analysoi viive lentotietoihin käyttämällä Hdinsightiin][hdinsight-analyze-flight-data]
- [Tietojen lataaminen Hdinsightiin][hdinsight-upload-data]
- [Sqoop käyttäminen Hdinsightiin] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
