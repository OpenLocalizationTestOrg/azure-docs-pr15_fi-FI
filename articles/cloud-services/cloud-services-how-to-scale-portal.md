<properties
    pageTitle="Automaattinen skaalaus pilvipalveluun portaalissa | Microsoft Azure"
    description="Opettele käyttämään portaalin määrittäminen automaattinen asteikko säännöt cloud web roolin tai työntekijän rooli Azure-tietokannassa."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Miten Automaattinen skaalaus pilvipalveluun

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-scale-portal.md)
- [Azure perinteinen portal](cloud-services-how-to-scale.md)

Ehtoja voi määrittää cloud-palvelun Työntekijä roolin, joka käynnistää asteikolla sisään tai ulos toiminto. Roolin ehdot voivat perustua suorittimen, levyn tai verkon kuormituksen roolin. Voit myös määrittää conditation, viestin jonossa tai joitakin muita Azure resurssi-tilaukseen liittyvää metrijärjestelmä.

>[AZURE.NOTE] Tässä artikkelissa keskitytään pilvipalvelussa Internetin kautta tai työntekijä roolit. Kun luot suoraan virtual machine (perinteinen), sitä isännöidään pilvipalvelussa. Voi skaalata vakio virtual machine [saatavuus](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) liittämällä tai poistaa ne manuaalisesti käyttöön tai poistaminen käytöstä.

## <a name="considerations"></a>Huomioon otettavia seikkoja

Ota huomioon seuraavat tiedot, ennen kuin määrität skaalaus sovelluksen:

- Skaalaus vaikuttaa core käyttö. Suurempi roolin esiintymät käyttämällä Lisää sydämiä. Tilauksen skaalata sovelluksen vain sydämiä rajoissa. Esimerkiksi jos tilauksesi kahtakymmentä Sydämiä ja voit käyttää sovellusta kaksi Normaali esitystapa enimmäismäärä on cloud services (neljä sydämiä yhteensä), voit vain skaalata määrittäminen muiden cloud palvelun ominaisuuksissa tilaukseesi 16 sydämiä mukaan. Saat lisätietoja koosta [Cloud palvelun koot](cloud-services-sizes-specs.md) .

- Voit skaalata jonon viestin raja-arvon perusteella. Lisätietoja olevien käyttämisestä on artikkelissa [jonon Tallennuspalvelu käyttämisestä](../storage/storage-dotnet-how-to-use-queues.md).

- Voit myös skaalata muut resurssit-tilaukseen liittyvää.

- Jotta sovelluksesi suuren käytettävyyden, varmista, että se otetaan käyttöön kahden tai useamman roolin esiintymät. Lisätietoja on artikkelissa [Palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).

## <a name="where-scale-is-located"></a>Skaalaa sijainti

Kun olet valinnut pilvipalvelussa sijaitsevaan, taulukossa pitäisi olla näkyvissä cloud service-sivu.

1. Valitse **roolit ja esiintymät** -ruutu cloud-palvelu-sivu cloud-palvelun nimi.   
**Tärkeää**: muista cloud-palvelun rooli ei alapuolella rooli roolin esiintymä.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Valitse **Skaalaa** -ruutu.

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automaattinen asteikko

Voit määrittää roolin mittakaava-asetuksia kahdessa eri **manuaalisesti** tai **automaattisesti**. Manuaalinen on tuttuun tapaan, voit määrittää esiintymät suora määrää. Automaattinen kuitenkin avulla voit määrittää sääntöjä, jotka määrittävät, kuinka ja miten paljon olet olisi skaalata.

**Aikataulun ja suorituskyvyn**sääntöjen **mukaan asteikko** asetus.

![Profiilin ja säännön cloud services mittakaava-asetuksia](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Aiemmin luotuun profiiliin.
2. Lisää ylemmän tason profiilin sääntö.
3. Lisää toiseen profiiliin.

Valitse **Lisää profiili**. Profiilin määrittää, missä haluat käyttää asteikon tilassa: **aina** **Toistuminen**, **kiinteän päivämäärän**.

Kun olet määrittänyt sääntöjä ja profiilin, valitsemalla yläreunassa **Tallenna** -kuvaketta.

#### <a name="profile"></a>Profiili

Profiilin määrittää vähimmäis- ja esiintymät mittakaava, kun asteikko-alue on aktiivinen.

* **Aina**

    Aina pitää tämän alueen ovat käytettävissä.  

    ![Pilvipalvelussa, aina skaalata](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Toistuvat tapahtumat**

    Jos haluat skaalata viikonpäivien valitseminen.

    ![Cloud service-asteikkoa, jossa toistuvan aikataulun](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Kiinteän päivämäärän**

    Jos haluat skaalata rooli kiinteän päivämääräalueen.

    ![CLoud service-asteikkoa, jossa kiinteän päivämäärän](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Kun olet määrittänyt profiili, valitse profiili-sivu alaosassa **OK** -painiketta.

#### <a name="rule"></a>Sääntö

Sääntöjen profiilin lisätään, ja ne kuvaavat ehto, jonka haluat käynnistävän asteikko. 

Säännön käynnistin perustuu mittarin, johon voit lisätä ehdollinen arvon pilvipalvelussa (suorittimen käyttö, levy tai verkon toiminnan). Lisäksi voit määrittää viestin jonossa tai joitakin muita Azure resurssi-tilaukseen liittyvää metrijärjestelmä käynnistin.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Kun olet määrittänyt säännön, valitse säännön sivu alaosassa **OK** -painiketta.

## <a name="back-to-manual-scale"></a>Takaisin manuaalinen asteikko

Siirry [Mittakaava-asetuksia](#where-scale-is-located) ja määritä **mukaan akseli** -vaihtoehto **esiintymä-määrä, jotka kirjoitan manuaalisesti**.

![Profiilin ja säännön cloud services mittakaava-asetuksia](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Tämä poistaa automaattisen skaalaus-roolin ja sitten voidaan määrittää esiintymän Laske suoraan. 

1. Akseli (manuaalinen tai automaattinen)-vaihtoehto.
2. Rooli esiintymä-liukusäädin määrittäminen Skaalaa esiintymät.
3. Rooli Skaalaa esiintymät.

Kun olet määrittänyt asetukset, valitse yläreunassa **Tallenna** -kuvaketta.

