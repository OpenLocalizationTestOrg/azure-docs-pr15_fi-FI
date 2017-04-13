<properties
   pageTitle="Järjestelmän päivitysten Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan ottamisesta käyttöön Azure Tietoturvakeskus suositukset **järjestelmän päivitykset** ja **sen jälkeen Järjestelmäpäivitykset uudelleen**."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Järjestelmän päivitysten Azure Tietoturvakeskus

Azure Tietoturvakeskus valvoo päivittäin Windowsin ja Linux näennäiskoneiden (VMs) puuttuvat käyttöjärjestelmän päivitykset. Tietoturvakeskus hakee luettelo käytettävissä suojaus ja kriittiset päivitykset Windows Update- tai Windows Server Update Services (WSUS), sen mukaan, joka on määritetty palvelun Windows AM.  Tietoturvakeskus tarkistaa myös Linux järjestelmien uusimmat päivitykset. Jos yhteyttä AM puuttuu järjestelmässä päivitys, Tietoturvakeskus suositeltavaa ottaa Järjestelmäpäivitykset

> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Käytä Järjestelmäpäivitykset** **suositukset** -sivu.
![Järjestelmän päivitykset][1]

2. **Käytä Järjestelmäpäivitykset** -sivu avautuu ja siinä näkyvät VMs puuttuvat Järjestelmäpäivitykset luettelo. Valitse AM.
![Valitse AM][2]

3. Sivu avautuu näyttäminen puuttuvat päivitykset luettelo, AM. Valitse järjestelmäpäivitys. Tässä esimerkissä Valitse KB3156016.
![Puuttuvat päivitykset][3]

4. Noudata puuttuu päivityksen **Päivitys** -sivu.
![Päivitys][4]

## <a name="reboot-after-system-updates"></a>Uudelleenkäynnistyksen jälkeen Järjestelmäpäivitykset

5. Palaa **suositukset** -sivu. Uuden tapahtuman luotiin, kun olet lisännyt Järjestelmäpäivitykset, kutsutaan **uudelleenkäynnistyksen jälkeen Järjestelmäpäivitykset**. Tämän vaihtoehdon voit, että sinun on käynnistettävä uudelleen, viimeistele järjestelmän päivitysten AM.
![Uudelleenkäynnistyksen jälkeen Järjestelmäpäivitykset][5]

6. Valitse **sen jälkeen Järjestelmäpäivitykset uudelleen**. Näyttäminen luettelona, sinun on käynnistettävä uudelleen Käytä järjestelmän päivitykset Viimeistele VMs **uudelleenkäynnistys odottaa suorittamiseen järjestelmäpäivityksiä** -sivu avautuu.
![Uudelleenkäynnistys odottaa][6]

Käynnistä AM azuren loppuun.

## <a name="see-also"></a>Katso myös

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – Etsi blogi kirjaa tietoja Azure tietosuojaan ja vaatimustenmukaisuuteen.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
