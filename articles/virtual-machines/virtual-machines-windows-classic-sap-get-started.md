<properties
   pageTitle="SAP: N käyttäminen Windows näennäiskoneiden | Microsoft Azure"
   description="Tyhjennä käyttämisestä SAP Microsoft Azure Windows-näennäiskoneiden (VMs)"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>SAP: in käyttäminen näennäiskoneiden Windows Azure-tietokannassa

Cloud tietojenkäsittely on yleisesti käytetty termi, joka on kasvun enemmän tärkeys sisällä IT, alaan pienten yritysten suuria ja kansainvälisissä yritysten ylöspäin. Microsoft Azure on Microsoftin, joka tarjoaa useita eri uusi pikavalikon Cloud Services-ympäristön. Kukin asiakas on nyt nopeasti valmistelu ja poista valmistelu sovellusten kuin pilvipalveluihin, joten niitä ei rajoitettu teknisiä tai budjetointi rajoitukset. Sen sijaan, että investointien ajasta ja kyselyjä laitteiston infrastruktuuri, yritykset voit keskittyä sovellus, liiketoimintaprosessien ja sen edut asiakkaat ja käyttäjille.

Microsoft tarjoaa Microsoft Azuren näennäiskoneiden kanssa täydellinen infrastruktuurin kuin Service (IaaS)-ympäristössä. SAP NetWeaver mukaan sovelluksia tuetaan-Azuren näennäiskoneiden (IaaS). Alla olevassa mallityökirjat kerrotaan, kuinka voit suunnitella ja toteuttaa SAP NetWeaver mukaan sovellukset-näennäiskoneiden Windows Azure-tietokannassa. Voit myös ottaa mukaan SAP NetWeaver sovellusten käyttöön [Linux näennäiskoneiden](virtual-machines-linux-classic-sap-get-started.md).

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP-Azure - NetWeaver HA

Otsikko: SAP NetWeaver-Azure - klusterointi SAP ASCS/SCS esiintymät Windows automaattisesti palvelinklusterissa käyttäminen Azure SIOS DataKeeper kanssa

Yhteenveto: "tässä asiakirjassa kerrotaan, miten voit määrittää erittäin käytettävissä SAP ASCS/SCS määritys Azure-SIOS DataKeeper avulla. SAP: N suojaa niiden yksittäisen kohdan virhe-osien, kuten SAP ASCS/SCS tai Enqueue replikointi palvelujen kanssa, jotka edellyttävät Jaettujen levyjen Windows automaattisesti palvelinklusterissa määrityksiä. SAP-komponentit ovat välttämättömiä SAP-järjestelmää toimintoja. Tämän vuoksi suuren käytettävyyden toiminnot on asetetaan paikalleen, varmista, että kyseisiä osia voidaan ylläpitää palvelimen tai AM kanssa Windows-klusterin määritykset paljas ja Hyper-V ympäristöissä tehdyksi epäonnistui. Vuodesta elokuussa 2015 Azure-itse ei voi antaa Jaettujen levyjen, joka vaaditaan Windowsin perusteella tarvittavat komponentit kriittinen SAP erittäin käytettävissä määrityksiä. Kuitenkin tuotteen mukaan SIOS DataKeeper avulla, Windows Server vikasietoklusteriin käyttömahdollisuudet SAP ASCS/SCS tarvittaessa voit rakentaa Azure IaaS-ympäristössä. Tässä asiakirjassa kerrotaan vaihe vaihe vaiheelta asennusohjeet Windows automaattisesti palvelinklusterissa määrityksen ja jaetun levyn myöntämä SIOS Datakeeper Azure-tietokannassa. Paperin kerrotaan käyttömahdollisuudet tiedot, jotka tekevät suuren käytettävyyden määritykset, toimi näin ollen optimaalisten tavalla Azure-, Windows- ja SAP-puolella. Paperin täydentää SAP asennusohjeet ja SAP muistiinpanoja, joka edustaa asennusten ja SAP-ohjelmisto-versioiden ensisijainen resurssit ympäristöissä annettu.

Päivitetty: August 2015

[Lataa opas](http://go.microsoft.com/fwlink/?LinkId=613056)
