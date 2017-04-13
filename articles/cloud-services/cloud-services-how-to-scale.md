<properties
    pageTitle="Automaattinen skaalaus pilvipalveluun portaalissa | Microsoft Azure"
    description="(perinteinen) Opettele käyttämään perinteinen portaalin määrittäminen automaattinen asteikko säännöt cloud web roolin tai työntekijän rooli Azure-tietokannassa."
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

Azure perinteinen portaalin asteikko-sivulla voit manuaalisesti skaalata web rooli tai työntekijän rooli tai voit ottaa automaattisen skaalauksen suorittimen kuormituksen tai viestin jonossa.

>[AZURE.NOTE] Tässä artikkelissa keskitytään pilvipalvelussa Internetin kautta tai työntekijä roolit. Kun luot suoraan virtual machine (perinteinen), sitä isännöidään pilvipalvelussa. Jotkin näistä tiedoista koskee seuraavanlaisiin näennäiskoneiden. Skaalaus käytettävyys määrittää siihen näennäiskoneiden on todella vain suljetaan ne käyttöön ja poistaminen käytöstä määrität asteikko sääntöjen perusteella. Lisätietoja näennäiskoneiden ja käytettävyyden joukot-kohdassa [Manage näennäiskoneiden käytettävyys](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Ota huomioon seuraavat tiedot, ennen kuin määrität skaalaus sovelluksen:

- Skaalaus vaikuttaa core käyttö. Suurempi roolin esiintymät käyttämällä Lisää sydämiä. Tilauksen skaalata sovelluksen vain sydämiä rajoissa. Esimerkiksi jos tilauksesi kahtakymmentä Sydämiä ja voit käyttää sovellusta kaksi Normaali esitystapa enimmäismäärä on cloud services (neljä sydämiä yhteensä), voit vain skaalata määrittäminen muiden cloud palvelun ominaisuuksissa tilaukseesi 16 sydämiä mukaan. Saat lisätietoja koosta [Cloud palvelun koot](cloud-services-sizes-specs.md) .

- Luo jonon ja liittää sen roolissa, ennen kuin voit skaalata sovelluksen viestin raja-arvon perusteella. Lisätietoja on artikkelissa [jonon Tallennuspalvelu käyttämisestä](../storage/storage-dotnet-how-to-use-queues.md).

- Voit skaalata resurssit, jotka on linkitetty pilvipalvelussa sijaitsevaan. Saat lisätietoja linkittämällä resurssien [Toimintaohje: linkittää resurssin pilvipalveluun](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Jotta sovelluksesi suuren käytettävyyden, varmista, että se otetaan käyttöön kahden tai useamman roolin esiintymät. Lisätietoja on artikkelissa [Palvelun tason sopimuksia](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Aikataulun skaalaus

Oletusarvon mukaan kaikki roolit eivät seuraa tietyn aikataulun. Sen vuoksi muuttaa asetuksia sovelletaan aina ja kaikki päivät koko vuoden. Jos haluat, voit määrittää manuaalisesti tai automaattisesti skaalausta:

- Viikonpäivät
- Viikonloput
- Viikon Hotelliöiden
- Viikon mornings
- Tiettyjen päivämäärien
- Tietyn päivämäärävälin

Tämä on conigured [Azure perinteinen portal](https://manage.windowsazure.com/)  
**Cloud Services** > **\[pilvipalvelussa sijaitsevaan\]** > **asteikko** > **\[tuotannon tai väliaikaisen\] ** sivulle.

Napsauta jokaista roolia haluat muuttaa **ajoittaa määrittäminen** -painiketta.

![Cloud palvelun Automaattinen skaalaus aikataulun mukaan][scale_schedules]



## <a name="manual-scale"></a>Manuaalinen asteikko

Valitse **asteikko** -sivulla voit manuaalisesti suurentaa tai pienentää käynnissä pilvipalvelussa esiintymien määrän. Tämä on määritetty kunkin aikataulun olet luonut tai kaikki aika, jos et ole luonut aikataulun.

1. [Azure perinteinen portal](https://manage.windowsazure.com/) **Pilvipalveluihin**ja valitse sitten Avaa koontinäyttö pilvipalvelussa nimi.

    > [AZURE.TIP] Jos et näe pilvipalvelussa sijaitsevaan, joudut ehkä muuttamaan **tuotannon** **väliaikaisen** tai päinvastoin.

2. **Valitse.**

3. Valitse muutettava skaalausasetukset, aikataulu. Oletusarvo on *ei tietyin väliajoin* Jos sinulla ei ole määritetty aikatauluja.

4. Etsi **asteikko metrijärjestelmä mukaan** -osassa ja valitse **ei mitään**. Tämä on oletusarvo rooleista.

5. Kullakin pilvipalvelussa roolilla on muuttamiseen käytettävä kerrat liukusäädintä.

    ![Skaalaa manuaalisesti cloud-palvelun rooli][manual_scale]

    Jos tarvitset esiintymiä, joudut ehkä [cloud palvelun virtuaalikoneen koon](cloud-services-sizes-specs.md)muuttaminen.

6. Valitse **Tallenna**.  
Rooli tapauksissa voi lisätä tai poistaa valintasi mukaan.

>[AZURE.TIP] Aina, kun näet ![][tip_icon] Vie hiiren osoitin ja saat lisätietoja tiettyä asetuksen toiminnoista.


## <a name="automatic-scale---cpu"></a>Automaattinen skaalaus - Suoritin

Tämä Skaalaa Jos suorittimen käyttö prosentteina ylä- tai alapuolelle määritetyn raja-arvoja. rooli esiintymät luodaan tai poistetaan.

1. [Azure perinteinen portal](https://manage.windowsazure.com/) **Pilvipalveluihin**ja valitse sitten Avaa koontinäyttö pilvipalvelussa nimi.

    > [AZURE.TIP] Jos et näe pilvipalvelussa sijaitsevaan, joudut ehkä muuttamaan **tuotannon** **väliaikaisen** tai päinvastoin.

2. **Valitse.**

3. Valitse muutettava skaalausasetukset, aikataulu. Oletusarvo on *ei tietyin väliajoin* Jos sinulla ei ole määritetty aikatauluja.

4. Etsi **asteikko metrijärjestelmä mukaan** -osassa ja valitse **Suoritin**.

5. Voit nyt määrittää vähimmäis- ja tietoalueen roolit esiintymät, kohde suorittimen käyttö (käynnistämään määrittäminen asteikolla) ja kuinka monta esiintymää skaalata ylös tai alas.

![Cloud-palvelun rooli skaalata suorittimen kuormituksen][cpu_scale]

>[AZURE.TIP] Aina, kun näet ![][tip_icon] Vie hiiren osoitin ja saat lisätietoja tiettyä asetuksen toiminnoista.





## <a name="automatic-scale---queue"></a>Automaattinen skaalaus - jonossa

Tämä Skaalaa automaattisesti, jos jonossa viestien määrä ylä- tai alapuolelle määritetty raja; rooli esiintymät luodaan tai poistetaan.

1. [Azure perinteinen portal](https://manage.windowsazure.com/) **Pilvipalveluihin**ja valitse sitten Avaa koontinäyttö pilvipalvelussa nimi.

    > [AZURE.TIP] Jos et näe pilvipalvelussa sijaitsevaan, joudut ehkä muuttamaan **tuotannon** **väliaikaisen** tai päinvastoin.

2. **Valitse.**

3. Etsi **asteikko metrijärjestelmä mukaan** -osassa ja valitse **Suoritin**.

4. Nyt voit määrittää vähimmäis- ja alueen roolit esiintymien, jonon ja sanomien käsittelemään jokaiselle esiintymälle ja kuinka monta esiintymää skaalata ylös ja alas määrä.

![Cloud-palvelun rooli skaalata viestien jono][queue_scale]

>[AZURE.TIP] Aina, kun näet ![][tip_icon] Vie hiiren osoitin ja saat lisätietoja tiettyä asetuksen toiminnoista.


## <a name="scale-linked-resources"></a>Skaalaa linkitetyn resurssit

Usein, kun rooli skaalata, on hyödyllisiin skaalata sovellus käyttää myös haluamasi tietokanta. Jos linkität tietokanta pilvipalveluun, voit käyttää kyseiselle resurssille Skaalausasetusten napsauttamalla linkkiä.

1. [Azure perinteinen portal](https://manage.windowsazure.com/) **Pilvipalveluihin**ja valitse sitten Avaa koontinäyttö pilvipalvelussa nimi.

    > [AZURE.TIP] Jos et näe pilvipalvelussa sijaitsevaan, joudut ehkä muuttamaan **tuotannon** **väliaikaisen** tai päinvastoin.

2. **Valitse.**

3. Etsi **linkitetyn resurssit** -osa ja napsauttanut **tietokannalle**asteikolta hallinta.

    > [AZURE.NOTE] Jos **linkitetty resurssit** -osa ei ole näkyvissä, sinun ei todennäköisesti ole linkitetty resursseja.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
