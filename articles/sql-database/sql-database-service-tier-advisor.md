<properties 
   pageTitle="Hinnat taso suosituksia Azure SQL-tietokanta" 
   description="Vaihdettaessa hinnat tasoa Azure-portaalissa, taso suosituksia hinnat ovat, jos suositellun taso, joka sopii parhaiten aiemmin Azure SQL-tietokantaan päivän työmäärän suorittamista varten. Hinnoittelu tasoa kuvaavat SQL-tietokantaan palvelun taso ja suorituskyvyn taso." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>SQL-tietokantaan hinnoittelu taso suositukset

 Hinnoittelu taso suosituksia Ehdota service tason ja suorituskyvyn taso, joka sopii parhaiten aiemmin Azure SQL-tietokantaan päivän työmäärän käynnissä.

> [AZURE.NOTE] Hinnoittelu taso suositukset ovat vain käytettävissä verkko- ja Business tietokannan ja joustavasti tietokannan jakavat--ja vain [Azure portal](https://portal.azure.com/).


Hae hinnat taso suosituksia aikana seuraavia toimintoja:

- [Palvelun taso ja suorituskyvyn tason muuttaminen (hinnoittelu taso) SQL-tietokanta](sql-database-scale-up.md)
- [Päivitä Azure SQL Serverin Vipuventtiili V12](sql-database-upgrade-server-portal.md)
- Siirry Vipuventtiili V12 palvelimellesi. Katso [SQL-tietokantaan hinnat taso suosituksia](sql-database-service-tier-advisor.md).
- [Joustavasti tietokanta-ryhmän luominen](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Yleiskatsaus

SQL-tietokanta-palvelun analysoi nykyisen suorituskyvyn ja ominaisuuksien vaatimusten mukaan arvioidaan historiallisten resurssien käyttö SQL-tietokantaan. Lisäksi pienin hyväksyttävä palvelutaso määritetään koko tietokanta ja käytössä [Liiketoiminnan jatkuvuus](sql-database-business-continuity.md) ominaisuuksien perusteella. 

Nämä tiedot analysoidaan service tason ja suorituskyvyn taso, joka sopii parhaiten suorittamisen tietokannan Normaali työmäärä ja ylläpito on nykyisen ominaisuusjoukko suositellaan.

- Palvelun tutkii edellisen 15-30 päivän, historiatiedot (resurssien käyttö, tietokannan kokoa ja tietokannan toiminta) ja suorittaa vertailun määrää resursseja, ja tällä hetkellä käytettävissä palvelutasot ja suorituskyvyn tasoon todellinen rajoitukset.
- Tietoja analysoidaan 15 sekunnin välein kunkin aikavälin tulosjoukon on jakaa aiemmin palvelutaso ja suorituskyvyn taso, joka sopii parhaiten käsittelyyn kyseisen tulosjoukon työmäärää.
- Nämä 15 sekunnin esimerkkejä, joiden on sitten kootaan suurempi 15 – 30 päivän analyysi ja palvelun taso ja suorituskyvyn taso, voit käsitellä optimaalisesti 95 % historiallisten työmäärää suositellaan.

### <a name="recommendations"></a>Suosituksia

Tietokannan käytön mukaan on tällä hetkellä 2 luokkia suositukset, jotka voivat ilmetä:


| Suositus | Kuvaus |
| :--- | :--- |
| Päivittäminen | Päivitä uusi taso. |
| Ei ole käytettävissä | Tietokannan edellyttää vähintään työmäärää tai noin 35 päivän toiminnan. Ei ole kelvollinen suositus antamaan riittävästi tietoja. |

## <a name="getting-pricing-tier-recommendations"></a>Käytön hinnat taso suositukset

Hae hinnoittelu taso suosituksia valitsemalla aiemmin luodun sivuston tai Business-tietokannan, valitse **kaikki asetukset**ja sitten **hinnoittelu taso (DTUs-asteikko)**. (Hinnoittelu taso suositukset ovat käytettävissä myös milloin voit [päivittää Azure SQL Serverin Vipuventtiili V12](sql-database-upgrade-server-portal.md).)

1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse **SELAA** > **SQL-tietokannat**.
4. Valitse tietokanta, jonka haluat nähdä suositus **SQL-tietokantoja** -sivu:

    ![Valitse tietokanta][1]

5. Tietokannan-sivu, valitse **kaikki asetukset** **hinnoittelu taso (DTUs-asteikko)**.


7. **Suositus hinnat tasoa** Avaa kohtaa, johon voit sitten ehdotettu taso ja valitse sitten muutettava taso, **Valitse** -painiketta.

    ![Esikatselu rekisteröityminen][4]

8. Myös valitsemalla **käyttö tietojen tarkasteleminen** , Avaa **Hinnat taso suositus tiedot** -sivu, jossa voit tarkastella tietokannan suositeltu taso, toiminto vertailun nykyisen ja suositellut tasoa ja historiallisten resurssien käyttö-analyysi-kaaviota.

    ![Esikatselu rekisteröityminen][5]



## <a name="summary"></a>Yhteenveto

Hinnoittelu taso suosituksia on automaattinen sisäänrakennetun telemetriatietojen kunkin SQL-tietokannan tietojen keräämistä ja suositellaan parhaat palvelun taso ja suorituskyvyn tason yhdistelmä tietokannan todellisen suorituskyvyn tarpeet ja ominaisuuksien vaatimusten perusteella. Valitse asetukset-sivu napsauttamalla saat näkyviin hinnat taso suosituksia Web- ja Business tietokantoja **hinnoittelu taso (DTUs-asteikko)** .



## <a name="next-steps"></a>Seuraavat vaiheet

Tietyn tietokannan tiedot, mukaan suorittamiseen päivitys tai artikkelissa yleensä ei suoriteta välittömästi. Portaalin antaa ilmoitukset tietokannan siirtymät, sen uuden tason tai kyselyt SQL-tietokantapalvelimen tietokannasta [sys.dm_operation_status (Azure SQL-tietokanta)](https://msdn.microsoft.com/library/dn270022.aspx) -näkymässä voit valvoa päivityksen tila.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
