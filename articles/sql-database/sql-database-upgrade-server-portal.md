<properties
    pageTitle="Päivityksen Azure SQL-tietokannan Vipuventtiili V12 Azure-portaalissa | Microsoft Azure"
    description="Kerrotaan, miten voit päivittää Azure SQL-tietokannan Vipuventtiili V12 mukaan lukien verkko- ja Business tietokannan päivittäminen ja sen tietokantojen siirtyminen suoraan Azure-portaalissa joustavasti tietokanta-ryhmän V11 palvelimen päivittäminen."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Päivityksen Azure SQL-tietokannan Vipuventtiili V12 Azure-portaalissa


> [AZURE.SELECTOR]
- [Azure portal](sql-database-upgrade-server-portal.md)
- [PowerShellin](sql-database-upgrade-server-powershell.md)


SQL-tietokannan Vipuventtiili V12 on uusin versio, joten aiemmin palvelimien päivittäminen SQL-tietokannan Vipuventtiili V12 suositellaan.
SQL-tietokannan Vipuventtiili V12 on monia [etuja verrattuna edellisen version](sql-database-v12-whats-new.md) mukaan lukien:

- Parannettu yhteensopivuus SQL Serverin kanssa.
- Parannettu premium suorituskyky ja uusia suorituskyvyn tasoja.
- [Joustavasti tietokannan jakavat](sql-database-elastic-pool.md).

Tässä artikkelissa on ohjeita päivittämiseen aiemmin SQL-tietokannan V11 palvelimia ja tietokantoja SQL-tietokannan Vipuventtiili V12.

Vipuventtiili V12 päivittämisen aikana voit päivittää Web- ja Business tietokantoja, uusi palvelutaso käyttöohjeet päivittäminen Internetin kautta tai Business tietokantoja, joiden mukaan.

Lisäksi [joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool.md) siirtämisestä voi kustannusten tehokkaampaa päivittäminen yksittäisen tietokantojen yksittäisen suorituskyvyn tasot (hinnoittelu tasoa). Jakavat yksinkertaistaa myös tietokannan hallinta, koska haluat ryhmän sen sijaan, että yksittäiset tietokannat suorituskyky-käyttöoikeustasojen hallinta erikseen suorituskyky-asetusten hallinta. Jos sinulla on useita palvelimia tietokantojen harkitse siirtämisen saman Serveriin ja hyödyntämistä laskemisesta resurssivarantoon. Voit helposti [siirtää tietokantojen V11 palvelinten suoraan joustavasti tietokannan jakavat PowerShellin avulla](sql-database-upgrade-server-powershell.md). Voit käyttää myös portaalin V11 tietokantojen siirtäminen resurssivarantoon, mutta portaalissa on jo Vipuventtiili V12 palvelimeen ja luoda varannon. Ohjeet toimitetaan tämän artikkelin luoda altaan palvelimen päivityksen jälkeen, jos sinulla on [tietokantoja, jotka voit hyötyä resurssivarantoon](sql-database-elastic-pool-guidance.md).

Huomaa, että tietokantoja se ja toimivat päivitettäessä koko. Uusi suorituskyvyn taso tilapäinen pudottaminen tietokantaan yhteyksien todellinen siirtymä milloin voi esiintyä erittäin pienen ajan, joka on yleensä 90 sekuntia, mutta voi olla jopa 5 minuuttia. Jos sovellus on [lyhytkestoisia vika käsittelyn yhteyden päät](sql-database-connectivity-issues.md) on riittävä suorittaa yhteydet katkeavat päivitys lopussa.

SQL-tietokannan Vipuventtiili V12 päivittäminen ei voi kumota. Päivityksen jälkeen palvelin ei voi palauttaa V11.

Päivityksen Vipuventtiili V12 jälkeen [palvelun taso suosituksia](sql-database-service-tier-advisor.md) ja [joustavasti resurssivarantoon suorituskykyyn liittyviä tietoja](sql-database-elastic-pool-guidance.md) eivät heti ole käytettävissä, kunnes palvelulla oman työmääriä uudessa palvelimessa aikaa. V11 palvelimen suositus historia ei koske Vipuventtiili V12 palvelimet, niin se ei säily.

## <a name="prepare-to-upgrade"></a>Päivityksen valmisteleminen

