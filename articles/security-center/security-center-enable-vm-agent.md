<properties
   pageTitle="Ota käyttöön AM agentti Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten Azure Tietoturvakeskus suositus **Käyttöön AM agentti**täytäntöön."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>AM agentti Azure Tietoturvakeskuksessa ottaminen käyttöön

AM-agentti on asennettuna näennäiskoneiden (VMs) käyttöön [tietojen kerääminen](security-center-enable-data-collection.md)järjestyksessä.  Azure Tietoturvakeskus avulla voit tarkastella, mitkä VMs edellyttävät AM-agentti ja suosittelee, että otat ne VMs AM-agentti.

AM-agentti asennetaan oletusarvoisesti VMs, jotka on otettu Azure Marketplacesta. Artikkelissa [AM agentti ja laajennukset – osa 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) on tietoja siitä, miten voit asentaa AM-agentti.


> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla. Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Käyttöön AM agentti** **suositukset-sivu**.
![Ota käyttöön AM agentti][1]

2. **AM agentti puuttuu tai ei vastaa**sivu avautuu. Tämä sivu näyttää VMs, jotka edellyttävät AM-agentti. Asenna AM-agentti sivu ohjeiden mukaisesti.
![AM agentti puuttuu][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
