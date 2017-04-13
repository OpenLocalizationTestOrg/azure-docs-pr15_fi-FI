<properties
    pageTitle="Laajennettu tapahtumien SQL-tietokantaan | Microsoft Azure"
    description="Tässä artikkelissa kuvataan laajennettu tapahtumien (XEvents) Azure SQL-tietokantaan ja miten tapahtuman istuntojen vaihdella hiukan Microsoft SQL Server-istunnot tapahtuma."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""
    tags=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="genemi"/>


# <a name="extended-events-in-sql-database"></a>Laajennettu tapahtumien SQL-tietokantaan

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Tässä ohjeaiheessa kerrotaan kuinka käyttöönoton laajennettu tapahtumat Azure SQL-tietokanta on hieman erilainen verrattuna Microsoft SQL Server laajennettu tapahtumat.


- SQL-tietokannan Vipuventtiili V12 kokemuksen laajennettu tapahtumat-toiminnon toinen puolet kalenterin 2015.
- SQL Server on ollut laajennettu tapahtumien 2008 jälkeen.
- Laajennettu tapahtumien SQL-tietokantaan ominaisuus on SQL Serverin ominaisuuksia tehokkaat alijoukkoa.


*XEvents* on epäviralliset kutsumanimi, jota käytetään joskus 'laajennettu tapahtumien' blogit ja epäviralliset muista sijainneista.


