<properties
    pageTitle="SQL-tietokannan Vipuventtiili V12 käyttökokemuksen päivityksen suunnitteleminen | Microsoft Azure"
    description="Tässä artikkelissa kuvataan valmistelut ja rajoitukset, jotka osallistuvat päivitystä versioon Vipuventtiili V12, Azure SQL-tietokanta."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Suunnittelu ja SQL-tietokannan Vipuventtiili V12 päivityksen valmisteleminen


Tässä ohjeaiheessa kerrotaan suunnittelu ja sinun on suoritettava päivittämään Azure SQL-tietokantoja versiosta V11 Vipuventtiili V12 valmistelut.


Uuden [Azure-portaalissa](https://portal.azure.com/) on käytettävissä tukemaan Vipuventtiili V12-käyttökokemuksen päivityksen.


Seuraavassa taulukossa on lueteltu muita ohjeaiheita Vipuventtiili V12.


| Otsikko ja linkki | Sisällön kuvaus |
| :--- | :--- |
| [SQL-tietokannan Vipuventtiili V12 uudet ominaisuudet](sql-database-v12-whats-new.md) | Tässä artikkelissa kuvataan tietoja siitä, miten Vipuventtiili V12 tuo lähemmäksi Azure SQL-tietokannan koko välistä eroa Microsoft SQL Serverin kanssa. |
| [SQL-tietokannan Vipuventtiili V12 tietokannan luominen](sql-database-get-started.md) | Tässä artikkelissa kuvataan, miten voit luoda uuden Azure SQL-tietokannan versio Vipuventtiili V12. Se kuvaa lisäksi vain tyhjän tietokannan eri vaihtoehtoja. |


## <a name="plan-ahead"></a>Suunnittele


Seuraavissa jaksoissa kuvataan oppiminen ja päätöksentekoa tulee ratkaista ennen actions kohti Vipuventtiili V12 Azure SQL-tietokannan päivittäminen kestää.

### <a name="version-clarification"></a>Version selventää


Tämä asiakirja koskee Microsoft Azure SQL-tietokannan päivittäminen versiosta V11 Vipuventtiili V12. Lisää varsinaisesti versioita ovat seuraavat kaksi arvoa lähellä ilmoittaa Transact-SQL-lause **Valitse @@version; ** :


- 12.0.2000.8 *(tai uudempi-bittinen Vipuventtiili V12)*
- 11.0.9228.18 *(V11)*
 - Joskus V11 on myös nimitystä SAWA v2.

### <a name="service-tier-planning"></a>Palvelun taso suunnittelu


Vipuventtiili V12 alkaen Azure SQL-tietokanta tukee vain nimeltä Basic, Vakio ja Premium palvelutasot. Web- ja Business-palvelutaso ei tue Vipuventtiili V12. Siksi, jos haluat päivittää Azure SQL-tietokannan, joka on tällä hetkellä verkossa tai Business-palvelutaso, päätä sen uusi palvelutaso pitäisi olla.


Lisätietoja Basic, Vakio ja Premium-palvelutasot on:

- [SQL-tietokantaan palvelutasot](sql-database-service-tiers.md)
- [SQL-tietokannan Web/Business tietokantoja ksi uuden palvelutasot](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Tarkista Geo replikoinnin määritys


Jos Azure SQL-tietokanta on määritetty Geo replikoinnin, olisi asiakirjan sen nykyiset määritykset ja Lopeta Geo-replikoinnin, ennen kuin aloitat päivityksen valmistelu toiminnot. Kun päivitys on valmis Geo toistoa varten on määritettävä tietokannan.


Strategia kannattaa jättää lähteen ennalleen ja testaamista tietokannan kopion.


## <a name="preparation-actions"></a>Valmistelu toiminnot


Kun olet suorittanut suunnittelun, voit suorittaa toiminnon viimeinen vaihe päivityksen valmisteleminen seuraavissa jaksoissa kuvattujen vaiheiden.


Päivityksen viimeinen vaihe on kuvaukset ovat käytettävissä ohjeaiheet, jotka on linkitetty tämän ohjeaiheen yläreunassa.


### <a name="service-tier-actions"></a>Palvelun taso-toiminnot


Web- ja Business-palvelun hinnat taso ei tue Vipuventtiili V12.


Jos V11 Azure SQL-tietokanta on verkossa tai Business-tietokanta, päivitysprosessin on vaihdettava tietokannan tuetut taso. Päivityksen suosittelee taso, joka sopii tietokannan työmäärää historia. Voit kuitenkin tahansa pidät tuetut tasolla.


Voit pienentää vaiheista päivityksen aikana valitsemalla verkko-ja Business-tason poispäin V11 tietokannan ennen päivitystä. Voit tehdä tämän käyttämällä uuden [Azure-portaalissa](https://portal.azure.com/).


Jos et tiedä, mitä palvelutaso, voit siirtyä, vakio taso S2 taso voi olla järkevää alkuperäinen valinta. Minkä tahansa suppeampaan taso on vähemmän resursseja kuin Web- ja Business-taso oli.


### <a name="suspend-geo-replication-during-upgrade"></a>Keskeyttää Geo replikoinnin päivityksen aikana


Vipuventtiili V12 päivittäminen ei voi suorittaa, jos Geo replikoinnin on aktiivinen tietokannan. Voit lopettaa Geo replikoinnin tietokannan on määritettävä ensin uudelleen.


Päivityksen jälkeen voit määrittää tietokannan käyttämään Geo replikoinnin uudelleen.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Avaa portit Azure-AM asiakasyhteydet varten


Jos asiakasohjelman muodostaa yhteyden SQL-tietokannan Vipuventtiili V12 samalla, kun asiakkaan suoritetaan Azure virtuaalikoneen (AM), sinun on avattava AM seuraavia portti-alueita:

- 11000 11999
- 14000 14999


Napsauta [tätä](sql-database-develop-direct-route-ports-adonet-v12.md) tietoa SQL-tietokannan Vipuventtiili V12 portit. Portit tarvitaan suorituskykyparannukset-SQL-tietokannan Vipuventtiili V12 mukaan.


##<a id="limitations"></a>Rajoitukset aikana ja Vipuventtiili V12 päivittämisen jälkeen


### <a name="portals-for-v12"></a>Vipuventtiili V12 portaaleihin


On kolme portaaleihin Azure varten ja jokaisella on eri ominaisuuksien koskeva SQL-tietokannan Vipuventtiili V12.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Azure-portaalissa on uusi ja on vielä esikatselu tila. Portaalin ei ole vielä osoitteessa koko Yleiset käytettävyys (GA). Tässä portaalissa:
 - Voit hallita Vipuventtiili V12 palvelin ja tietokanta.
 - Voit päivittää V11 tietokannan Vipuventtiili V12.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Tämä perinteinen Azure-portaali ehkä myöhemmin vähennettävä asteittain. Tässä portaalissa:
 - Voit hallita Vipuventtiili V12 palvelin ja tietokanta.
 - Voit *ei ole* yhteyttä V11 tietokannan Vipuventtiili V12 päivitys.


- (http://*yourservername*. database.windows.net)<br/>Azure SQL-tietokannan perinteinen portaalissa:
 - Voit*ei* Vipuventtiili V12 palvelinten hallinta.


On suositeltavaa muodostaa Azure SQL-tietokannat ja Visual Studio 2015 (VS2015). VS2015 voi käyttää esimerkiksi seuraavia tehtäviä:


- Voit suorittaa Transact-SQL-lause.
- Voit suunnitella rakenne.
- Voit tehdä tietokannan, online- tai offline-tilassa.


Tai sen sijaan maksutta voit ladata [Visual Studio yhteisön 2015](https://www.visualstudio.com/vs/community/), joka on täydet toiminnot sisältävien ohjelmien VS2015 versio.


Vanhat Azure perinteinen-portaalissa, tietokanta-sivulla voit valita **avoinna Visual Studiossa** käynnistää VS2015 yhteyden muodostamisesta Azure SQL-tietokanta tietokoneessa.


Toinen vaihtoehto voit SQL Server Management Studiossa (SSMS) 2014 kanssa [CU6](http://support.microsoft.com/kb/3031047/) muodostaa Azure SQL-tietokantaan. Lisätietoja rajaamisesta on blogikirjoituksessa ovat:<br/>[Asiakkaan tooling päivitykset Azure SQL-tietokantaan](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Rajoitus *aikana* päivityksen Vipuventtiili V12


V11 tietokanta on edelleen käytettävissä tietojen käytön Vipuventtiili V12 päivityksen aikana. On vielä muutama huomioon otettavat rajoituksia.


| Rajoitus | Kuvaus |
| :--- | :--- |
| Päivityksen kesto | Päivityksen kestoa määräytyy kokoa, edition ja palvelimen tietokantojen määrä. Päivitysprosessin suorittamisen tuntia päivää palvelimia erityisesti niiden palvelimet, joilla on tietokantoja:<br/><br/>*50 megatavun tai<br/> * Premium palvelutaso-palvelussa<br/><br/>Päivityksen aikana palvelimessa uusien tietokantojen luominen kasvattaa päivityksen kesto. |
| Geo ei ole sallittuja | GEO replikointia ei tueta Vipuventtiili V12 palvelimessa, joka liittyy V11 päivitys. |
| Tietokanta ei ole käytettävissä lyhyesti viimeisen vaiheen Vipuventtiili V12 päivitys: | Tietokantojen kuuluvat V11 palvelimeen säilyvät päivityksen aikana. Yhteys palvelimeen ja tietokantoja on kuitenkin lyhyesti käytettävissä viimeinen vaihe, kun päälle Vaihda aloittaa siitä V11 valmis Vipuventtiili V12.<br/><br/>Vaihda kuluessa voi vaihdella 40 sekuntia 5 minuuttia. Useimmat palvelimet valitsin päälle odotetaan suorittamiseen sisällä 90 sekuntia. Vaihda ajan kuluessa kasvaa palvelimissa, joilla on suuri määrä tietokannat tai kun tietokannat on paljon kirjoittaminen toiminnoista. |


### <a name="limitation-after-upgrade-to-v12"></a>Vipuventtiili V12 rajoitus *jälkeen* päivittäminen


| Rajoitus | Kuvaus |
| :--- | :--- |
| Ei voi palauttaa V11 | Päivityksen käytönaikainen, kun tulos ei voi kumota tai kumottu. |
| Web- tai Business taso | Kun päivityksen käynnistyy, palvelimeen uuden tietokannan Vipuventtiili V12 voi enää tunnista tai hyväksy verkossa tai Business-palvelutaso. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Viedä ja tuoda *sen jälkeen, kun* päivityksen Vipuventtiili V12


Voi viedä tai tuoda Vipuventtiili V12 tietokannan [Azure-portaalissa](https://portal.azure.com/). Tai voit viedä tai tuoda käyttämällä seuraavia työkaluja:


- SQL Server Management Studiossa (SSMS)
- Visual Studio 2015
- Tietojen tason sovelluksen Framework (DacFx)


Kuitenkin työkalujen käyttämisestä on ensin on tai asenna niiden viimeisimmät päivitykset, jotta ne tue Vipuventtiili V12: n uusiin ominaisuuksiin:


- [SQL Server Management Studiossa 2014: n kumulatiivisen päivityksen 6](http://support2.microsoft.com/kb/3031047)
- [SQL Server-tietokannan Tooling Visual Studio 2013: n päivitys helmikuussa 2015](https://msdn.microsoft.com/data/hh297027)
- [Azure SQL-tietokannan Vipuventtiili V12 helmikuu 2015 tietojen tason sovelluksen Framework (DacFx)](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Edellisen työkalulinkkien on päivitetty tai sen jälkeen 2015 2 maaliskuussa. On suositeltavaa, että käytät työkaluilla uudempaa päivitykset.


#### <a name="automated-export"></a>Automaattinen vienti


Automaattinen Vie edelleen käytettävissä esikatselu.  Asiakkaan työkalun päivityksiä ei tarvita käytettäessä automaattinen Vie.  Jos haluat ottaa tuloksena tiedostoon ja tuoda paikallinen-palvelimeen, sillä päivitykset yllä tarvitaan onnistuisi.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>Palauttaa poistetun V11 tietokannan Vipuventtiili V12

Seuraavassa kerrotaan Vipuventtiili V12 Azure SQL-tietokanta-palvelimeen voi palauttaa poistettuja V11 Azure SQL-tietokantaan.

1. Oletetaan, että sinulla V11 Azure SQL-tietokantapalvelimeen. <br/> Palvelimessa on tietokannan osoitteessa vanhentuneen Web ja Business-palvelutaso.
2. Tietokannan poistaminen.
3. Voit päivittää palvelimen Vipuventtiili V12.
4. Seuraavaksi voit palauttaa tietokannan palvelimeen. <br/> Tietokanta on päivitetty siten Vipuventtiili V12 vakiorivejä tason S0 tasolla.
5. Tietokannan voi siirtyä tuettujen service minkä tahansa tason, jos S0 ei ole tarpeen.


### <a name="powershell-cmdlets"></a>PowerShellin cmdlet-komennot


PowerShellin cmdlet-komennot ovat käytettävissä aloittaa, lopettaa tai valvoa päivityksen Azure SQL-tietokannan Vipuventtiili V12 V11 tai muita pre-Vipuventtiili V12-versiosta.

- [SQL-tietokannan Vipuventtiili V12 päivityksen PowerShellin avulla](sql-database-upgrade-server-powershell.md)

Oppaat tietoja näiden PowerShell-cmdlet-kohdassa:


- [Hae AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Käynnistä AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Lopeta AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Lopeta-cmdlet-komento tarkoittaa peruuttaa, ei Keskeytä. Tällä ei voi palata päivityksen kuin alusta alkaen uudelleen. Lopeta-cmdlet-komento tyhjentää ja vapauttaa kaikki tarvittavat resurssit.


## <a name="failure-resolution"></a>Virhe tarkkuus


Jos päivitys epäonnistuu pariton jostakin syystä, V11 tietokannan pysyy aktiivinen ja käytettävissä normaalisti.


## <a name="related-links"></a>Aiheeseen liittyvät linkit


- Microsoft Azure [esikatselutoiminnot](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
