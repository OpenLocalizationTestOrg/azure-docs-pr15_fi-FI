<properties
    pageTitle="Mitä tehdä, jos vaikuttavat Azure Virtual verkkojen Azure keskeytetty | Microsoft Azure"
    description="Katso, mitä tehdään, jos vaikuttavat Azure Virtual verkkojen Azure keskeytetty."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtuaalinen verkon – Liiketoiminnan jatkuvuus

##<a name="overview"></a>Yleiskatsaus

Virtual Network (VNet) on looginen esitys verkoston pilveen. Voit määrittää oman yksityisen IP-osoitetilaa ja määritetään verkon aliverkosta kyselyjä. VNets toimii luota rajan isännöimiseen Laske resurssien kuten Azuren näennäiskoneiden ja pilvipalveluihin (web/työntekijä roolit). VNet avulla suoraan yksityinen IP-tietoliikenne ylläpidettävä sen resurssien välillä. Virtuaalinen verkon voidaan linkittää myös paikalliseen verkkoon jonkin hybrid-asetukset, kuten VPN-yhdyskäytävän tai ExpressRoute kautta.
 
VNet luodaan alueen puitteissa. Voit luoda VNets saman osoitetilan kaksi eri alueiden kanssa (eli US Itä- ja US Länsi, mutta ei voi yhdistää ne keskenään suoraan). 

##<a name="business-continuity"></a>Liiketoiminnan jatkuvuus

Voi olla usealla eri tavalla, sovellus saattaa katketa. Tietyn alueen voitu kokonaan kokonaisuudessaan luonnollinen huono tai osittainen huono useita laitteita ja palveluja virheen vuoksi. Vaikutus VNet-palvelusta on eri tavoilla.

**K: Mitä teen, jos koko alue käyttökatkosta? Toisin sanoen, jos alue on kokonaan edellyttäisi luonnollinen huono vuoksi? Mitä tapahtuu isännöidään alueen Virtual verkkoja?**

V: Virtual verkko- ja tarvittavien alueen resurssit pysyy palvelun keskeytymisen aikana ei ole käytettävissä.

![Yksinkertainen Virtual verkkokaavio](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**K: Miten voin luoda uudelleen samassa Virtual verkossa toisella alueella?**

A: virtual Network (VNet) on melko kevyt resurssi. Voit kutsua Azure-ohjelmointirajapinnan VNet luomalla saman osoitetilan toisella alueella. Luo uudelleen samassa ympäristössä, joka on ollut tarvittavien alueen, edellyttää API-puheluissa ottamaan pilvipalveluihin (web/työntekijä roolit) ja näennäiskoneiden, joita haluat käyttää uudelleen. Käytössä on myös VPN-yhdyskäytävän asettamasi ja Yhdistä paikalliseen verkkoon, jos haluat käyttää yhteys paikalliseen (esimerkiksi yhdistelmäympäristössä).

Ohjeet luomiseen VNet löytyy [tähän](./virtual-networks-create-vnet-arm-pportal.md). 

**K: Voinko käyttävien VNet tietyn alueen luodaan uudelleen toisen alueen etuajassa?**

V: Kyllä, voit luoda saman yksityisen IP-osoitetilan ja resurssien käyttäminen kaksi eri alueiden etuajassa kaksi VNets. Jos asiakas on isännöinnin internet vastakkaisten palvelut VNet ne voi määrittää liikenteen hallinta geo reitin liikennettä alue, joka on aktiivinen. Kuitenkin asiakkaan voida yhdistää kaksi VNets saman osoitetilan paikalliseen verkkoon, kun se aiheuttaa reititys ongelmat. Asiakkaan muodostaa huono aika ja VNet jonkin alueen menetys sisältää vastaavat osoitetilaa paikalliseen verkkoon käytettävissä alueen muiden VNet.

Ohjeet luomiseen VNet löytyy [tähän](./virtual-networks-create-vnet-arm-pportal.md).
