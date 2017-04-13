<properties
   pageTitle="Verkoston Azure Tietoturvakeskuksessa suojaaminen | Microsoft Azure"
   description="Tämän asiakirjan osoitteet Azure Tietoturvakeskus suositukset, jotka auttavat suojaamaan Azure verkkoasi ja jatkat suojauskäytäntöjen mukaisesti."
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

# <a name="protecting-your-network-in-azure-security-center"></a>Verkoston Azure Tietoturvakeskuksessa suojaaminen

Azure Tietoturvakeskus analysoi Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, Luo suositukset, joka opastaa määrittäminen tarvittavat ohjausobjektit.  Suositukset koskevat Azure resurssityypit: näennäiskoneiden (VMs), verkko-SQL- ja sovellukset.

Tässä artikkelissa käsitellään suosituksia, jotka sopia omaan verkkoosi.  Verkon suosituksia center ympärille seuraavan sukupolven palomuurit, verkko-käyttöoikeusryhmiä tai määrittäminen saapuvan liikenteen säännöt.  Käytä alla olevassa taulukossa viitteenä voi auttaa sinua ymmärtämään verkko-suosituksia ja mitä kukin tehdä, jos haluat käyttää sitä.

## <a name="available-network-recommendations"></a>Käytettävissä olevat verkon koskevia suosituksia

|Suositus|Kuvaus|
|-----|-----|
|[Lisää seuraava luonti palomuuri](security-center-add-next-generation-firewall.md)|Suosittelee, että lisäät seuraava luonti-palomuurin (NGFW) Microsoft-kumppanilta, jotta voit parantaa suojausta suojaukset.|
|[Reitin liikenne NGFW vain](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Suosittelee, että määrität verkon suojauksen ryhmän (NSG) säännöt, jotka saapuvan liikenteen pakottaminen oman AM oman NGFW kautta.|
|[Verkon käyttöoikeusryhmät ottaminen käyttöön aliverkosta ja näennäiskoneiden](security-center-enable-network-security-groups.md)|Suosittelee käyttöön NSGs aliverkosta tai VMs.|
|[Vastakkaisten päätepisteen Internet käyttöoikeuksien rajoittaminen](security-center-restrict-access-through-internet-facing-endpoints.md)|Suosittelee, että määrität NSGs saapuvan liikenteen säännöt.|

## <a name="see-also"></a>Katso myös

Saat lisätietoja suositukset, jotka koskevat muita Azure resurssityypit on seuraavissa artikkeleissa:

- [Oman näennäiskoneiden Azure Tietoturvakeskuksessa suojaaminen](security-center-virtual-machine-recommendations.md)
- [Sovellusten Azure Tietoturvakeskuksessa suojaaminen](security-center-application-recommendations.md)
- [Azure SQL-palvelun Azure Tietoturvakeskuksessa suojaaminen](security-center-sql-service-recommendations.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
