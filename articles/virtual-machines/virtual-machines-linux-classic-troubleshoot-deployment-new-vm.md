<properties
   pageTitle="Vianmääritys Linux AM käyttöönoton perinteinen | Microsoft Azure"
   description="Vianmääritys, kun luot uuden Linux virtual koneen Azure perinteinen käyttöönotto"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
  ms.service="virtual-machines-linux"
  ms.workload="na"
  ms.tgt_pltfrm="vm-linux"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/06/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-classic-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a>Perinteinen käyttöönoton luomisessa uuden Linux virtual koneen Azure ongelmien vianmääritys

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-selectors](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-selectors-include.md)]

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Kerää valvonta

Aloita vianmääritys, kerää valvontalokien liittyvän ongelman virheestä.

Azure-portaalissa, valitse **Selaa** > **näennäiskoneiden** > *Windows virtuaalikoneen* > **asetukset** > **valvontalokien**.

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

**Y:** Jos Käyttöjärjestelmä on Linux generalized, ja se on ladattu ja/tai oikeus generalized asetus tallennetaan, valitse voi virheitä. Vastaavasti jos Käyttöjärjestelmä on Linux erilaisia, ja se on ladattu tai siepata erityinen-asetus ja valitse ei ole virheitä.

**Lataa:**

**N<sup>1</sup>:** Jos Käyttöjärjestelmä on Linux generalized, ja se tuodaan kuin erilaisia, näkyviin tulee valmistelu aikakatkaisu-virheen, koska AM pysähtyy valmistelu vaiheessa.

**N<sup>2</sup>:** Jos Käyttöjärjestelmä on Linux erilaisia ja se on ladattu palvelimeen, kuten generalized, saat valmistelu epäonnistui virheen, koska uudet AM suoritetaan alkuperäisen tietokonenimi, käyttäjänimellä ja salasanalla.

**Ratkaisu:**

Sekä näiden virheiden ratkaisemisesta Lataa alkuperäisen SITEN, käytettävissä paikallinen, samaa asetusta, OS (generalized/erilaisia) kanssa. Voit ladata että generalized, muista suorittaa - deprovision ensin. Lisätietoja on kohdassa [Luo ja lataa Virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä](virtual-machines-linux-classic-create-upload-vhd.md) .

**Sieppaa virheitä:**

**N<sup>3</sup>:** Jos Käyttöjärjestelmä on Linux generalized ja se on tallennettu nimellä erilaisia, näkyviin tulee valmistelu aikakatkaisu-virheen koska alkuperäisen AM ei voi käyttää, kun se on merkitty generalized.

**N<sup>4</sup>:** Jos Käyttöjärjestelmä on Linux erilaisia ja se kirjataan kuin generalized, saat valmistelu epäonnistui virheen, koska uudet AM suoritetaan alkuperäisen tietokonenimi, käyttäjänimellä ja salasanalla. Myös alkuperäinen AM ei ole käytettävissä, koska se on merkitty vain erityinen.

**Ratkaisu:**

Sekä näiden virheiden ratkaisemisesta poistaa nykyisen kuvan portaalin ja [siepata sen nykyisen näennäiskiintolevyjen](virtual-machines-linux-classic-capture-image.md) (generalized/erilaisia) OS samaa asetusta, kanssa.

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a>Ongelma: Mukautettu / valikoima / marketplace kuva; kohdistus-virhe
Tämä virhe ilmenee tilanteissa, uusi pyyntö AM lähettäessään klusteriin, joka ei ole sopimaan pyynnön tilaa tai ei tue vaadittua AM kokoa. Ei ole mahdollista sekoita eri tieto VMs saman pilvipalvelussa. Jos haluat luoda uuden AM kuin pilvipalvelussa sijaitsevaan tukee koon, epäonnistuu niin pyynnön suorittaminen.

Voit luoda uuden AM pilvipalvelussa rajoitukset voi tulla jokin seuraavista tilanteista aiheutuvat virhe.

**Syy 1:** Cloud-palvelu on kiinnitetty tiettyyn klusteri tai se on linkitetty affiniteetti ryhmään ja näin ollen kiinnitetty tiettyyn klusteri suunniteltu ominaisuus. Niin uusi Laske resurssi pyytää siten, että affiniteetti ryhmän on jälleen kohtaa, johon olemassa olevat resurssit isännöidään samassa klusterissa. Kuitenkin samassa klusterissa saattaa ei tue pyydetty AM kokoa tai on ei riittävästi käytettävissä olevan tilan, tuloksena on kohdistus-virheen. Tämä on TOSI, onko uudet resurssit luodaan uusi pilvipalvelussa tai aiemmin pilvipalvelussa kautta.

**Ratkaisu 1:**

- Luo uusi pilvipalvelussa ja liittää sen alueen tai alue-pohjaiseen virtual verkkoon.
- Luo uusi AM uusi pilvipalvelussa.
  Jos saat virheilmoituksen, kun yrität luoda uuden pilvipalvelussa, yritä uudelleen myöhemmin tai muuttaa pilvipalvelussa alue.

> [AZURE.IMPORTANT] Jos yritit luoda uuden AM aiemmin pilvipalvelussa, mutta ei voitu ja luoda uuden AM uusi pilvipalveluun, voit yhdistää kaikki VMs saman pilvipalvelussa. Voit tehdä Poista aiemmin pilvipalvelussa VMs ja siepata niitä-niiden levyjen uusi pilvipalvelussa. Kuitenkin on tärkeää muistaa, että uusi pilvipalvelussa on uusi nimi ja VIP, joten sinun on päivitettävä näitä kaikki riippuvuudet, jotka käyttävät näitä tietoja tällä hetkellä aiemmin pilvipalvelussa.

**Syy 2:** Cloud-palvelu on liitetty virtual verkkoon, joka on linkitetty affiniteetti-ryhmään, joten se on kiinnitetty tietyn klusteriin rakenne. Uuden Laske-resurssin pyytää siten, että affiniteetti ryhmän muodostetut vuoksi kohtaa, johon olemassa olevat resurssit isännöidään saman klusterin. Kuitenkin samassa klusterissa saattaa ei tue pyydetty AM kokoa tai on ei riittävästi käytettävissä olevan tilan, tuloksena on kohdistus-virheen. Tämä on TOSI, onko uudet resurssit luodaan uusi pilvipalvelussa tai aiemmin pilvipalvelussa kautta.

**Ratkaisu 2:**

- Luo uuden alueellisen virtual verkon.
- Luo uusi AM uuden virtual verkon.
- [Yhdistä aiemmin virtual verkon](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/) uusi virtual verkkoon. Katso lisätietoja [alueellisten virtual verkkojen](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/). Vaihtoehtoisesti voit [siirtää affiniteetti-ryhmä-pohjainen virtual verkon alueellisen virtual verkkoon](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), ja luo sitten uusi AM.

## <a name="next-steps"></a>Seuraavat vaiheet
Jos käytössä ilmenee ongelmia, kun Käynnistä pysäytetty Linux AM tai muuttaa aiemmin luodun Linux AM Azure-tietokannassa, katso [perinteinen käyttöönoton ongelmien vianmääritys uudelleenkäynnistyksen tai koon aiemmin luotuun Virtual Linux-Machine, Azure-tietokannassa](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md).
