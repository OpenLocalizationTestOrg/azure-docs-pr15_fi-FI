<properties
    pageTitle="Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon Azure-portaalissa"
    description="Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon Azure-portaalissa"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-using-the-azure-portal"></a>Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon Azure-portaalissa

> [AZURE.SELECTOR]
- [Azure portal](sql-database-export.md)
- [PowerShellin](sql-database-export-powershell.md)

Tässä artikkelissa on ohjeita arkistointiin Azure SQL-tietokantaan (Azuren Blob-objektien tallennustilaan tallennettuja) BACPAC tiedostoon [Azure portal](https://portal.azure.com).

Kun haluat luoda arkiston Azure SQL-tietokantaan, voit viedä tietokannan rakenteen ja tiedot BACPAC tiedostoon. BACPAC-tiedosto on vain jonka tunniste on BACPAC ZIP-tiedoston. BACPAC tiedoston myöhemmin voidaan tallentaa Azure-blob-säiliö tai paikallinen tallennus paikallisen sijaintiin ja myöhemmin tuodut Edellinen Azure SQL-tietokantaan tai SQL Serveriin paikallisen asennuksen. 

***Huomioon otettavia seikkoja***

- Yhtenäistämistä muulla tavoin arkisto, varmista, että ei kirjoita tehtävän tapahtuu viennin aikana tai [muulla tavoin yhdenmukaisia kopioi](sql-database-copy.md) Azure SQL-tietokantaan vietävän.
- Azure-Blob-säiliö arkistoida BACPAC tiedoston enimmäiskoko on 200 gt. Suurempi BACPAC tiedoston paikalliseen tallennussijaintiin arkistoon käyttämällä [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komentorivi-apuohjelmaa. Tämän apuohjelman mukana Visual Studio ja SQL Server. Voit myös [ladata](https://msdn.microsoft.com/library/mt204009.aspx) SQL Server Data Tools saat tämän apuohjelman uusin versio.
- Arkistoinnin Azure premium säilöön BACPAC-tiedoston avulla, ei tueta.
- Jos vientitoiminnon ylittää 20 tuntia, se voidaan peruuttaa. Voit parantaa suorituskykyä viennin aikana seuraavasti:
 - Tilapäisesti suurentaa service-taso.
 - Lakkaa kaikki lukeminen ja kirjoittaminen tehtävän viennin aikana.
 - Muut kuin null-arvojen etsiminen kaikki suurten taulukoiden käyttäminen [ryhmitellyn indeksin](https://msdn.microsoft.com/library/ms190457.aspx) . Liitetty indeksit ilman vienti saattaa epäonnistua, jos kestää yli 6 – 12 tuntia. Tämä johtuu siitä Vie-palvelu on tehtävä taulukon tarkistus yrittää vie koko taulukko. Suorita **DBCC SHOW_STATISTICS** ja varmista, että *RANGE_HI_KEY* ei ole null ja sen arvo on hyvä jakauma on hyvä keino selvittää, jos taulukoissa on optimoitu Vie. Lisätietoja on artikkelissa [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).


> [AZURE.NOTE] BACPACs ei ole tarkoitettu voidaan käyttää varmuuskopiointi ja palauttaminen toimintoja. Azure SQL-tietokanta luo automaattisesti jokaisen käyttäjän tietokannan varmuuskopiot. Lisätietoja on artikkelissa [Liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md).

Tässä artikkelissa suorittamiseen tarvitset seuraavat:

- Azure tilaus.
- Azure SQL-tietokanta. 
- Ensin [Vakio Azure-tallennustilan tilin](../storage/storage-create-storage-account.md) blob-säilö BACPAC tallennuspaikoista vakio tallennustilan.

## <a name="export-your-database"></a>Vie tietokantaan

Avaa vietävä tietokannalle SQL-tietokanta-sivu.

> [AZURE.IMPORTANT] Voit varmistaa muulla tavoin yhdenmukaista BACPAC tiedoston olisi ensimmäisen [tietokannan kopion luominen](sql-database-copy.md) ja vie tiedot sitten tietokannan kopion. 

1.  Siirry [Azure portal](https://portal.azure.com).
2.  Valitse **SQL-tietokannat**.
3.  Valitse tietokanta arkistoon.
4.  Valitse SQL-tietokanta-sivu **Vie** Avaa **Vie tietokantaan** -sivu:

    ![Vie-painike][1]

5.  **Tallennustilan** ja valitse kohdassa tilin ja Blob-objektien säilytykseen mihin BACPAC tallennetaan:

    ![Vie tietokantaan][2]

6. Valitse todennustyyppi. 
7.  Kirjoita haluamasi todennus-tunnistetiedot Azure SQL-palvelimeen, jossa olet viemässä tietokannan.
8.  Valitse **OK** , jos haluat arkistoida tietokannan. Valitsemalla **OK** Luo Vie-tietokannan pyynnön ja lähettää sen-palveluun. Vie vievät pituutta määräytyy koosta ja monimutkaisuudesta tietokannan ja palvelutaso. Saat ilmoituksen.

    ![vienti-ilmoitus][3]

## <a name="monitor-the-progress-of-the-export-operation"></a>Vientitoiminnon etenemisen seuranta

1.  Valitse **SQL-palvelimiin**.
2.  Valitse palvelin, joka sisältää vain arkistoida alkuperäisen (lähde)-tietokannan.
3.  Vieritä toimintoja.
4.  Valitse **Tuo/Vie historia**SQL server-sivu:

    ![Tuo Vie historia][4]

## <a name="verify-the-bacpac-is-in-your-storage-container"></a>Tarkista, BACPAC on tallennustilan säilö

1.  Valitse **tallennustilan tilit**.
2.  Valitse tallennuspaikka BACPAC arkisto tallennustilan tili.
3.  **Säilöjen** ja valitse veit lisätietoja (Voit ladata ja tallentaa tästä BACPAC) tietokannasta säilö.

    ![.bacpac tiedostotiedot][5]  

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure SQL-tietokantaan BACPAC tuomisesta on artikkelissa [Tuo BACPCAC Azure SQL-tietokantaan](sql-database-import.md)
- Lisätietoja SQL Server-tietokantaan BACPAC tuomisesta on artikkelissa [Tuo BACPCAC SQL Server-tietokantaan](https://msdn.microsoft.com/library/hh710052.aspx)



<!--Image references-->
[1]: ./media/sql-database-export/export.png
[2]: ./media/sql-database-export/export-blade.png
[3]: ./media/sql-database-export/export-notification.png
[4]: ./media/sql-database-export/export-history.png
[5]: ./media/sql-database-export/bacpac-archive.png

