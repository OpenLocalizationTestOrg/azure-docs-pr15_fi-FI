<properties
   pageTitle="Azure SQL-tietokanta-Web- ja Business Edition Auringonlasku usein kysytyt kysymykset | Microsoft Azure"
   description="Ota selvää, kun Azure SQL-Web- ja Business-tietokantoja poistettu käytöstä ja ominaisuuksista ja uusi palvelutasot toimintoja."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Web- ja Business-version Auringonlasku usein kysytyt kysymykset

Azure SQL-Web- ja Business tietokantoja on nyt poistettu käytöstä. Basic, Vakio, Premium ja Lisää joustavasti tasoa korvaa retiring Web- ja Business-tietokannat.

Voit helpottaa päivittämiseen verkko- ja Business tietokannan, SQL-tietokanta-palvelun suosittelee haluamasi palvelun taso ja suorituskyky-tason (hinnoittelu taso) tietokantojen sen historiallisten kuormituksen perusteella.

**Jos haluat saada hinnat taso suositukset:**

- [SQL-tietokannan Vipuventtiili V12 päivityksen Azure-portaalissa](sql-database-upgrade-server-portal.md)
- [SQL-tietokannan Vipuventtiili V12 päivityksen PowerShellin avulla](sql-database-upgrade-server-powershell.md)
- [Muuta verkossa tai Business tietokannan hinnoittelu taso](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Miksi Azure portaalin näkyy Omat Web- ja Business edition tietokantoja poistettu käytöstä?

Koska Web- ja Business edition-tietokannat eivät ole käytettävissä jälkeen syyskuu 2015-portaalin nimet Internetin kautta tai Business tietokantaa kuin poistettu käytöstä. Käytöstä poistettuja-otsikko toimii myös, että verkko- ja Business tietokantoja päivittää vakio, Basic tai Premium muistutus. Lisätietoja aiemmin luotujen tietokantojen verkossa tai Business-päivityksen uudet palvelutasot on artikkelissa [Azure SQL-tietokannan Vipuventtiili V12 päivittäminen](sql-database-upgrade-server-portal.md).

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Mitä uusi palvelutaso on paras valinta aiemmin verkossa tai Business tietokannan, päivittämään?

Tiettyjen ominaisuuksien ja suorituskyvyn vaatimukset sovelluksen valitseminen sopivalle uusi palvelu tason ja suorituskyvyn tasolle aiemmin verkossa tai Business tietokannan määräytyy.

Hinnoittelu taso suositukset yläpuolelle tai yksityiskohtaiset tiedot on kuvattu avulla voit helpottaa valitsemalla haluamasi uusi palvelutaso on artikkelissa [Azure SQL-tietokannan Vipuventtiili V12 päivitys](sql-database-upgrade-server-portal.md).

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Miksi Microsoft esittely uusi palvelutasot?

Asiakaspalautteen perusteella Azure SQL-tietokanta on ottanut käyttöön uuden palvelutasot, joka auttaa asiakkaita Lisää tukevat helposti relaatiotietokannasta toiminnoista. Uusi tasoa on suunniteltu ennakoitavissa suorituskyky olisi yli seitsemän käyttöoikeustasot, jotka himmennetty näkyvä tapahtumien sovelluksen tarpeisiin valikoiman. Lisäksi uusi tasoa tarjouksen liiketoiminnan jatkuvuuden ominaisuuksista, vahvempi toiminta-aika-SLA suurempi vähemmän rahaa ja parannettu laskutuksen kokemus tietokannan koot solualueen.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Missä Lisää uusi palvelutasot voit lukea?

Lisätietoja uuden palvelutasot ja suorituskyvyn malli on [palvelutasot](sql-database-service-tiers.md). Alla on tietoja uusi palvelutasot yksityiskohtaiset kuvattu [SQL-tietokantaan hinnat](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Mitä ominaisuuksia tai toimintoja ei ole käytettävissä Basic, Vakio ja Premium-palveluissa?

Federations-ominaisuus poistettu käytöstä ja verkko- ja Business-versioissa. Asiakkaiden asteikko-kohtaa tietokantansa käyttäville on sen sijaan tai siirtää [joustavasti Tietokantatyökalut](sql-database-elastic-scale-get-started.md) [Azure SQL-tietokantaan](sql-database-elastic-scale-get-started.md), joka helpottaa luomisesta ja hallinnasta sharding käyttävä sovellus. .NET asiakkaan kirjaston avulla voit määrittää, kuinka tiedot on määritetty shards ja reitittää OLTP pyynnöt sopivien tietokantojen sovellukset. Hallintatoiminnot, joka määrittää, kuinka tiedot jaetaan shards kesken tukemaan Azure cloud-palvelun mallin sisältyvällä, että voit halutessasi isännöidä Azure tilaus. Lisäksi [joustavasti Tietokantatyökalut](sql-database-elastic-scale-get-started.md)Microsoft säilyvät luominen ja julkaiseminen [mukautetun sharding kuvioiden ja käytännöt ohjeet](https://msdn.microsoft.com/library/azure/dn764977.aspx) learnings laaja asiakkaan työt-perusteella. Mittakaava, toimintojen käyttäville asiakkaille kannattaa kuitata ulos [joustavasti Tietokantatyökalut](sql-database-elastic-scale-get-started.md) ja/tai ota yhteyttä Microsoftin Support arvioida niiden asetukset.

Microsoft on myös muuttaminen tietokannan kopion kokemusta Premium-tietokannat. TIETOKANNAN luominen aiemmin premium tietokannan kiintiö on rajoitettu... Tiedoston kopion olevan T-SQL luotu hyllytetty Premium-tietokantaan ilman varatut resurssit, joka on yhtä paljon liiketoiminnan tietokantana veloitetaan. Premium kiintiö on nyt enemmän vapaasti käytettävissä, hyllytetty Premium ei enää tueta. Tietokannan kopiot luodaan nyt sama edition ja suorituskyvyn tason lähteenä ja laskutetaan vastaavasti. Asiakkaat voivat valita eri taso tai suorituskyvyn tasolle pienentää niiden halutessasi kopioidun tietokantojen muuntaminen. Aiemmin luotujen hyllytetty Premium tietokantojen muunnetaan Business-version, tässä versiossa osana. On suunniteltu [Ajankohta palauttamisen](sql-database-recovery-using-backups.md#point-in-time-restore) käyttöönotto vähentää ei tarvitse tehdä varmuuskopioita tietokannat.

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Miten Basic, Vakio ja Premium parantaa laskutuksen käyttökokemus?

Perustiedot, Vakio, Premium Azure SQL-tietokantoja on laskuttaa tunnin mukaan ja käyttäjä pystyy skaalata tietokantojen ylös tai alas. Ovat laskuttaa kiinteää korkoa palvelun taso ja suorituskyvyn parhaan tunnin välein valitseminen perusteella. Lisäksi suorituskyky tasot (Esimerkki: Basic, S1 ja P2) jakaudu laskutusosoite tietokannan päivää/tunnit yksi kuukausi suorituskyvyn kunkin tason aiheutuneet määrä on helpompaa. Web- ja Business tietokannan edelleen laskuteta tietokannan yksiköt tietokannan koon perusteella. Voit vierailla [SQL-tietokantaan hinnat sivun](https://azure.microsoft.com/pricing/details/sql-database/) lisätietoja hinnat ja uusi palvelutasot erot.


## <a name="see-also"></a>Katso myös

[Azure SQL-tietokanta](https://azure.microsoft.com/documentation/services/sql-database/)

[Web- ja Business hinnat](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Palvelutasot](sql-database-service-tiers.md)

[Azure SQL-tietokannan Vipuventtiili V12 päivittäminen](sql-database-upgrade-server-portal.md)
