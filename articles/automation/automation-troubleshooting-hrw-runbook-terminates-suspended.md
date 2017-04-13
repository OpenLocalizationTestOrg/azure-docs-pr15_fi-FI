<properties
   pageTitle="Hybrid Runbookin työntekijä: Runbookin työn lopettaa Suspended tilassa | Microsoft Azure"
   description="Ongelmia syyt ja ratkaisut Hybrid Runbookin työntekijä projektin päättyminen-virheen."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hybrid Runbookin työntekijä: Runbookin työn lopettaa Suspended tilassa

## <a name="summary"></a>Yhteenveto

Oman runbookin keskeytetään jälkeen yritetään suorittaa kolme kertaa. On ehdot, jotka saattavat keskeyttää onnistuu runbookin ja Aiheeseen liittyvät virheilmoitus ei ole osoittaa, miksi lisätiedot. Tässä artikkelissa on vianmääritysohjeita Hybrid Runbookin työntekijän runbookin suoritusvirheistä liittyvien ongelmien varalta.

Jos tässä artikkelissa ei käsitellä Azure ongelmaa, käy Azure [MSDN](https://azure.microsoft.com/support/forums/)ja Pinon ylivuoto. Voit julkaista ongelmaa nämä keskustelupalstalle tai [ @AzureSupport Twitter-](https://twitter.com/AzureSupport). Voit myös jättää Azure tukipyyntö valitsemalla **tuen saaminen** [Azure tuki](https://azure.microsoft.com/support/options/) -sivustossa.

## <a name="symptom"></a>Oire

Runbookin suorittaminen epäonnistui, ja funktio palauttaa virheen on "" Aktivoi"ei voi suorittaa, koska prosessi on pysähtynyt odottamatta työ-toiminto. Työn toiminnon yritettiin 3 kertaa."


## <a name="cause"></a>Syy

On useita virheen mahdollisia syitä: 

  1. Hybrid työntekijä on välityspalvelimen tai palomuurin takana
  2. Hybrid työntekijä on käynnissä tietokoneessa on pienempi kuin laitteiston [vaatimukset](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. Runbooks ei voi todentaa paikallisen tiedoilla


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Syy 1: Hybrid Runbookin työntekijä on välityspalvelimen tai palomuurin takana

Hybrid Runbookin työntekijä on käynnissä tietokoneessa on palomuurin tai välityspalvelimen palvelimen takana ja lähtevien verkkokäyttö ei ehkä sallittu tai määritetty oikein.

### <a name="solution"></a>Ratkaisu

Varmista, että tietokone on lähtevä pääsy *. cloudapp.net porttiin 443, 9354 ja 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Syy 2: Tietokoneessa on pienempi kuin laitteistovaatimukset

Hybrid Runbookin työntekijä-tietokoneissa on täytettävä laitteiston vähimmäisvaatimukset ennen kuin määrität sen isännöimiseen tätä ominaisuutta. Muussa tapauksessa muiden taustaprosessit ja aiheutuvat runbooks suorituksen aikana ristiriita resurssien käytön mukaan tietokone tullut yli käytössä ja aiheuttaa runbookin työn viivyttää tai aikakatkaisu. 

### <a name="solution"></a>Ratkaisu 

Varmista ensin nimetyn Hybrid Runbookin työntekijä-toiminnon tietokoneesi täyttää laitteiston vähimmäisvaatimukset.  Jos se ei seurata suorittimen ja muistin käyttö korrelaatio Hybrid Runbookin työntekijä prosessien suorituskykyä ja Windows määrittämiseen.  Jos näkyvissä on muistia tai suorittimen paine, tämä saattaa johtua ei tarvitse päivittää tai lisätä uusia suorittimien lisää muistia-virheen ratkaiseminen ja osoitteiden resurssin pullonkaula. Vaihtoehtoisesti voit valita eri Laske resurssi, joka voi tukea vähimmäisvaatimukset ja skaalausta, kun työmäärän kasvua on tarpeen.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Syy 3: Runbooks ei voi todentaa paikallisen tiedoilla

### <a name="solution"></a>Ratkaisu

Tarkista **Microsoft SMA** tapahtumaloki vastaavan tapahtuman kuvaukset *Win32 prosessin päättyi koodiin [4294967295]*.  Tämän virheen syynä on et vielä määritetty todennus oman runbooks tai määritetty Hybrid työntekijä-ryhmässä Suorita nimellä tunnistetietojen.  Tarkista Vahvista todennus on määritetty oikein, että runbooks [Runbookin käyttöoikeudet](automation-hybrid-runbook-worker.md#runbook-permissions) .  


 

