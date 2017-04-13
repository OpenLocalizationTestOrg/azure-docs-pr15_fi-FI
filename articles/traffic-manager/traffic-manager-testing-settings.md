<properties
    pageTitle="Testauksessa liikenteen hallinta-asetukset | Microsoft Azure"
    description="Tämän artikkelin avulla voit testata liikenteen hallinta-asetukset"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="test-your-traffic-manager-settings"></a>Testaa liikenteen hallinta

Testaa liikenteen hallinta, sinun on useita asiakkaita on eri sijainneissa, josta voit suorittaa testejä. Tuo sitten päätepisteet liikenteen hallinta-profiilissasi alaspäin yksi kerrallaan.

* Määritä DNS-TTL-arvoa pieni, niin, että muutokset välittäminen nopeasti (esimerkiksi 30 sekuntia).
* Tiedät Azure pilvipalveluiden ja käytät profiilin sivustot IP-osoitteet.
* Käytä Työkalut, joiden avulla voit selvittää DNS-nimen IP-osoitteeseen ja näyttää osoite.

Tarkistat näet, että DNS-nimien ratkaista profiilin päätepisteet IP-osoitteisiin. Yhdenmukaisesti liikenne reititys-menetelmällä liikenteen hallinta profiilissa määritetyn korjaa nimet. Työkaluja, kuten **nslookup** tai **tarkastella** avulla voit selvittää DNS-nimiä.

Seuraavien esimerkkien avulla voit testata liikenteen hallinta profiilin.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Tarkista liikenteen hallinta profiilin Windowsin nslookup ja ipconfig käyttäminen

1. Avaa komennon tai Windows PowerShell-kehote järjestelmänvalvojana.
2. Kirjoita `ipconfig /flushdns` , tyhjennä DNS-tulkintatoiminnon välimuisti.
3. Kirjoita `nslookup <your Traffic Manager domain name>`. Esimerkiksi seuraava komento tarkistaa etuliite *myapp.contoso* toimialuenimi

        nslookup myapp.contoso.trafficmanager.net

    Tyypillinen tulos näyttää seuraavat tiedot:

    * DNS-nimen ja voit ratkaista tämän liikenteen hallinta toimialuenimeä käytetään DNS-palvelimen IP-osoite.
    * Liikenteen hallinta toimialuenimen kirjoitit komentorivillä "nslookup- ja IP-osoite, johon liikenteen hallinta toimialueen korjaa jälkeen. Toinen IP-osoite on tärkeää tarkistaa. Se on vastattava julkisen virtual IP (VIP)-osoitteen johonkin pilvipalveluihin tai käytät liikenteen hallinta profiilin sivustot.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Miten automaattisesti liikenne reititys menetelmä

1. Jätä kaikki päätepisteitä ylös.
2. Käytä yhden asiakasohjelman, pyytää nslookup tai samanlaisia apuohjelman yrityksen toimialuenimen DNS-ratkaisun.
3. Varmista, että ratkaista IP-osoite ensisijainen päätepiste.
4. Siirrä alas ensisijainen päätepisteen tai poistaa seurantaa tiedoston siten, että liikenteen hallinta saattavat mukaan, että sovellus ei ole käytettävissä.
5. Odota, DNS--elinaika (TTL) liikenteen hallinta profiilin sekä muita kahdessa minuutissa. Jos DNS-TTL on 300 sekunnin (5 minuuttia), voit esimerkiksi on odotettava seitsemän minuuttia.
6. Tyhjentää käyttäjän DNS asiakkaan välimuistin ja pyydä DNS-ratkaisun avulla nslookup. Windowsin voit tyhjentää DNS-välimuisti ipconfig/flushdns-komennolla.
7. Varmista, että ratkaista IP-osoite toissijainen päätepiste.
8. Toista vaiheet, joka tuo alaspäin päätepisteiden puolestaan. Varmista, että DNS palauttaa seuraavan päätepisteen IP-osoite-luettelossa. Kun kaikki päätepisteet eivät ole käytettävissä, ensisijainen päätepisteen IP-osoite kannattaa hankkia uudelleen.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Painotetun liikenne reititys menetelmä testaaminen

1. Jätä kaikki päätepisteitä ylös.
2. Käytä yhden asiakasohjelman, pyytää nslookup tai samanlaisia apuohjelman yrityksen toimialuenimen DNS-ratkaisun.
3. Varmista, että ratkaista IP-osoite vastaa jonkin oman päätepisteet.
4. DNS-asiakasohjelman välimuistin tyhjentäminen ja toista vaiheet 2 ja 3 jokaisen päätepiste. Raportissa pitäisi näkyä eri IP-osoitteet, funktio palauttaa kunkin käyttäjän päätepisteet.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Suorituskyvyn liikenne reititys menetelmä testaaminen

Testaa tehokkaasti suorituskyvyn liikenne reititys menetelmän, sinulla on oltava asiakkaat, jotka sijaitsevat eri puolilla maailmaa. Voit luoda eri Azure alueet, jotka voidaan testata palvelujen asiakkaille. Jos sinulla on yleinen verkkoon, voit etäyhteyden asiakkaiden muiden osien maailman kirjautuminen ja testit sieltä.

Voit myös jakautuvat Vapauta verkkopohjaisia DNS-haku ja tarkastella käytössä. Jotkin näistä työkaluista avulla voit tarkistaa DNS-nimenselvitys eri puolilla maailmaa sijainneista. Tee haku-"DNS-haku-esimerkkejä. Kolmannen osapuolen palvelujen, kuten Gomez tai esityksen voidaan Vahvista profiileja ovat jakaminen liikenne odotetulla tavalla.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Tietoja liikenteen hallinta liikenne reititys menetelmät](traffic-manager-routing-methods.md)
* [Liikenteen hallinta suorituskykyyn liittyviä tietoja](traffic-manager-performance-considerations.md)
* [Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)




