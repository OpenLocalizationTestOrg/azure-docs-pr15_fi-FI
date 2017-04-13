<properties
   pageTitle="Luo Internetiin yhteydessä oleva-kuormituksen resurssien hallinta-portaalissa Azure | Microsoft Azure"
   description="Opettele luomaan Internetiin yhteydessä oleva-kuormituksen resurssien hallinta Azure-portaalissa"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Luominen Internetiin yhteydessä oleva kuormituksen Azure-portaalissa

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään resurssien hallinnan käyttöönottomalli. Voit myös [luominen Internetiin yhteydessä oleva kuormituksen käyttämällä perinteinen käyttöönotto](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Tämä koskee yksittäisiä tehtäviä se on tehtävä kuormituksen luominen ja kerrotaan yksityiskohtaisesti, mitä tehdään suoritettava tavoitteen järjestystä.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Mikä on luotava Internetiin yhteydessä oleva-kuormituksen?

Sinun täytyy luoda ja määrittää kuormituksen tasauspalvelun ottamaan seuraaville objekteille.

- IP-määritys edusta - sisältää julkisten IP-osoitteiden saapuvaa verkkoliikennettä.

- Taustatietokannan osoiteryhmä - sisältää verkkoliittymät (NIC) vastaanottaa verkkoliikennettä kuormituksen näennäiskoneiden.

- Kuormituksen tasaamisen säännöt - sisältää sääntöjä yhdistäminen julkisen porttiin kuormituksen taustatietokantaan osoite resurssivarantoon-porttiin.

- Saapuvan liikenteen säännöt NAT - sisältää yhdistäminen julkisen porttiin kuormituksen tiettyä virtual konetta taustatietokantaan osoite varannon portin säännöt.

- Probes - sisältää kunto keräysputkien käytettävä näennäiskoneiden esiintymien taustatietokantaan osoite varannon käytettävyyden tarkistaminen.

Saat lisätietoja kuormituksen tasauspalvelun osien Azure resurssien hallinnan osoitteessa [Azure Resurssienhallinta tuki ladata tasaustoiminto](load-balancer-arm.md).


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Määrittää kuormituksen Azure-portaalissa

> [AZURE.IMPORTANT] Tässä esimerkissä oletetaan, että kutsutaan **myVNet**virtual verkkoon. Lisätietoja on ohjeaiheessa toiminto [Luo VPN](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) . Se myös olettaa ei sisällä **myVNet** kutsutaan aliverkon **Aliverkon kg voidaan** ja kaksi VMs **web1** ja **web2** vastaavasti käytettävyys samat sisällä nimeltään **myAvailSet** **myVNet**. Lisätietoja VMs [tätä linkkiä](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .


1. Selaimesta Siirry Azure-portaaliin: [http://portal.azure.com](http://portal.azure.com) ja kirjaudu sisään Azure-tilisi kanssa.

2. Valitse näytön vasemmassa yläkulmassa **Uusi** > **Verkko** > **kuormituksen.**

3. Kirjoita **Luo kuormituksen** , sivu kuormituksen tasauspalvelun nimi. Tätä kutsutaan **myLoadBalancer**.

4. Valitse **tyyppi**-kohdassa **julkiset**.

5. **Julkiseen IP-osoite**Luo uusi julkinen IP kutsutaan **myPublicIP**.

6. Valitse resurssi-ryhmässä **myRG**. Valitse haluamasi **sijainti**ja valitse sitten **OK**. Kuormituksen alkaa sitten ottaminen käyttöön ja kestää muutaman minuutin loppuun käyttöönotto.

![Resurssiryhmä, kuormituksen päivittäminen](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Luo taustatietokantaan osoite resurssivarantoon

1. Kun kuormituksen tasauspalvelun on otettu, valitse sen resurssien kuluessa. Valitse asetukset-kohdasta Taustajärjestelmä jakavat. Kirjoita taustatietokannan ryhmän nimi. Valitse **Lisää** -painiketta, joka näkyy sivu yläosaan.

2. Valitse **Lisää virtual machine** - **Lisää Taustajärjestelmä resurssivarantoon** -sivu.  Valitse **käytettävyys Määritä** **käytettävyys määrittäminen** ja **myAvailSet**. Seuraavaksi Valitse näennäiskoneiden kohdassa sivu **valitsemalla näennäiskoneiden** ja valitse sitten **web1** ja **web2**, ohjelma luo kuormituksen tasaamisen kaksi VMs. Varmista, että molemmat on sininen valintamerkit vasemmalle alla olevassa kuvassa esitetyllä tavalla. Valitse **Valitse** -, sivu ja sen jälkeen **valita Virtual koneet** sivu OK ja sitten **OK** **Taustajärjestelmä resurssivarantoon lisääminen** -sivu.

    ![Taustajärjestelmä osoite - ryhmän lisääminen ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Varmista, että ilmoitukset avattava luettelo on päivitystä koskevat tallentaminen kuormituksen tasauspalvelun Taustajärjestelmä resurssivarantoon lisäksi VMs **web1** ja **web2**Verkkokäyttöliittymän päivitys.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Näytteenottimen, kg sääntö ja NAT sääntöjen luominen

1. Luo kunto näytteenottimen.

    Valitse kuormituksen tasauspalvelun asetukset-kohdassa keräysputkien. Valitse **Lisää** sivu yläosassa.

    Määrittäminen näytteenottimen kahdella tavalla: HTTP- tai TCP. Tässä esimerkissä näytetään HTTP, mutta TCP voi määrittää samalla tavalla.
    Päivitä tarvittavat tiedot. Kuten edellä, **myLoadBalancer** ladata porttiin 80 liikenne saldo. Valittu polku on HealthProbe.aspx väli on 15 sekunnin kuluttua ja perusasemassa raja-arvo on 2. Kun olet valmis, valitse **OK** , jos haluat luoda näytteenottimen.

    Vie osoitin "i" Lisätietoja yksittäisiä määritysten ja miten niitä voi muuttaa tarpeen mukauttaminen-kuvaketta.

    ![Näytteenottimen lisääminen](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Kuormituksen tasauspalvelun säännön luominen

    Valitse kuormituksen kuormituksen tasauspalvelun asetukset-kohdasta säännöt. Valitse **Lisää**uusi sivu. Säännön nimi. Tässä on HTTP. Valitse frontend portti- ja Taustajärjestelmä portti. Tässä kohdassa 80 valitaan molemmat. Valitse **kg Taustajärjestelmä** Taustajärjestelmä resurssivarantoon ja aiemmin luotuja **HealthProbe** keräysputken nimellä. Määrityksiä voidaan määrittää tarpeen mukaan. Valitse Tallenna sääntö kuormituksen OK.

    ![Kuormituksen säännön lisääminen](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Saapuvan NAT sääntöjen luominen

    Valitse NAT saapuvan liikenteen säännöt kuormituksen tasauspalvelun asetukset-kohdassa. Uusi sivu, valitse **Lisää**. Saapuvan NAT-säännön nimeksi. Tätä kutsutaan **inboundNATrule1**. Kohde on oltava aiemmin luotu julkisen IP. Valitse mukautettu palvelussa ja haluat käyttää protokolla. Tässä TCP on valittuna. Määritä portti 3441, ja kohde portti, tässä tapauksessa 3389. Valitse Tallenna sääntö.

    Kun ensimmäinen sääntö on luotu, toista tämä vaihe toisen saapuvan NAT säännön inboundNATrule2 kutsutut portin 3442 kohde portti 3389.

    ![Saapuvan liikenteen NAT-säännön lisääminen](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Poista kuormituksen tasaus

Jos haluat poistaa kuormituksen tasauspalvelun, valitse kuormituksen, jonka haluat poistaa. **Poista** sivu yläosassa osoittamalla *Kuormituksen* -sivu. Valitse **Kyllä** pyydettäessä.

## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-cli.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