- **Päivitä kaikki tietokannat Internetin kautta tai Business**: Katso [Päivitä kaikki Web- ja Business-tietokannat](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) osan alapuolella tai Katso [näyttö ja hallita joustavasti tietokanta-ryhmän (PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Tarkista ja Pysäytä Geo replikoinnin**: Jos Azure SQL-tietokanta on määritetty Geo replikoinnin olisi asiakirjan sen nykyiset määritykset ja [Lopeta Geo replikoinnin](sql-database-geo-replication-portal.md#remove-secondary-database). Kun päivitys on valmis uudelleen tietokannan Geo replikoinnin.
- **Avaa seuraavat portit, jos sinulla on Azure-AM-asiakkaat**: Jos asiakasohjelman muodostaa yhteyden SQL-tietokannan Vipuventtiili V12 samalla, kun asiakkaan suoritetaan Azure virtuaalikoneen (AM), sinun on avattava porttialueiden 11000 11999 ja 14000-14999 käyttöön AM. Lisätietoja on artikkelissa [SQL-tietokannan Vipuventtiili V12 portit](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>Päivityksen käynnistäminen

1. [Azure portal](https://portal.azure.com/) Selaa palvelimeen, jonka haluat päivittää valitsemalla **SELAA** > **SQL Server**-painiketta ja valitsemalla haluat päivittää v2.0-palvelimeen.
2. Valitse **Uusimman SQL-tietokannan päivitys**ja valitse sitten **Päivitä tähän palvelimeen**.

      ![Päivitä-palvelin][1]

3. Palvelimen päivittämisestä uusimman SQL-tietokannan päivitys on pysyvä ja voi peruuttaa. Vahvista päivitys, kirjoita palvelimen nimi ja valitse **OK**.

## <a name="upgrade-all-web-and-business-databases"></a>Päivitä kaikki Web- ja Business-tietokannat

Jos palvelimessa on verkossa tai Business tietokantoja on päivitettävä ne. SQL-tietokannan Vipuventtiili V12 päivittämisen aikana päivität Web- ja Business piilotetusta uusi palvelutaso.    

Voit helpottaa päivittämiseen, SQL-tietokanta-palvelun suosittelee tarvittavat palvelun taso ja suorituskyky-tason (hinnoittelu taso) tietokantojen. Palvelun suosittelee varten suorittamalla luodun tietokannan kuormituksen analysoiminen tietokannalle historiallista käyttö parhaat taso.

3. Valitse kunkin tarkistettava tietokanta ja valitse suositellut hinnat taso päivittämiseksi **päivittää tätä palvelinta** -sivu. Voit aina käytettävissä hinnoittelu tasoa Selaa ja valitse haluamasi vaihtoehto ympäristön sopii parhaiten.


     ![tietokannat][2]


7. Kun olet napsauttanut ehdotettu taso voit valita jommankumman ja **Valitse hinnoittelu taso** -sivu, jossa voit valita taso ja valitse sitten muutettava taso, **Valitse** -painike. Valitse kunkin Web- tai Business tietokannalle uuden taso.

    Mikä on tärkeää muistaa on, että SQL-tietokantaan ei lukittu millä tahansa tietyn palvelun taso tai suorituskyvyn tasolla, kyselyjä, joten tietokannan muutoksen vaatimukset voit helposti muuttaa käytettävissä palvelutasot ja suorituskykyä. Itse asiassa Basic, Vakio ja Premium SQL-tietokantoja laskuttaa tunnin mukaan ja käyttäjä pystyy skaalata tietokantojen ylös tai alas 4 kertaa 24 tunnin ajan kuluessa.

    ![suosituksia][6]


Kun kaikki tietokannat palvelimessa soveltuvat olet valmis aloittamaan päivityksen

## <a name="confirm-the-upgrade"></a>Vahvista päivitys

3. Kun palvelimessa kaikki tietokannat soveltuvat päivityksen haluat **Palvelimen nimi** ja varmista, että haluat suorittaa päivitys ja valitse sitten **OK**.

    ![Tarkista päivityksen][3]


4. Päivityksen käynnistyy ja näyttää tekstimuodossa edistymisen ilmoitus. Päivitysprosessin on aloitettu. Tietyn tietokantoja tietoja mukaan Vipuventtiili V12 päivittäminen voi kestää jonkin aikaa. Tänä aikana kaikki tietokannat palvelimessa säilyy online-tilassa, mutta palvelin ja tietokanta hallinnan toiminnot on rajoitettu.

    ![edistymisen päivittäminen][4]

    Uusi suorituskyvyn todellinen siirtymä milloin tason tilapäinen pudottaminen tietokantaan yhteyksiä voi esiintyä toistaminen erittäin pienen ajan (yleensä mitattuna sekuntia). Jos sovellus on lyhytkestoisia vika käsittely (uudelleen logiikan) yhteyden päät on riittävä suorittaa yhteydet katkeavat päivitys lopussa.

5. Kun päivitys-toiminto päättyy **Uusimman päivityksen** -sivu näyttää **käytössä**.

    ![Vipuventtiili V12 käytössä][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Tietokantojen siirtäminen joustavasti tietokannan resurssivarantoon

[Azure portal](https://portal.azure.com/) Vipuventtiili V12 palvelimeen Selaa ja valitse **Lisää resurssivarantoon**.

- tai -

Jos näyttöön tulee sanoma **napsauttamalla tätä saat suositellut joustavasti tietokannan jakavat tälle palvelimelle**, napsauta sitä avulla on helppo luoda, joka on tarkoitettu palvelimen tietokantojen resurssivarantoon. Lisätietoja on artikkelissa [hinnan ja suorituskyvyn Huomioitavaa joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-guidance.md).

![Ryhmän lisääminen palvelimeen][7]

Noudata Viimeistele oman resurssivarannon luominen [Luo joustavasti tietokannan varanto](sql-database-elastic-pool.md) on artikkelissa.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Näytön tietokantojen SQL-tietokannan Vipuventtiili V12 päivittämisen jälkeen

>[AZURE.IMPORTANT] Päivitä uusimpaan versioon, SQL Server Management Studiossa (SSMS) voit hyödyntää uusia Vipuventtiili v12 ominaisuuksia. [Lataa SQL Server Management Studiossa] (https://msdn.microsoft.com/library/mt238290.aspx).

Päivityksen jälkeen seurata aktiivisesti tietokantaan sovellusten käytössä haluamasi suorituskykyä ja optimoi asetuksia haluamallasi tavalla.

Lisäksi seuranta yksittäiset tietokannat, voit valvoa joustavasti tietokannan jakavat [valvonta, hallinta- ja joustavasti tietokanta-ryhmän Azure-portaalissa kokoa](sql-database-elastic-pool-manage-portal.md) tai [PowerShellin](sql-database-elastic-pool-manage-powershell.md)avulla.


**Kulutus resurssitietojen:** Basic, Vakio ja Premium tietokantojen kulutus resurssitietojen on käytettävissä [sys.dm_ db_ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV käyttäjän tietokannan kautta. Tämä DMV on lähellä reaaliaikaisesti kulutus resurssitietoa osoitteessa 15 toisen rakeisuuden edellisen tunnin toiminnon. Aikavälin DTU prosentti-kulutus lasketaan enimmäisprosenttiarvon kulutus suorittimen, IO ja kirjaudu dimensioista. Näin kyselyn keskimääräinen DTU prosentti kulutus päälle edellisen tunnin laskemiseen:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Seurannan lisätiedot:

- [Yksittäisen tietokantojen azure SQL-tietokanta suorituskyvyn ohjeita](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Hinnan ja suorituskyvyn Huomioitavaa joustavasti tietokannan resurssivarantoon](sql-database-elastic-pool-guidance.md).
- [Seurannan Azure SQL-tietokanta dynaaminen hallinta-näkymien käyttäminen](sql-database-monitoring-with-dmvs.md)




**Ilmoitukset:** Määrittää 'Ilmoitukset' Azure-portaalissa ilmoittaa, kun DTU kulutus päivitetyn tietokannan lähestyy tiettyjä korkean tason. Tietokannan ilmoituksia voi olla eri suorituskyvyn mittarit, kuten DTU, Suoritin tai IO Log Azure portaali asetukset. Tietokannan selaamalla ja valitse **ilmoitusten säännöt** **asetukset** -sivu.

Voit esimerkiksi määrittäminen sähköposti-ilmoituksen "DTU prosentteina", jos DTU prosentti keskiarvo ylittää 75 % viimeisen 5 minuuttia päälle. Lisätietoja ilmoitusten määrittämisestä [ilmoitusten](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) vastaanottaminen viittaavat.





## <a name="next-steps"></a>Seuraavat vaiheet

- [Tarkista altaan suosituksia ja luoda varannon](sql-database-elastic-pool-create-portal.md).
- [Muuttaa tietokannan palvelun taso ja suorituskykyä](sql-database-scale-up.md).



## <a name="related-links"></a>Aiheeseen liittyvät linkit

- [SQL-tietokannan Vipuventtiili V12 uudet ominaisuudet](sql-database-v12-whats-new.md)
- [Suunnittelu ja SQL-tietokannan Vipuventtiili V12 päivityksen valmisteleminen](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
