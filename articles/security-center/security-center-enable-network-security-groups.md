<properties
   pageTitle="Ota käyttöön verkon käyttöoikeusryhmät Azure Tietoturvakeskuksessa | Microsoft Azure"
   description="Tässä asiakirjassa kerrotaan, miten Azure Tietoturvakeskus suositus **Käyttöön verkon käyttöoikeusryhmät**täytäntöön."
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

# <a name="enable-network-security-groups-in-azure-security-center"></a>Verkon käyttöoikeusryhmät Azure Tietoturvakeskuksessa ottaminen käyttöön

Azure Tietoturvakeskus suosittelee verkon käyttöoikeusryhmän (NSG) ottaminen käyttöön, jos jokin ei ole jo käytössä. NSGs sisältää luettelon Käyttöoikeusluettelon (Access Control) säännöt, jotka Salli tai estä verkkoliikennettä AM-esiintymissä Virtual verkossa. NSGs voi olla aliverkosta tai yksittäiset AM esiintymät, aliverkon sisällä. Jos aliverkon liitetään NSG, kaikki kyseisen aliverkon AM esiintymät koskevat Käyttöoikeusluettelon säännöt. Lisäksi yksittäisiä AM liikenne voidaan rajoittaa edelleen liittämällä NSG suoraan, jos haluat, että AM. Saat lisätietoja on artikkelissa [verkon suojauksen ryhmän (NSG) ominaisuudet?](../virtual-network/virtual-networks-nsg.md)

Jos sinulla ei ole käytössä NSGs, Tietoturvakeskus esittää kaksi suosituksia sinulle: Ota käyttöön verkon käyttöoikeusryhmät-valintaruudun aliverkosta ja ota käyttöön näennäiskoneiden verkon suojauksen ryhmien. Voit valita taso, aliverkon tai AM, jos haluat käyttää NSGs.


> [AZURE.NOTE] Tämän asiakirjan esitellään palvelun Esimerkki käyttöönoton avulla.  Tämä ei ole vaiheittaiset ohjeet.

## <a name="implement-the-recommendation"></a>Suositus täytäntöön

1. Valitse **Käyttöön verkon käyttöoikeusryhmät** aliverkosta tai näennäiskoneiden **suositukset** -sivu.
![Ota käyttöön verkon käyttöoikeusryhmät][1]

2. **Määritä puuttuu verkon käyttöoikeusryhmät** aliverkosta tai näennäiskoneiden sen mukaan, jonka valitsit suositus-sivu avautuu. Valitse aliverkon tai virtual kone NSG määrittämistä varten.

  ![Määritä NSG aliverkon][2]

  ![AM NSG määrittäminen][3]
3. Valitse **Valitse verkko-käyttöoikeusryhmän** sivu aiemmin NSG tai Luo uusi NSG valitsemalla.

  ![Valitse verkko-käyttöoikeusryhmän][4]

Jos luot uuden NSG, [hallinnasta NSGs Azure-portaalissa](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) NSG ja määritä suojaussäännöt ohjeiden mukaisesti.

## <a name="see-also"></a>Katso myös

Tässä artikkelissa ilmeni Tietoturvakeskus suositus "Käyttöön verkon käyttöoikeusryhmät" aliverkosta tai näennäiskoneiden ottamisesta käyttöön. Saat lisätietoja ottaminen käyttöön NSGs on seuraavissa artikkeleissa:

- [Mikä verkon suojauksen ryhmän (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Voit hallita NSGs Azure-portaalissa](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Saat lisätietoja Tietoturvakeskuksessa on seuraavissa artikkeleissa:

- [Azure Tietoturvakeskuksessa suojauskäytäntöjen määrittäminen](security-center-policies.md) – Opi määrittämään suojauskäytäntöjä Azure tilaukset ja resurssiryhmät.
- [Hallinta suojausta koskevia suosituksia Azure Tietoturvakeskuksessa](security-center-recommendations.md) – Katso, miten suositukset auttaa suojaamaan Azure resurssit.
- [Suojauksen kunnon valvonta Azure Tietoturvakeskuksessa](security-center-monitoring.md) – Opi Azure resurssien kunnon valvonta.
- [Hallinta ja niihin vastaaminen suojausvaroitusten Azure Tietoturvakeskuksessa](security-center-managing-and-responding-alerts.md) --Lue, miten voit hallita ja vastata suojausvaroitusten.
- [Seuranta-kumppaniratkaisuihin ja Azure Tietoturvakeskus](security-center-partner-solutions.md) – Katso, miten voit valvoa kumppaniratkaisuihin kunto-tilaa.
- [Azure Security Center usein kysytyt kysymykset](security-center-faq.md) – Etsi usein kysytyt kysymykset-palvelun avulla.
- [Azure-suojauksen blogi](http://blogs.msdn.com/b/azuresecurity/) – uusimmista Azure suojaus ja tiedot.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
