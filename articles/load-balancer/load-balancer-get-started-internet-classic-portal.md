
<properties
   pageTitle="Selainpohjainen vastakkaisten kuormituksen Azure perinteinen portaalissa perinteinen käyttöönoton mallin luomisesta | Microsoft Azure"
   description="Selainpohjainen vastakkaisten kuormituksen Azure perinteinen portaalissa perinteinen käyttöönotto-mallin luominen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Selainpohjainen vastakkaisten kuormituksen (perinteinen) Azure perinteinen portaalissa luomisesta

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [luominen selainpohjainen vastakkaisten kuormituksen Azure resurssien hallinnan avulla](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Internetiin yhteydessä oleva-kuormituksen näennäiskoneiden määrittäminen

Haluat ladata saldo verkkoliikennettä Internetistä näennäiskoneiden cloud-palvelun kautta, sinun on luotava kuormituksen määrittäminen. Tässä toimintosarjassa oletetaan, että olet jo luonut näennäiskoneiden ja että ne ovat kaikki samassa pilvipalvelussa.

**Voit määrittää kuormituksen tiedostojoukon näennäiskoneiden**

1. Azure perinteinen portaalissa **näennäiskoneiden**ja valitse sitten virtual koneen kuormituksen joukon nimi.

2. Valitse **päätepisteet**ja valitse sitten **Lisää**.

3. Valitse **Lisää päätepisteen virtual machine** -sivulla olevaa oikealle osoittavaa nuolta.

4. **Määritä päätepisteen tiedot** -sivulla:

    * Kirjoita **nimi**-päätepisteen nimi tai valitse nimi yleisiä protokollia ennalta määritettyjä päätepisteet luettelo.
    * **Protokolla**Valitse päätepisteen TCP tai UDP-tyypin vaatii protokolla tarpeen mukaan.
    * Kirjoita **portti julkisen ja yksityisen portti**, porttinumerot, jonka haluat käyttää, virtuaalikoneen tarpeen mukaan. Voit käyttää yksityinen portti- ja palomuurisäännöt virtuaalikoneen ohjaaminen uudelleen niin, että sovelluksesi vastaava liikenne. Yksityinen portti voi olla sama kuin julkinen portti. Päätepisteen web (HTTP) tietoliikenteen voi esimerkiksi määrittää portti 80 julkisista ja yksityisistä-porttiin.

5. Valitse **Luo joukko kuormituksen**ja valitse sitten oikealle osoittavaa nuolta.

6. **Määritä kuormituksen määrittäminen** -sivulla kuormituksen joukon nimi ja osoita näytteenottimen toimintaa Azure ladata tasaustoiminto arvot. Lataa tasaustoiminto käyttää keräysputkien voit selvittää, ovatko kuormituksen määrittäminen näennäiskoneiden vastaanottaa saapuvan liikenteen.

7. Napsauta Luo kuormituksen päätepiste valintamerkkiä. Näkyviin tulee **Kyllä** virtuaalikoneen **päätepisteet** sivun **kuormituksen joukon nimi** -sarakkeessa.

8. Portaalissa **näennäiskoneiden**, muita virtual-tietokoneen kuormituksen joukon nimi, valitse **päätepisteet**ja valitse sitten **Lisää**.

9. **Lisää päätepisteen virtual machine** -sivulla **Lisää päätepiste aiemmin kuormituksen määritetyt**, kuormituksen joukon nimi ja napsauta oikealle osoittavaa nuolta.

10. Valitse **Määritä päätepisteen tiedot** -sivulla päätepisteen nimi ja valitse sitten valintamerkkiä.

Kuormituksen-ryhmässä Lisää näennäiskoneiden toista vaiheet 8 – 10.



## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)

