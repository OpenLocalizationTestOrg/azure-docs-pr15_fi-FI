<properties
 pageTitle="Cloud palvelun käyttöönoton vianmääritys | Microsoft Azure"
 description="On muutamia yleisiä ongelmia, voit suorittaa kyselyjä otettaessa pilvipalveluun Azure. Tässä artikkelissa on joitakin niistä ratkaisuja."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Cloud palvelun käyttöönoton ongelmien vianmääritys

Asentaessasi cloud-palvelun sovelluspaketin Azure voit hankkia tietoja käyttöönoton Azure-portaalissa **Ominaisuudet** -ruudusta. Tässä ruudussa voit käyttää tietoja auttaa pilvipalvelussa sijaitsevaan liittyvien ongelmien vianmääritys ja voit antaa nämä tiedot tukemaan Azure uusi tukipyyntö avattaessa.

Voit etsiä **Ominaisuudet** -ruudussa seuraavasti:

* Azure-portaalissa pilvipalvelussa sijaitsevaan käyttöönoton, **kaikki**asetukset ja valitse **Ominaisuudet**.
* Valitse Azure perinteinen portal pilvipalvelussa sijaitsevaan käyttöönotto- **raporttinäkymät-IKKUNAN**oikeassa alakulmassa (kohdassa **quick glance**)-sivun sijaitsee. Huomaa, että tämä ruutu ei ole "Ominaisuudet" ei nimeä.

> [AZURE.NOTE] Voit kopioida **Ominaisuudet** -ruudussa sisällön Leikepöytä napsauttamalla ruudun oikeassa alakulmassa olevaa kuvaketta.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Ongelma:-sivuston käyttäminen ei onnistu, mutta Omat käyttöönoton on käynnissä ja kaikki roolin esiintymät ovat valmiina

Sivuston URL-linkki näkyy portaalissa ei ole portti. Sivustot oletusportti on 80. Jos sovellus on määritetty toimimaan eri portin, oikea porttinumero on lisättävä URL-osoitteeseen sivuston käytettäessä.

1. Valitse Azure-portaalissa pilvipalvelussa sijaitsevaan käyttöönotto.
2. Azure-portaalin **Ominaisuudet** -ruutuun ja valitse portit rooli-esiintymien (kohdassa **Syötteen päätepisteet**).
3. Jos portti ei ole 80, Lisää Porttiarvo URL-osoite, kun käytät sovellus. Voit määrittää oletusportti, kirjoittamalla URL-osoite, jota seuraa kaksoispiste (:), perään porttinumero ilman välilyöntejä.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Ongelma: Rooli-esiintymät kierrätetään ilman me mitään

Palvelun kenties tapahtuu automaattisesti, kun Azure havaitsee ongelman solmujen ja siirtää sen vuoksi roolin esiintymät uusien solmujen. Jos näin tapahtuu, saatat nähdä kierrättäminen automaattisesti roolin kopioita. Jos haluat tarkistaa, jos palvelun kenties tapahtui:

1. Valitse Azure-portaalissa pilvipalvelussa sijaitsevaan käyttöönotto.
2. Azure-portaalin **Ominaisuudet** -ruudussa Tarkista tiedot ja määrittää, onko palvelun kenties tapahtui aika, joka havaittujen kierrättäminen roolit.

Roolien myös Roskakorin suunnilleen kerran kuukaudessa host OS ja Vieras OS päivitysten aikana.  
Lisätietoja on seuraavassa blogimerkinnässä [Roolin esiintymän käynnistyy määräpäivän OS päivityksiä](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Ongelma: voin ei voi tehdä VIP Vaihda ja virhesanoma

VIP Vaihda ei voi suorittaa, jos käyttöönoton päivitys on käynnissä. Päivitysten tapahtuvat automaattisesti kun:

* Uuden vieras-käyttöjärjestelmän on käytettävissä ja Automaattiset päivitykset on määritetty.
* Palvelun kenties tapahtuu.

Voit tarkistaa, jos automaattisen päivityksen estää tekevät VIP vaihtaminen:

1. Valitse Azure-portaalissa pilvipalvelussa sijaitsevaan käyttöönotto.
2. Azure-portaalin **Ominaisuudet** -ruudussa Etsi **tila**-kentän arvo. Jos se on **Valmis**, valitse **edellisen toiminnon** nähdäksesi, jos jokin viimeksi on tapahtunut voivat estää VIP swap.
3. Toista vaiheet 1 ja 2 tuotannon käyttöönottoa varten.
4. Jos automaattinen päivitys on meneillään, odota sen päättymistä, ennen kuin yrität VIP swap.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Ongelma: Roolin esiintymän toiston välillä aloittaminen, alustaminen onnistuu, varattu tai pysäytetään

Tämä ehto saattaa johtua sovelluksen koodi, paketti tai määritysten tiedosto. Tässä tapauksessa sinun pitäisi nähdä tilan muuttaminen muutaman minuutin välein ja Azure portaalin saattaa sano jotain kuten **Recycling**, **varattu**tai **alustaminen onnistuu**. Tämä tarkoittaa, ettei jotakin vikaa sovellus, jossa on pitäminen roolin esiintymän suorittamisen.

Lisätietoja ongelman vianmäärityksestä on artikkelissa blogimerkinnässä [Azure PaaS Laske diagnostiikka-tietoihin](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) ja [yleisiä ongelmia, jotka aiheuttavat roolien hallinta](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Ongelma: Omat sovellus ei toimi

1. Valitse rooli esiintymän Azure-portaalissa.
2. Azure-portaalin **Ominaisuudet** -ruudussa Ota huomioon seuraavat ehdot voit ratkaista ongelman:
   * Jos roolin esiintymän viimeksi lakkasi (Voit tarkistaa arvon **Keskeytä määrä**)-käyttöönoton voi päivittää. Odota nähdäksesi, jos roolin esiintymän jatkaa omalla toiminta.
   * Jos roolin esiintymä on **varattu**, tarkista sovelluksen koodi nähdäksesi, jos [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) tapahtuma käsitellään. Voit joutua lisättävät tai korjattavat lisäkoodin, joka käsittelee tapahtuman.
   * Käymällä läpi diagnostiikkatiedot ja vianmäärityksen skenaarioita blogimerkinnässä [Azure PaaS Laske diagnostiikka-tietoihin](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

>[AZURE.WARNING] Jos pilvipalvelussa sijaitsevaan roskakorista, voit palauttaa ominaisuuksien käyttöönotto-pyyhkiminen tehokkaasti alkuperäisen ongelman tiedot.

## <a name="next-steps"></a>Seuraavat vaiheet

Näytä lisää [artikkeleita vianmääritys](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) pilvipalveluihin.

Cloud palvelun roolin ongelmien vianmääritys Azure PaaS diagnostiikka tietojen avulla, kohdassa [Kevin Williamson blogin sarja](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
