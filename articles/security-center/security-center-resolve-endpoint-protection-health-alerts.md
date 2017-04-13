<properties
   pageTitle="Ratkaise päätepisteen suojauksen kunto ilmoitukset Azure Tietoturvakeskus | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten Azure Tietoturvakeskus suositus **ratkaista Endpoint Protection kunto ilmoitusten**täytäntöön."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Ratkaise päätepisteen suojauksen kunto ilmoitukset Azure Tietoturvakeskus

Azure Tietoturvakeskus suosittelee ratkaista olemassa päätepisteen suojauksen kunto ilmoitukset.  Tietoturvakeskus avulla näet, mitkä näennäiskoneiden (VMs) on endpoint protection virheet ja kuinka monta virheet.

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla. Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Ratkaise Endpoint Protection kunto ilmoitusten** **suositukset-sivu**.
![Ratkaise päätepisteen suojauksen kunto-ilmoitukset][1]

2. Tämä avaa sivu, jossa luetellaan VMs virheet ja virheiden määrä, kunkin AM **Endpoint Protection virhe** . Valitse luettelosta AM.
![Endpoint protection virhe][2]

3. **Virheiden luettelo** -sivu avautuu valitun AM, näyttäminen virheiden luettelo. Valitse luettelosta lisätietoja epäonnistui. Sivu avautuu valitun virheen tietoja.
![Virheiden luettelon][3]
  ![virheen tapahtuma][4]

## <a name="see-also"></a>Katso myös

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md)– Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Suojausta koskevia suosituksia Azure Tietoturvakeskuksessa hallinta](security-center-recommendations.md)– Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon seuranta Azure Tietoturvakeskus](security-center-monitoring.md)– Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md)--Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md)– Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/)– uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
