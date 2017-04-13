<properties
   pageTitle="AM uudelleenkäynnistyksen tai koon ongelmat | Microsoft Azure"
   description="Liittyviä Resurssienhallinta käyttöönoton uudelleen tai aiemmin luotuun Virtual Linux-Machine, Azure-tietokannassa koon muuttaminen"
   services="virtual-machines-linux, azure-resource-manager"
   documentationCenter=""
   authors="Deland-Han"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>

<tags
   ms.service="virtual-machines-linux"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.devlang="na"
   ms.workload="required"
   ms.date="09/09/2016"
   ms.author="delhan"/>

# <a name="troubleshoot-resource-manager-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a>Liittyviä Resurssienhallinta käyttöönoton uudelleen tai aiemmin luotuun Virtual Linux-Machine, Azure-tietokannassa koon muuttaminen

Kun yrität käynnistää pysäytetty Azure-virtuaalikoneen (AM) tai aiemmin Azure-AM koon, kohtaat Yleinen virhe on kohdistus-virheen. Tämä virhe syntyy silloin, kun klusterin tai alueen joko ei ole käytettävissä olevien resurssien tai ei tue pyydetty AM kokoa.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Kerää valvonta

Aloita vianmääritys, kerää valvontalokien liittyvän ongelman virheestä. Seuraavissa linkeissä sisältää yksityiskohtaiset tiedot prosessi:

[Resurssien ryhmä-käyttöönotoissa Azure-portaalissa vianmääritys](../resource-manager-troubleshoot-deployments-portal.md)

[Valvonta-toimintojen resurssien hallinta](../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Ongelma: Virhe pysäytetty AM käynnistettäessä

Yrität Käynnistä pysäytetty AM, mutta saat kohdistus-virheen.

### <a name="cause"></a>Syy

Käynnistä pysäytetty AM pyyntö on osoitteessa alkuperäisen klusterin, joka isännöi pilvipalvelussa sijaitsevaan yritetään. Kuitenkin klusterin ei ole käytettävissä pyyntöä tilaa.

### <a name="resolution"></a>Tarkkuus

*   Lopeta kaikki käytettävyys määrittäminen VMs ja Käynnistä kukin AM.

  1. **Resurssin**ryhmät > _Resurssiryhmän_ > **resurssien** > _käytettävyyden määrittäminen_ > **näennäiskoneiden** > _virtuaalikoneen_ > **Lopeta**.

  2. Kun kaikki VMs lopettaa, valitse pysäytetty VMs ja valitse Aloita.

*   Yritä uudelleen pyynnön myöhemmin.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Ongelma: Virhe, kun aiemmin AM koon muuttaminen

Yrität aiemmin AM kokoa, mutta saat kohdistus-virheen.

### <a name="cause"></a>Syy

Pyynnön koon AM on osoitteessa alkuperäisen klusterin, joka isännöi pilvipalvelussa sijaitsevaan yritetään. Klusterin ei tue pyydetyt AM kokoa.

### <a name="resolution"></a>Tarkkuus

* Uudelleen käyttämällä AM pienemmäksi pyynnön.

* Jos pyydetty AM kokoa voi muuttaa:

  1. Lopeta kaikki VMs käytettävyys määrittäminen.

    * **Resurssin**ryhmät > _Resurssiryhmän_ > **resurssien** > _käytettävyyden määrittäminen_ > **näennäiskoneiden** > _virtuaalikoneen_ > **Lopeta**.

  2. Kun kaikki VMs keskeyttää, muuttaa suurentaminen haluamasi AM.
  3. Valitse kokoa on muutettu AM napsauttamalla **Käynnistä-painiketta**ja käynnistä sitten pysäytetty VMs.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos käytössä ilmenee ongelmia, kun luot uuden Linux AM Azure, katso [käyttöönoton ongelmien vianmääritys luomisessa uuden Linux virtual koneen Azure-tietokannassa](../virtual-machines/virtual-machines-linux-troubleshoot-deployment-new-vm.md).