> [AZURE.NOTE] Lokakuussa 2015 saavuttaman laajennettu tapahtuman istunnon-toiminto on aktivoitu Azure SQL-tietokantaan esikatselu tasolla. Yleinen käytettävyys (GA)-päivämäärä ei ole vielä määritetty.
>
> Azure [Palvelupäivityksistä](https://azure.microsoft.com/updates/?service=sql-database) -sivulla on viestit, kun GA ilmoitukset tehdään.


Lisätietoja laajennetun tapahtumien Azure SQL-tietokanta ja Microsoft SQL Server on saatavilla kohdassa:

- [Pikaopas: SQL Serverin laajennettu tapahtumat](http://msdn.microsoft.com/library/mt733217.aspx)
- [Laajennettu tapahtumat](http://msdn.microsoft.com/library/bb630282.aspx)


## <a name="prerequisites"></a>Edellytykset


Tässä artikkelissa oletetaan, että jotkin tuntemus jo:


- [Azure SQL-tietokanta-palvelun](https://azure.microsoft.com/services/sql-database/).


- [Laajennettu tapahtumien](http://msdn.microsoft.com/library/bb630282.aspx) Microsoft SQL Server.
 - Microsoftin laajennettu tapahtumien ohjeista usean koskee SQL Server- ja SQL-tietokantaan.


Edellisen näyttäminen seuraavat on hyötyä, kun [kohde](#AzureXEventsTargets)tapahtumatiedoston valitseminen:


- [Azure tallennuspalvelu](https://azure.microsoft.com/services/storage/)


- PowerShellin
 - [Azure PowerShellin Azure-tallennustilan kanssa](../storage/storage-powershell-guide-full.md) – on täydellinen PowerShellistä ja Azure tallennuspalvelu tietoja.


## <a name="code-samples"></a>MALLIKOODEJA


Aiheeseen liittyviä ohjeita on kaksi MALLIKOODEJA:


- [Soi puskurin kohde koodi laajennettu tapahtumien SQL-tietokantaan](sql-database-xevent-code-ring-buffer.md)
 - Lyhyt yksinkertainen Transact-SQL-komentosarja.
 - Olemme korostaa koodin otoksen artikkelissa, kun olet käsitellyt soi puskurin kohde, olisi vapautat sen resursseja suorittamalla alter avattavan `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` lause. Voit myöhemmin lisätä esiintymässä soi puskurin mukaan `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Tapahtuman tiedostojen kohde koodi laajennettu tapahtumien SQL-tietokantaan](sql-database-xevent-code-event-file.md)
 - Vaihe 1 on PowerShellin Azure-tallennustilan säilö luomiseen.
 - Vaihe 2 on Transact-SQL, joka käyttää Azure-tallennustilan säilö.


## <a name="transact-sql-differences"></a>Transact-SQL-erot


- [Luoda tapahtuman istunnon](http://msdn.microsoft.com/library/bb677289.aspx) -komennon suorittaminen SQL Server, voit käyttää **Edelleen SERVER** -lause. Mutta SQL-tietokantaan **Edelleen tietokanta** -lause sen sijaan.


- **Edelleen tietokanta** -lause koskee myös [Muuttaa tapahtuman istunnon](http://msdn.microsoft.com/library/bb630368.aspx) ja [PUDOTA tapahtuman istunnon](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-komentoja.


- Paras käytäntö on tapahtuman istunnon voi sisällyttää **STARTUP_STATE = ON** **Luoda tapahtuman istunnon** tai **muuttaa tapahtuman istunnon** -lausekkeisiin.
 - **= ON** arvo tukee automaattisen uudelleenkäynnistyksen jälkeen uudelleenmääritys looginen tietokannassa automaattisesti vuoksi.


## <a name="new-catalog-views"></a>Uusi luettelo-näkymät


Laajennettu tapahtumat-toiminto tukee useita [luettelon näkymiä](http://msdn.microsoft.com/library/ms174365.aspx). Luettelon näkymien kertoa *metatiedot tai määritysten* käyttäjän luoma tapahtuman istuntojen nykyisessä tietokannassa. Näkymien ei palauta tietoja active tapahtuman istuntojen esiintymät.


| Nimi<br/>luettelo-näkymä | Kuvaus |
| :-- | :-- |
| **sys.database_event_session_actions** | Palauttaa rivi kutakin kuhunkin tapahtumaan tapahtuma-istunnon. |
| **sys.database_event_session_events** | Palauttaa jokaisen tapahtuman rivin tapahtuma-istunnossa. |
| **sys.database_event_session_fields** | Palauttaa rivi kutakin mukauttaminen-pysty saraketta, jotka on nimenomaisesti määrittää tapahtumien ja kohteet. |
| **sys.database_event_session_targets** | Palauttaa rivin tapahtuman kunkin kohteen tapahtumaa-istunnossa. |
| **sys.database_event_sessions** | Palauttaa rivin tapahtuma-istunnossa SQL-tietokanta-tietokannassa. |


Microsoft SQL Server-samalla luettelon näkymien on nimet, jotka sisältävät *.server\_ * sijaan *.database\_*. Nimi-kaava on esimerkiksi **sys.server_event_%**.


## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Uuden dynaaminen hallinta näkymät [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)


Azure SQL-tietokanta on [dynaaminen hallinta näkymät (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) , jotka tukevat laajennettu tapahtumat. DMVs kerro *tapahtuman istuntojen* .


| DMV nimi | Kuvaus |
| :-- | :-- |
| **sys.dm_xe_database_session_event_actions** | Palauttaa tietoja tapahtuman istunnon toiminnot. |
| **sys.dm_xe_database_session_events** | Palauttaa istunnon tapahtumien tietoja. |
| **sys.dm_xe_database_session_object_columns** | Näyttää objektit, jotka on sidottu istunnon määritysten arvot. |
| **sys.dm_xe_database_session_targets** | Palauttaa istunnon kohteiden tietoja. |
| **sys.dm_xe_database_sessions** | Palauttaa rivin, joka on määritetty tapahtuma-istunnossa nykyiseen tietokantaan. |


Microsoft SQL Server-samalla luettelon näkymät ovat nimeltään ilman * \_tietokannan* osa, kuten:


- **sys.dm_xe_sessions**, sen sijaan, että nimi<br/>**sys.dm_xe_database_sessions**.


### <a name="dmvs-common-to-both"></a>Yleisiä sekä DMVs


Laajennettu tapahtumien on muita käytettävissä olevia kirjasimia Azure SQL-tietokanta ja Microsoft SQL Server DMVs:


- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Etsi käytettävissä olevista laajennettu tapahtumat, toiminnot ja kohteet


Voit suorittaa yksinkertaisen SQL- **Valitse** saat luettelon käytettävissä olevista tapahtumista, toiminnot ja kohde.


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>SQL-tietokantaan tapahtuma-istunnot kohteet


Seuraavassa on kohteet, jotka voit siepata tuloksia tapahtuma-istunnot SQL-tietokantaan:


- [Soi puskurin kohde](http://msdn.microsoft.com/library/ff878182.aspx) - pitää lyhyesti tapahtumatiedot muistiin.
- [Tapahtuman laskuri kohde](http://msdn.microsoft.com/library/ff878025.aspx) - laskee kaikki tapahtumat, jotka esiintyvät laajennetun events-istuntoon.
- [Tapahtuman tiedostojen kohde](http://msdn.microsoft.com/library/ff878115.aspx) - kirjoituksia valmis puskuri Azuren tallennustilaan säilöön.


[Tapahtuman jäljitys for Windows (tapahtumien seuranta)](http://msdn.microsoft.com/library/ms751538.aspx) API ei ole käytettävissä laajennettu tapahtumien SQL-tietokantaan.


## <a name="restrictions"></a>Rajoitukset


Muutama tietoturvaan liittyvät erotusten befitting SQL-tietokantaan cloud-ympäristössä:


- Laajennettu tapahtumat ovat sillä yhden vuokraajan eristystaso mallin perusteella. Tapahtuma-istunnon yhtä tietokantaa ei voi käyttää tietoja tai tapahtumien toisesta tietokannasta.

- Lähetät ei voi **Luoda tapahtuman istunnon** lauseen kontekstissa **perusmuodon** tietokannan.


## <a name="permission-model"></a>Käyttöoikeus-malli


**Oikeudet** -käyttöoikeuksia on oltava tietokannan antaa **Luoda tapahtuman istunnon** -lause. Tietokannan omistaja (dbo) on **oikeudet** .


### <a name="storage-container-authorizations"></a>Tallennustilan säilö lupa


Voit luoda Azure-tallennustilan säilö SAS tunnuksen on määritettävä **rwl** käyttöoikeudet. **Rwl** -arvo sisältää seuraavat oikeudet:


- Luku
- Kirjoittaminen
- Luettelo


## <a name="performance-considerations"></a>Suorituskykyyn liittyviä tietoja


Missä laajennettu tapahtumien tehostettu käyttö saavutustasopisteet Lisää aktiivisen muistin kuin on kunnossa yleistä järjestelmän skenaariot on. Tämän vuoksi Azure SQL-tietokanta-järjestelmän dynaamisesti asettaa ja säätää aktiivisen muistin, joka on kerätty tapahtuma-istunnon koskevat rajoitukset. Monet eri tekijät salliva asetus dynaaminen laskentaan.


Jos näyttöön tulee virhesanoma, jossa lukee muistiin enimmäismäärä on voimassa, voi kestää joitakin korjaavat toiminnot ovat:


- Suorita vähemmän samanaikaisia tapahtuma-istuntoja.


- Kautta oman **Luo** ja **Muuta** lauseet tapahtuman istunnoissa, vähentää muistin, voit määrittää **Maks\_MUISTIN** lause.


### <a name="network-latency"></a>Verkon latenssin


**Tapahtuman tiedostojen** kohde kärsiä verkon latenssin tai epäonnistuu aikana Azuren tallennustilaan BLOB pysyvä tiedot. Muita tapahtumia SQL-tietokantaan voi viivästyä, kun ne Odota suorittamiseen verkkoliikennettä. Tämä viive voi hidastaa havainnollistamiseen.

- Vältä pienentämään suorituskyvyn riskin asettaminen **EVENT_RETENTION_MODE** vaihtoehto **NO_EVENT_LOSS** tapahtuman-istunnon määritykset.


## <a name="related-links"></a>Aiheeseen liittyvät linkit


- [Azure PowerShellin Azure tallennustilan kanssa](../storage/storage-powershell-guide-full.md).
- [Azure-tallennustilan cmdlet-komennot](http://msdn.microsoft.com/library/dn806401.aspx)


- [Azure PowerShellin Azure-tallennustilan kanssa](../storage/storage-powershell-guide-full.md) – on täydellinen PowerShellistä ja Azure tallennuspalvelu tietoja.
- [Opi käyttämään .NET-Blob-objektien tallennustilaan](../storage/storage-dotnet-how-to-use-blobs.md)


- [Luo TUNNISTETIEDON (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [LUODA tapahtuman istunto (Transact-SQL)](http://msdn.microsoft.com/library/bb677289.aspx)


- [Jonathan Kehayias blogimerkinnät Microsoft SQL Server-laajennettu tapahtumista](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


Muut koodin otoksen aiheet laajennettu tapahtumien on osoitteessa on seuraavissa linkeissä. Sinun on säännöllisesti tarkistettava näet, onko malli on tarkoitettu Microsoft SQL Server Azure SQL-tietokanta ja minkä tahansa otoksen. Sitten voit päättää, onko otosten suorittamiseen tarvittavaa pieniä muutoksia.


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
