<properties
   pageTitle="AM uudelleenkäynnistyksen tai koon ongelmat | Microsoft Azure"
   description="Perinteinen käyttöönoton vianmääritys uudelleenkäynnistyksen tai aiemmin luotuun Virtual Linux-Machine, Azure-tietokannassa koon muuttaminen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="required"
   ms.date="09/20/2016"
   ms.devlang="na"
   ms.author="delhan"/>

# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Perinteinen käyttöönoton vianmääritys uudelleenkäynnistyksen tai aiemmin luotuun Virtual Linux-Machine, Azure-tietokannassa koon muuttaminen

> [AZURE.SELECTOR]
- [Perinteinen](../articles/virtual-machines/virtual-machines-linux-classic-restart-resize-error-troubleshooting.md)
- [Resurssien hallinta](../articles/virtual-machines/virtual-machines-linux-restart-resize-error-troubleshooting.md)

Kun yrität käynnistää pysäytetty Azure-virtuaalikoneen (AM) tai aiemmin Azure-AM koon, kohtaat Yleinen virhe on kohdistus-virheen. Tämä virhe syntyy silloin, kun klusterin tai alueen joko ei ole käytettävissä olevien resurssien tai ei tue pyydetty AM kokoa.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Kerää valvonta

Aloita vianmääritys, kerää valvontalokien liittyvän ongelman virheestä.

Azure-portaalissa, valitse **Selaa** > **näennäiskoneiden** > _Linux virtuaalikoneen_ > **asetukset** > **valvontalokien**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Ongelma: Virhe pysäytetty AM käynnistettäessä

Yrität Käynnistä pysäytetty AM, mutta saat kohdistus-virheen.

### <a name="cause"></a>Syy

Käynnistä pysäytetty AM pyyntö on osoitteessa alkuperäisen klusterin, joka isännöi pilvipalvelussa sijaitsevaan yritetään. Kuitenkin klusterin ei ole käytettävissä pyyntöä tilaa.

### <a name="resolution"></a>Tarkkuus

* Luo uusi pilvipalvelussa ja liittää sen joko alueen tai alue-pohjaiseen virtual verkkoon, mutta ei affiniteetti ryhmälle.

* Poista pysäytetty AM.

* Luo uusi pilvipalvelussa AM levyjen avulla.

* Aloita uudelleenluotuun AM.

Jos saat virheilmoituksen, kun yrität luoda uuden pilvipalvelussa, yritä uudelleen myöhemmin tai muuttaa pilvipalvelussa alue.

> [AZURE.IMPORTANT] Uusi pilvipalvelussa on uusi nimi ja VIP, joten sinun on muutettava kaikki riippuvuudet, joka käyttää aiemmin luotuja pilvipalvelussa tiedoista.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Ongelma: Virhe, kun aiemmin AM koon muuttaminen

Yrität aiemmin AM kokoa, mutta saat kohdistus-virheen.

### <a name="cause"></a>Syy

Pyynnön koon AM on osoitteessa alkuperäisen klusterin, joka isännöi pilvipalvelussa sijaitsevaan yritetään. Klusterin ei tue pyydetyt AM kokoa.

### <a name="resolution"></a>Tarkkuus

Pienentää pyydetty AM ja yritä uudelleen koon pyynnön.

* Selaa **kaikkia** > **näennäiskoneiden (perinteinen)** > _virtuaalikoneen_ > **asetukset** > **kokoa**. Katso lisätietoja kohdasta [kokoa virtuaalikoneen](https://msdn.microsoft.com/library/dn168976.aspx).

Jos ei ole mahdollista AM koon pienentämiseksi, toimi seuraavasti:

  * Luo uusi pilvipalveluun, varmistaa, että se on ei ole linkitetty affiniteetti ryhmään ja ei ole liitetty virtual verkkoon, joka on linkitetty affiniteetti ryhmään.

  * Luo uusi, suuremman AM sitä.

Voit yhdistää kaikki VMs saman pilvipalvelussa. Jos aiemmin pilvipalvelussa liittyy alue-pohjaiseen virtual verkkoon, voit muodostaa uusi pilvipalvelussa aiemmin virtual verkkoon.

Jos aiemmin pilvipalvelussa ei ole liitetty alue-pohjaiseen virtual verkkoon, sinun tulee poistaa aiemmin pilvipalvelussa VMs ja luo ne uuteen pilvipalvelussa niiden levyjen. Kuitenkin on tärkeää muistaa, että uusi pilvipalvelussa on uusi nimi ja VIP, joten sinun on päivitettävä näitä kaikki riippuvuudet, jotka käyttävät näitä tietoja tällä hetkellä aiemmin pilvipalvelussa.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos käytössä ilmenee ongelmia, kun luot uuden Linux AM Azure, katso [käyttöönoton ongelmien vianmääritys luomisessa uuden Linux virtual koneen Azure-tietokannassa](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
