<properties
   pageTitle="Suojaaminen sovellustesi Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tämän asiakirjan osoitteet Azure Tietoturvakeskus suositukset, jotka auttavat suojaamaan Azure sovelluksia ja jatkat suojauskäytäntöjen mukaisesti."
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

# <a name="protecting-your-applications-in-azure-security-center"></a>Sovellusten Azure Tietoturvakeskuksessa suojaaminen

Azure Tietoturvakeskus analysoi Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, Luo suositukset, joka opastaa määrittäminen tarvittavat ohjausobjektit.  Suositukset koskevat Azure resurssityypit: näennäiskoneiden (VMs), verkko-SQL- ja sovellukset.

Tässä artikkelissa käsitellään suositukset, jotka liittyvät sovellukset.  Sovelluksen suosituksia center käyttöönoton web-sovelluksen palomuurin ympärille.  Käytä alla olevassa taulukossa viitteenä voi auttaa sinua ymmärtämään käytettävissä sovelluksen suositukset ja mitä kukin tehdä, jos haluat käyttää sitä.

## <a name="available-application-recommendations"></a>Käytettävissä oleva suositukset

|Suositus|Kuvaus|
|-----|-----|
|[Web-sovelluksen palomuurin lisääminen](security-center-add-web-application-firewall.md)|Suosittelee, että asennat web päätepisteet web application palomuuri-(WAF). Voit suojata useita web-sovellusten Tietoturvakeskuksessa lisäämällä aiemmin WAF käyttöönoton näiden sovellusten. WAF laitteiden (luodaan käyttämällä resurssien hallinnan käyttöönottomalli) on erillinen virtual verkon käyttöön. WAF laitteiden (luotu perinteinen käyttöönoton mallin) on rajoitettu verkon käyttöoikeusryhmän avulla. Tuki laajennetaan mukautetun käyttöönottoon WAF laitteen, (perinteinen) myöhemmin.|
|[Viimeistele sovellusten suojaus](security-center-add-web-application-firewall.md#finalize-application-protection)|Viimeistele määritys, WAF, liikenne on reititetään WAF laitteen-uudelleen. Tämä suositus seuraavan Suorita tarvittavat asetuksia.|

## <a name="see-also"></a>Katso myös

Saat lisätietoja suositukset, jotka koskevat muita Azure resurssityypit on seuraavissa artikkeleissa:

- [Oman näennäiskoneiden Azure Tietoturvakeskuksessa suojaaminen](security-center-virtual-machine-recommendations.md)
- [Verkoston Azure Tietoturvakeskuksessa suojaaminen](security-center-network-recommendations.md)
- [Azure SQL-palvelun Azure Tietoturvakeskuksessa suojaaminen](security-center-sql-service-recommendations.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
