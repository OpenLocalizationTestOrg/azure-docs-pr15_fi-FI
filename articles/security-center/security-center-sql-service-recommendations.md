<properties
   pageTitle="Suojaaminen Azure SQL-palvelu Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tämän asiakirjan osoitteet Azure Tietoturvakeskus suositukset, jotka auttavat suojaamaan Azure SQL-palvelu ja jatkat suojauskäytäntöjen mukaisesti."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Azure Tietoturvakeskus suojaaminen Azure SQL-palvelu

Azure Tietoturvakeskus analysoi Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, Luo suositukset, joka opastaa määrittäminen tarvittavat ohjausobjektit.  Suositukset koskevat Azure resurssityypit: näennäiskoneiden (VMs), verkko-SQL- ja sovellukset.

Tässä artikkelissa käsitellään suositukset, jotka koskevat Azure SQL-palveluun.  Azure SQL service suosituksia Centerin ympärille Azure SQL-palvelimet ja tietokantojen valvonnan ja ottamalla käyttöön ottaminen käyttöön SQL-tietokantoja salausta.  Käytä alla olevassa taulukossa viitteenä voi auttaa sinua ymmärtämään käytettävissä SQL-palvelun suosituksia ja mitä kukin tehdä, jos haluat käyttää sitä.

## <a name="available-sql-service-recommendations"></a>Suosituksia käytettävissä SQL-palvelu

|Suositus|Kuvaus|
|-----|-----|
|[Ota käyttöön tarkistaminen SQL server](security-center-enable-auditing-on-sql-servers.md)|Suosittelee, että otat seurannan Azure SQL-palvelimet (Azure SQL-palvelua vain; ei ole käytössä oman näennäiskoneiden SQL).|
|[Tietokannan SQL tarkistaminen](security-center-enable-auditing-on-sql-databases.md)|Suosittelee, että otat seurannan Azure SQL-tietokannat (Azure SQL-palvelua vain; ei ole käytössä oman näennäiskoneiden SQL).|
|[Läpinäkyvä salauksen käyttöön SQL-tietokannat](security-center-enable-transparent-data-encryption.md)|Suosittelee, että otat salaus SQL-tietokannat (vain Azure SQL-palvelu).|

## <a name="see-also"></a>Katso myös

Saat lisätietoja suositukset, jotka koskevat muita Azure resurssityypit on seuraavissa artikkeleissa:

- [Oman näennäiskoneiden Azure Tietoturvakeskuksessa suojaaminen](security-center-virtual-machine-recommendations.md)
- [Sovellusten Azure Tietoturvakeskuksessa suojaaminen](security-center-application-recommendations.md)
- [Verkoston Azure Tietoturvakeskuksessa suojaaminen](security-center-network-recommendations.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
