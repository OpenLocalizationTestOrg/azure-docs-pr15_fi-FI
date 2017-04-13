<properties
    pageTitle="Azure SQL-tietokantaan palvelun taso ja suorituskyvyn tason muuttaminen | Microsoft Azure"
    description="Muuta palvelutaso ja suorituskyvyn tasoon Azure SQL-tietokantaan esitetään, kuinka voit skaalata SQL-tietokantaan, ylös tai alas. Muuttaminen Azure SQL-tietokantaan hinnoittelu taso."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Palvelun taso ja suorituskyvyn tason muuttaminen (hinnoittelu taso) SQL-tietokanta Azure-portaalissa


> [AZURE.SELECTOR]
- [**Azure portal**](sql-database-scale-up.md)
- [PowerShellin](sql-database-scale-up-powershell.md)


Palvelutasot ja suorituskyvyn tasoon kuvaavat ominaisuuksia ja resursseista SQL-tietokantaan ja päivittää sovelluksen muutoksen tarpeisiin. Lisätietoja on artikkelissa [Palvelutasot](sql-database-service-tiers.md).

Huomaa, että palvelun tason muuttaminen ja/tai tietokannan suorituskykytaso Luo alkuperäisen tietokannan tasolla suorituskykyä ja vaihtaa sitten se yhteyksien. Tietoja ei häviä vaihdon mutta aikana lyhyt hetken, kun olemme siirtää se, tietokannan yhteydet on poistettu käytöstä, jotta joidenkin tapahtumien lennon saattaa peruutettava. Tässä ikkunassa vaihtelee, mutta on keskimäärin noin neljän sekunnin ja 99 % tapauksista on alle 30 sekuntia. Erittäin harvoin etenkin jos on suuri lento on tällä hetkellä yhteyksiä tapahtumien määrä on poistettu käytöstä, tässä ikkunassa voi olla pidempi.  

Koko asteikon ylöspäin prosessin kestoa määräytyy tietokannan koon ja palvelun taso ennen ja sen jälkeen muuta. Esimerkiksi 250 gt tietokannassa, jossa voit, kohteesta tai kuluessa vakiorivejä, taso muuttamista täyttävän 6 tunnin kuluessa. Tietokannalle samankokoinen, joka muuttuu suorituskyvyn tasojen Premium-palvelutaso se valmistuu 3 tunnin kuluessa.


[Azure SQL-tietokannan palvelutasot](sql-database-service-tiers.md) ja suorituskyvyn tasoon tietojen avulla voit määrittää haluamasi taso ja suorituskyvyn tason Azure SQL-tietokantaan.

- Tietokannan muuntaminen, tietokanta on oltava pienempi kuin kohde-palvelutaso enimmäiskoko. 
- Päivitystä tietokannan kanssa [Geo replikoinnin](sql-database-geo-replication-overview.md) käytössä, ennen ensisijainen luodun tietokannan on päivitettävä ensin sen toissijainen tietokantojen haluamasi suorituskyvyn taso.
- Kun päivitysprosessin palvelutaso, kaikki Geo replikoinnin yhteydet on ensin Lopeta. 
- Palauta-palveluja määräytyvät eri palvelutasot. Jos sinulla on päivitysprosessin saattavat menettää mahdollisuuden palauttaa pisteeseen senhetkinen tai jos käytössäsi on alemman varmuuskopion säilytysaika. Lisätietoja on artikkelissa [Azure SQL-tietokannan varmuuskopiointi ja palauttaminen](sql-database-business-continuity.md).
- Hinnat taso tietokannan vaihtaminen ei vaikuta max tietokannan koon. Voit muuttaa tietokannan enimmäiskoon käyttämällä [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) tai [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- Tietokannan uusia ominaisuuksia ei käytetä, ennen kuin muutokset ovat valmiit.



**Tässä artikkelissa suorittamiseen tarvitset seuraavat:**

- Azure SQL-tietokantaan. Jos sinulla ei ole SQL-tietokantaan, luo yhden noudattamalla tämän artikkelin: [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Tietokannan palvelun taso ja suorituskyvyn tason muuttaminen


Avaa skaalattava ylös tai alas tietokannalle SQL-tietokanta-sivu:

1.  Valitse **Lisää palveluja** [Azure portal](https://portal.azure.com)- > **SQL-tietokannat**.
2.  Valitse tietokanta, jota haluat muuttaa.
3.  Valitse **SQL-tietokanta** -sivu **hinnoittelu taso (asteikko DTUs)**:

    ![hinnat taso][1]

1.  Valitse uusi taso ja valitse **Valitse**:

    Valitsemalla **Valitse** lähettää asteikko pyyntö muuttaa hinnoittelu taso. Sen mukaan, tietokannan kokoa skaalaus-toiminto voi kestää jonkin aikaa valmiina (katso tämän artikkelin yläosassa tiedot).

    > [AZURE.NOTE] Hinnat taso tietokannan vaihtaminen ei vaikuta max tietokannan koon. Voit muuttaa tietokannan enimmäiskoon käyttämällä [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) tai [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).

    ![Valitse hinnoittelu taso][2]

3.  Valitse oikeassa yläkulmassa ilmoituskuvaketta (tili):

    ![ilmoitukset][3]

    Napsauta ilmoituksen tekstiä Avaa tiedot-ruutu, jossa lukee pyynnön tilaa.




## <a name="next-steps"></a>Seuraavat vaiheet

- [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) tai [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx)tietokannan max koon muuttaminen.
- [Skaalaa, ja valitse](sql-database-elastic-scale-get-started.md)
- [Yhdistä ja kyselyjen kanssa SSMS SQL-tietokanta](sql-database-connect-query-ssms.md)
- [Vie Azure SQL-tietokanta](sql-database-export.md)

## <a name="additional-resources"></a>Lisäresursseja

- [Liiketoiminnan jatkuvuus yhteenveto](sql-database-business-continuity.md)
- [SQL-tietokanta-asiakirjat](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
