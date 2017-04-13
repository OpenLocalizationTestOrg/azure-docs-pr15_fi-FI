<properties
   pageTitle="Poista käytöstä, ottaminen käyttöön tai poistaa liikenteen hallinta profiilin | Microsoft Azure"
   description="Tämän artikkelin avulla voit käsitellä liikenteen hallinta-profiileista."
   services="traffic-manager"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="disable-enable-or-delete-a-profile"></a>Poista käytöstä, ottaminen käyttöön tai poistaa profiili


Voit poistaa aiemmin luodun liikenteen hallinta profiilin niin, että se ei viittaa pyynnöt sen määritetyn päätepisteet. Kun poistat liikenteen hallinta-profiili, profiilin itse ja profiilin tietoja säilyy muuttumattomana ja voi muokata liikenteen hallinta käyttöliittymässä. Kun haluat ottaa käyttöön uudelleen profiilin, voit helposti tehdä niin Azure-portaaliin ja viitteitä jatkuu. Kun luot liikenteen hallinta profiilin Azure-portaalissa, se on automaattisesti käytössä.

## <a name="to-disable-a-profile"></a>Profiilin poistaminen käytöstä

1. Muokkaa DNS-resurssitietue haluamasi tietuetyypin ja osoitin toinen nimi tai tiettyyn kohtaan IP-osoitteen käyttäminen Internetissä Internet DNS-palvelimeen. Muuta toisin sanoen DNS-resurssitietue Internet DNS-palvelimeen, niin, että se ei enää käyttää CNAME-resurssitietue, joka osoittaa toimialueen liikenteen hallinta profiilin nimi.
1. Liikenne lopetetaan parhaillaan ohjataan päätepisteet profiilin liikenteen hallinta-asetusten avulla.
1. Valitse profiili, jonka haluat poistaa käytöstä. Valitse profiili-liikenteen hallinta-sivulla on korostettu profiilin napsauttamalla profiilinimi viereiseen sarakkeeseen. Älä valitse profiili tai nimen vieressä olevaa nuolta, kun tämä siirtää sinut profiilin asetukset-sivun.
1. Kun olet valinnut profiilin, valitse sivun alareunassa Poista käytöstä.

## <a name="to-enable-a-profile"></a>Jos haluat ottaa käyttöön profiili

1. Valitse profiili, jonka haluat ottaa käyttöön. Valitse profiili-liikenteen hallinta-sivulla on korostettu profiilin napsauttamalla profiilinimi viereiseen sarakkeeseen. Älä valitse profiili tai nimen vieressä olevaa nuolta, kun tämä siirtää sinut profiilin asetukset-sivun.
1. Kun olet valinnut profiilin, valitse Ota käyttöön sivun alareunassa.
1. Muokkaa DNS-resurssitietue CNAME-tietueen tyyppiä, joka yhdistää yrityksen toimialuenimen liikenteen hallinta profiilin toimialuenimi käyttämään Internet DNS-palvelimeen. Lisätietoja on [kohdassa yrityksen Internet-toimialueen liikenteen hallinta-toimialueeseen](traffic-manager-point-internet-domain.md).
1. Liikenne alkaa parhaillaan ohjataan päätepisteet uudelleen.

## <a name="delete-a-profile"></a>Poista profiili


1. Varmista, että DNS-resurssitietue Internet DNS-palvelimeen ei enää käyttää CNAME-resurssitietue, joka osoittaa toimialueen liikenteen hallinta profiilin nimi.
1. Valitse profiili, jonka haluat poistaa. Valitse profiili-liikenteen hallinta-sivulla Korosta profiilin
1. Jos valitset profiilin viereiseen sarakkeeseen. Älä valitse profiili tai nimen vieressä olevaa nuolta, kun tämä siirtää sinut profiilin asetukset-sivun.
1. Kun olet valinnut profiilin, valitsemalla Poista sivun alareunassa.

## <a name="next-steps"></a>Seuraavat vaiheet

[Päätepisteen ottaminen käyttöön tai liikenteen hallinta - poistaminen käytöstä](disable-or-enable-an-endpoint.md)

[Määritä automaattisesti reititys menetelmä](traffic-manager-configure-failover-routing-method.md)

[PYÖRISTÄ-funktiota Mikko reititys menetelmä määrittäminen](traffic-manager-configure-round-robin-routing-method.md)

[Määritä suorituskyvyn reititys menetelmä](traffic-manager-configure-performance-routing-method.md)

[Vianmäärityksen liikenteen hallinta heikentynyt tila](traffic-manager-troubleshooting-degraded.md)

