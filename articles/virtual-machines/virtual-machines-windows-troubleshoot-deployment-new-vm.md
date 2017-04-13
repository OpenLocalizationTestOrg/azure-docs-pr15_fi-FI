<properties
   pageTitle="Vianmääritys Windows AM käyttöönotto-RM | Microsoft Azure"
   description="Resurssien hallinnan käyttöönotto ongelmien vianmääritys, kun luot uuden virtual koneen Windows Azure"
   services="virtual-machines-windows, azure-resource-manager"
   documentationCenter=""
   authors="JiangChen79"
   manager="felixwu"
   editor=""
   tags="top-support-issue, azure-resource-manager"/>

<tags
  ms.service="virtual-machines-windows"
  ms.workload="na"
  ms.tgt_pltfrm="vm-windows"
  ms.devlang="na"
  ms.topic="article"
  ms.date="09/09/2016"
  ms.author="cjiang"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-windows-virtual-machine-in-azure"></a>Uuden Windows virtual koneen luominen Azure Resurssienhallinta käyttöönoton ongelmien vianmääritystä

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Kerää valvonta

Aloita vianmääritys, kerää valvontalokien liittyvän ongelman virheestä. Seuraavat linkit sisältävät yksityiskohtaiset tiedot voit seurata prosessia.

[Resurssien ryhmä-käyttöönotoissa Azure-portaalissa vianmääritys](../resource-manager-troubleshoot-deployments-portal.md)

[Valvonta-toimintojen resurssien hallinta](../resource-group-audit.md)

[AZURE.INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[AZURE.INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

**Y:** Jos Käyttöjärjestelmä on Windows generalized, ja se on ladattu ja/tai oikeus generalized asetus tallennetaan, valitse voi virheitä. Vastaavasti jos Käyttöjärjestelmä on Windows erilaisia, ja se on ladattu tai siepata erityinen-asetus ja valitse ei ole virheitä.

**Lataa:**

**N<sup>1</sup>:** Jos Käyttöjärjestelmä on Windows generalized ja se tuodaan kuin muokattava, näkyviin tulee valmistelu aikakatkaisu-virheen jumissa Tervetuloa-ohjelman näytössä AM kanssa.

**N<sup>2</sup>:** Jos Käyttöjärjestelmä on Windows erilaisia ja se on ladattu palvelimeen, kuten generalized, näyttöön tulee valmistelu epäonnistui virheen kanssa AM jumissa Tervetuloa-ohjelman näytössä, koska uudet AM suoritetaan alkuperäisen tietokonenimi, käyttäjänimellä ja salasanalla.

**Tarkkuus**

Sekä näiden virheiden ratkaisemisesta käyttäminen (generalized/erilaisia) OS samaa asetusta, [Lisää-AzureRmVhd lataaminen alkuperäisen Näennäiskiintolevyn](https://msdn.microsoft.com/library/mt603554.aspx), käytettävissä olevat paikallisen. Voit ladata että generalized, muista sysprep suoritetaan ensin.

**Sieppaa virheitä:**

**N<sup>3</sup>:** Jos Käyttöjärjestelmä on Windows generalized ja se kirjataan kuin muokattava, näkyviin tulee valmistelu aikakatkaisu-virheen koska alkuperäisen AM ei voi käyttää, kun se on merkitty generalized.

**N<sup>4</sup>:** Jos Käyttöjärjestelmä on Windows erilaisia ja se on oteta talteen, kun generalized, näyttöön tulee valmistelu epäonnistui virheen, koska uudet AM suoritetaan alkuperäisen tietokonenimi, käyttäjänimellä ja salasanalla. Myös alkuperäinen AM ei ole käytettävissä, koska se on merkitty vain erityinen.

**Tarkkuus**

Sekä näiden virheiden ratkaisemisesta poistaa nykyisen kuvan portaalin ja [siepata sen nykyisen näennäiskiintolevyjen](virtual-machines-windows-vhd-copy.md) (generalized/erilaisia) OS samaa asetusta, kanssa.

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a>Ongelma: Mukautettu/valikoima/marketplace kuva; kohdistus-virhe
Tämä virhe syntyy tilanteissa, kun uusi AM pyyntö on kiinnitetty klusteriin, joka ei tue vaadittua AM koon tai ei ole sopimaan pyynnön tilaa.

**Syy 1:** Klusterin ei tue pyydetty AM kokoa.

**Ratkaisu 1:**

- Uudelleen käyttämällä AM pienemmäksi pyynnön.
- Jos pyydetty AM kokoa voi muuttaa:
  - Lopeta kaikki VMs käytettävyys määrittäminen.
  **Resurssin**ryhmät > *Resurssiryhmän* > **resurssien** > *käytettävyyden määrittäminen* > **näennäiskoneiden** > *virtuaalikoneen* > **Lopeta**.
  - Kun kaikki VMs keskeyttää, Luo uusi AM halutun kokoinen.
  - Aloita uusi AM ensin ja valitse pysäytetty VMs ja **Käynnistä-painiketta**.

**Syy 2:** Klusterin ei ole vapaa resurssit.

**Ratkaisu 2:**

- Yritä pyyntöä myöhemmin uudelleen.
- Jos uusi AM voi olla osa eri käytettävyys
  - Luo uusi AM eri käytettävyys, joka määrittää (sama alue).
  - Lisää uusi AM samassa virtual verkossa.

## <a name="next-steps"></a>Seuraavat vaiheet
Jos käytössä ilmenee ongelmia, kun käynnistät pysäytetty Windows-AM tai muuttaa olemassa olevan Windows-AM Azure-tietokannassa, katso [käyttöönoton vianmääritys Resurssienhallinta uudelleen tai aiemmin luodun Virtual Windows-tietokoneeseen, Azure-tietokannassa koon ongelmat](virtual-machines-windows-restart-resize-error-troubleshooting.md).
