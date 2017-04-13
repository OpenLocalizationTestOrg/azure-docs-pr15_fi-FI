<properties
    pageTitle="Tilauksen ja tilien ohjeet | Microsoft Azure"
    description="Lisätietoja keskeisiä suunnittelu ja käyttöönotto ohjeita tilaukset ja Azure-tilit."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="subscription-and-accounts-guidelines"></a>Tilaus ja -tilien ohjeet

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa keskitytään tietoja voit esitellä tilaus ja käyttäjätilien hallinta kuin ympäristön ja käyttäjän perus kasvaa.


## <a name="implementation-guidelines-for-subscriptions-and-accounts"></a>Toteutuksen jälkeisen ohjeet tilaukset ja -tilien

Päätökset:

- Mitä tilaukset ja tee sitten tilit, sinun on käytettävä IT työmäärää tai infrastruktuuri joukko?
- Miten vastaamaan organisaation hierarkian eritellä?

Tehtävät:

- Määrittää looginen organisaatiohierarkia, kun haluat hallita tilauksen tason.
- Määritä vastaamaan looginen hierarkia pakollinen tilit ja valitse jokaisen tilin tilaukset.
- Luo joukko tilaukset ja tilille nimeämiskäytännön mukaisesti.


## <a name="subscriptions-and-accounts"></a>Tilaukset- ja -tilit

Käsitellä Azure, tarvitset vähintään yksi Azure tilaukset. Resurssit, kuten näennäiskoneiden (VMs) tai näiden tilauksista ole virtual verkot.

- Yritysasiakkaille on yleensä yrityksen rekisteröinti, joka on ylimmän resurssin hierarkiassa ja on yhdistetty vähintään yksi tileihin.
- Kuluttajille ja asiakkaiden ilman yrityksen rekisteröinti ylimmän resurssi on tili.
- Tilaukset liittyvät asiakkaat ja voi olla vähintään yksi tilaukset-tiliä kohti. Azure tietueet laskutustiedot tilauksen tasolla.

Vuoksi kaksi hierarkiaa tasoa tilin/tilauksen suhteen enimmäismäärä on tärkeää Tasaa nimeämiskäytännön asiakkaat ja tilaukset laskutuksen tarpeisiin. Esimerkiksi jos yleinen yritys Azure, he haluavat on yksi tili alueittain ja on tilaukset hallittuihin osoitteessa alue:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub01.png)

Voit esimerkiksi käyttää seuraavaa rakennetta:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub02.png)

Jos alue on enemmän kuin yksi tilaus olevaan ryhmään liittyvät päättää, nimeämiskäytännön pitäisi sisällyttää voi koodata lisätiedot tili tai tilauksen nimi. Organisaation sallii massaging laskutuksen tiedot Luo uusi hierarkia tasoja aikana laskutuksen raportteja:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub03.png)

Organisaatio voi näyttää seuraavalta:

![](./media/virtual-machines-common-infrastructure-service-guidelines/sub04.png)

Annamme enterprise Agreement-sopimus yksityiskohtainen Laskutus ladattavan tiedoston tilistä tai kaikkien tilien kautta.


## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 