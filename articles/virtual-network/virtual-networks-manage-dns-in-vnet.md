<properties 
   pageTitle="Hallitse DNS-palvelimet käyttämä virtual verkon (VNet)"
   description="Opettele lisäämään ja poistamaan DNS-palvelimet virtual verkon (vnet)"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Hallitse DNS-palvelimet käyttämä virtual verkon (VNet)

Voit hallita DNS-palvelimet käytetään VNet hallinta-portaalin tai verkko-kokoonpanotiedosto luettelo. Voit lisätä enintään 12 DNS-palvelimet kunkin VNet. Määritettäessä DNS-palvelimet on tärkeää varmistaa luettelon DNS-palvelimet-ympäristössä oikeassa järjestyksessä. DNS-palvelin luettelot eivät toimi pyöreän pöydän. Niitä käytetään, että ne määritetyssä järjestyksessä. Jos luettelon ensimmäinen DNS-palvelimen onnistuu saavutetaan, asiakas käyttää kyseistä DNS-palvelimeen, riippumatta siitä, onko DNS-palvelin toimii oikein vai ei. Virtuaalinen verkon DNS server-järjestyksen muuttaminen DNS-palvelimet poistaminen luettelosta ja lisää ne järjestyksessä, jossa haluat takaisin sisään.

>[AZURE.WARNING] Kun DNS-luettelosta on päivitetty, virtual verkossa sijaitsevat niin, että ne Nosta uusi DNS-palvelinasetukset näennäiskoneiden on käynnistettävä uudelleen. Näennäiskoneiden säilyvät Käytä niiden nykyiset määritykset, kunnes ne käynnistetään uudelleen.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Muokkaa DNS-palvelinten luettelo virtual verkon hallinta-portaalissa

1. Kirjaudu **hallinta-portaalin**.

1. Siirtyminen-ruudussa **verkot**ja valitse sitten virtual verkon **nimi** -sarakkeen nimi.

1. Valitse **Määritä**.

1. **DNS-palvelimet**voit määrittää seuraavasti:

    - **Voit rekisteröidä (Lisää) uusi DNS-palvelin –** Kirjoittaa nimi ja IP-osoite-ruutuihin. Tämä lisää DNS-palvelimen virtual verkon DNS-palvelimet-luettelosta ja rekisteröi myös Azure DNS-palvelimeen.

    - **Lisää DNS-palvelin, joka oli aiemmin rekisteröidyn –** Jos olet jo rekisteröity DNS server Azure, voit valita valmiiksi luettelosta.

    - **Jos haluat poistaa virtual verkosta – DNS-palvelin** Napsauta poistettavaa palvelimen nimen vieressä X. Huomaa, että tämä poistaa vain palvelin VPN-luetteloa. DNS-palvelin pysyy Azure oman virtual verkkojen käyttämään rekisteröity. Voit poistaa tilauksen DNS-palvelimeen, siirry **verkot -> DNS-palvelimet** -sivulle.

    - **Jos haluat muuttaa järjestystä DNS-palvelimet –** Poista kaikki DNS-palvelimet, jotka on lueteltu ja lisää ne sitten takaisin sisään siinä järjestyksessä, jossa haluat. Muista, että tämä ei ole pyöreän pöydän DNS-luettelosta.

    - **DNS-palvelin – nimeäminen uudelleen** Korosta luettelo DNS-palvelimeen, ja kirjoita uusi nimi. Tämä uusi DNS-palvelin rekisteröidään Azure sekä lisääminen virtual verkon DNS-palvelimet-luettelosta. Vanha DNS-palvelin ja sen IP-osoite säilyy rekisteröity Azure kanssa. Voit poistaa sen **DNS-palvelimet** -sivulla, jos et käytä sitä virtual verkkojen.

1. Valitse **Tallenna** sivun alalaidassa Tallenna kokoonpanosi uusi DNS-palvelimet.

1. Käynnistä virtual verkon he voivat hankkia uuden DNS-asetukset sijaitsevat näennäiskoneiden.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>DNS-palvelinten luettelo, verkon määritysten tiedoston muokkaaminen

Jos haluat muokata DNS-palvelinten luettelo verkon määritys-tiedoston avulla, sinun Vie määritysasetukset hallinta-portaalista. Valitse Muokkaa verkko-kokoonpanotiedosto ja tuoda sen taaksepäin hallinta-portaalin. Alla on korkean tason luettelo noudattamalla ohjeita.

1. Vie virtual verkkoasetukset verkon määritystiedostoa. Lisätietoja ja vienyt määritysten verkkoasetukset Katso [Verkon määritysten tiedostoon Vie Virtual verkko-asetukset](virtual-networks-using-network-configuration-file.md).

1. Määritä virtual verkon DNS-palvelimen tiedot. Saat lisätietoja DNS-palvelin, joka määrittää [määrittäminen Virtual verkon määritystiedostossa DNS-palvelimeen](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Saat lisätietoja verkkotiedostojen [Azure Virtual verkon määritysten rakenteen](https://msdn.microsoft.com/library/azure/jj157100.aspx) ja [Määritä Virtual verkko-käyttö verkon määritystiedostoa](virtual-networks-using-network-configuration-file.md).

1. Tuo verkko-kokoonpanotiedosto. Saat lisätietoja ja ohjeita verkon määritys-tiedoston tuominen, [verkon määritysten tiedoston tuominen](virtual-networks-using-network-configuration-file.md).

1. Käynnistä virtual verkon he voivat hankkia uuden DNS-asetukset sijaitsevat näennäiskoneiden.
