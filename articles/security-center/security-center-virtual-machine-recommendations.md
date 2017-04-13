<properties
   pageTitle="Oman näennäiskoneiden Azure Tietoturvakeskuksessa suojaaminen | Microsoft Azure"
   description="Tämän asiakirjan osoitteet Azure Tietoturvakeskus suositukset, jotka auttavat suojaamaan oman näennäiskoneiden ja jatkat suojauskäytäntöjen mukaisesti."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Oman näennäiskoneiden Azure Tietoturvakeskuksessa suojaaminen

Azure Tietoturvakeskus analysoi Azure resurssien suojaus-tilaan. Kun Tietoturvakeskus havaitsee mahdollisen tietoturva-aukkoja, Luo suositukset, joka opastaa määrittäminen tarvittavat ohjausobjektit.  Suositukset koskevat Azure resurssityypit: näennäiskoneiden (VMs), verkko-SQL- ja sovellukset.

Tässä artikkelissa käsitellään suositukset, jotka koskevat VMs.  AM suosituksia center ympärille tietojen kerääminen, järjestelmä päivitysten, valmistelu antimalware, salaaminen AM-levyille ja paljon muuta.  Käytä alla olevassa taulukossa viitteenä voi auttaa sinua ymmärtämään käytettävissä AM suositukset ja mitä kukin tehdä, jos haluat käyttää sitä.

## <a name="available-vm-recommendations"></a>Käytettävissä olevat AM suositukset

|Suositus|Kuvaus|
|-----|-----|
|[Tietojen keräämisen tilaukset ottaminen käyttöön](security-center-enable-data-collection.md)|Suosittelee, että otat tietojen keräämisen suojauskäytäntö kaikista tilauksistasi ja kaikki näennäiskoneiden (VMs)-tilausten.|
|[Riskien OS heikkouksien parantaminen](security-center-remediate-os-vulnerabilities.md)|Tasaa OS-määrityksiä ja suositellut määritykset-sääntöjä suosittelee esimerkiksi salli salasanojen tallentamista.|
|[Järjestelmän päivitykset](security-center-apply-system-updates.md)|Suosittelee, että otat puuttuu tietoturvan ja kriittiset päivitykset käyttöön VMs.|
|[Uudelleenkäynnistyksen jälkeen Järjestelmäpäivitykset](security-center-apply-system-updates.md#reboot-after-system-updates)|Suosittelee, että käynnistät Viimeistele järjestelmän päivitysten AM.|
|[Endpoint Protection asentaminen](security-center-install-endpoint-protection.md)|Suosittelee valmistella antimalware ohjelmien VMs (vain Windows VMs).|
|[Ratkaise Endpoint Protection kunto-ilmoitukset](security-center-resolve-endpoint-protection-health-alerts.md)|Suosittelee ratkaista endpoint protection virheet.|
|[Ota käyttöön AM agentti](security-center-enable-vm-agent.md)|Voit nähdä VMs vaativat AM-agentti. AM-agentti on oltava asennettuna VMs jotta valmistella korjaustiedoston scanning perusaikataulun scanning ja antimalware ohjelmat. AM-agentti asennetaan oletusarvoisesti VMs, jotka on otettu Azure Marketplacesta. Artikkelissa [AM agentti ja laajennukset – osa 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) on tietoja siitä, miten voit asentaa AM-agentti.|
| [Käytä salauksen](security-center-apply-disk-encryption.md) |Suosittelee salaa AM levyjen Azure salauksen (Windows ja Linux VMs) avulla. Salauksen suositellaan OS sekä tiedot että AM asemat.|
| [Päivitä käyttöjärjestelmäversio](security-center-update-os-version.md) | Suosittelee päivittäminen (OS)-käyttöjärjestelmäversio Cloud-palvelun käytettävissä viimeisimmän version OS perheen.  Saat lisätietoja pilvipalveluihin artikkelissa [Cloud Services-palvelujen yleiskatsaus](../cloud-services/cloud-services-choose-me.md). |
| [Haavoittuvuuden arviointi ei ole asennettu](security-center-vulnerability-assessment-recommendations.md) | Suosittelee haavoittuvuuden arviointi-ratkaisun asentaminen oman AM. |
| [Riskien heikkouksien parantaminen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Voit nähdä oman AM asennettuihin haavoittuvuuden arviointi-ratkaisun havaitsemien järjestelmän ja sovelluksen heikkouksien. |

## <a name="see-also"></a>Katso myös

Saat lisätietoja suositukset, jotka koskevat muita Azure resurssityypit on seuraavissa artikkeleissa:

- [Sovellusten Azure Tietoturvakeskuksessa suojaaminen](security-center-application-recommendations.md)
- [Verkoston Azure Tietoturvakeskuksessa suojaaminen](security-center-network-recommendations.md)
- [Azure SQL-palvelun Azure Tietoturvakeskuksessa suojaaminen](security-center-sql-service-recommendations.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
