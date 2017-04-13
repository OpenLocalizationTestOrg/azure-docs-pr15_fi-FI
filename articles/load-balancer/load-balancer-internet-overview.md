
<properties
   pageTitle="Vastakkaisten kuormituksen tasauspalvelun yleiskatsaus Internet | Microsoft Azure "
   description="Yleistä Internet vastakkaisten kuormituksen ja niiden ominaisuuksista. Miten kuormituksen tasauspalvelun toimii Azuren näennäiskoneiden ja pilvipalveluihin."
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internet-facing-load-balancer-overview"></a>Internet aukeaman kuormituksen tasauspalvelun yleiskatsaus

Azure kuormituksen yhdistää julkiseen IP-osoite ja portin määrän saapuvan liikenteen yksityinen IP-osoitetta ja porttia määrä virtuaalikoneen ja päinvastoin vastauksen tietoliikenteen virtual tietokoneesta. Lataa Vastatilin sääntöjen avulla voit jakaa tietyntyyppiset liikenne useita näennäiskoneiden tai palveluiden välillä. Voit esimerkiksi levittää web pyynnön liikenne kuormituksen useiden WWW-palvelimien tai web roolit.

Voit määrittää julkisen päätepisteen pilvipalvelussa, joka sisältää web roolit tai työntekijä roolit-palvelun määritystiedostossa (.csdef).

_Servicedefinition.csdef_ -tiedosto sisältää päätepisteen määritystä ja kun sinulla on useita web- tai Työntekijä roolin käyttöönottoa roolin esiintymät, kuormituksen päivitetään sen asetukset. Tapa lisätä esiintymät cloud käyttöönoton muuttuu esiintymän Laske-palvelun määritystiedosto (.csfg).

Seuraavassa kuvassa on salattu web liikenteestä, joka on jaettu kolmeen näennäiskoneiden 443 julkisista ja yksityisistä TCP-portin kesken kuormituksen päätepiste. Nämä kolme näennäiskoneiden on kuormituksen määrittäminen.

![julkinen kuormituksen tasauspalvelun Esimerkki](./media/load-balancer-internet-overview/IC727496.png))

Kuva 1 - kuormituksen päätepisteen salattuja web tietoliikenteen

Kun Internet-asiakkaat lähettää verkkosivun pyynnöt TCP-porttiin 443 pilvipalvelussa julkiseen IP-osoite, Azure ladata tasaustoiminto jakaa pyynnöt kolme näennäiskoneiden kuormituksen määrittäminen välillä. Saat lisätietoja kuormituksen tasauspalvelun algoritmit [ladata tasaustoiminto yhteenveto-sivu](load-balancer-overview.md#load-balancer-features).

Oletusarvon mukaan Azure kuormituksen jakaa verkkoliikennettä tasaisesti virtuaalikoneen useita kertoja. Voit myös määrittää istunnon affiniteetti, Lisätietoja on artikkelissa [kuormituksen tasauspalvelun jakauman tilassa](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisätietoja [sisäinen kuormituksen](load-balancer-internal-overview.md) ja ymmärtää helpommin, mitä kuormituksen on mahtuu cloud käyttöönottoa varten.

Voit myös [luomisesta vastakkaisten kuormituksen Internet](load-balancer-get-started-internet-arm-ps.md) - ja minkä tyyppistä [jakauman tilassa](load-balancer-distribution-mode.md) tietyn kuormituksen tasauspalvelun verkon liikenne asetusten määrittäminen.

Jos sovellus täytyy pidä yhteydet toiminnassa palvelinten kuormituksen tasauspalvelun takana, ymmärrät Lisää [Käyttämättömät TCP](load-balancer-tcp-idle-timeout.md)-aikakatkaisu asetuksista kuormituksen. Sen avulla tietoja käyttämättömänä yhteyden toiminnan, kun käytät Azure kuormituksen.
