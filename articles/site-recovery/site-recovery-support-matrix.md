<properties
    pageTitle="Azure palauttaminen tuki matriisin | Microsoft Azure"
    description="Tuetut käyttöjärjestelmät ja -osien yhteenveto Azure sivuston palauttamista varten"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure palauttaminen tuki matriisi

Tässä artikkelissa on yhteenveto tuetut käyttöjärjestelmät ja Azure palauttaminen käyttöönotoissa osat. Luettelo tuetuista osat ja edellytykset on käytettävissä kunkin käyttöönottotapa kussakin ne vastaavat käyttöönoton artikkelin ja tässä asiakirjassa yhteenveto.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Tuetut käyttöjärjestelmät virtualization palvelimia


**Lähde** | **Kohde** | **Host OS**
---|---|---|--- 
**Hyper-V isännät (ilman VMM)** | Azure | Windows Server 2012 R2 ja uusimmat päivitykset
**Hyper-V isännät (VMM)** | Azure | Windows Server 2012 R2 ja uusimmat päivitykset
**Hyper-V isännät (VMM)** | Toissijainen VMM-sivusto | Vähintään Windows Server 2012: n uusimmat päivitykset
**VMware isännät/vCenter** | Azure | vCenter 5.5 tai 6.0 (vain 5,5 ominaisuuksien tuki) <br/><br/> vSphere 6.0, 5.5 tai 5.1 uusimmat päivitykset
**VMware isännät/vCenter** | Toissijainen VMware sivuston | vCenter 5.5 tai 6.0 (vain 5,5 ominaisuuksien tuki) <br/><br/> vSphere 6.0, 5.5 tai 5.1 uusimmat päivitykset

## <a name="supported-requirements-for-replicated-machines"></a>Replikoitua koneet tuetut vaatimukset

**Lähde** | **Mikä on replikoida** | **Kohde** | **Host OS**
---|---|---|--- 
**Hyper-V VMs** | Mikä tahansa työmäärää | Azure | Mikä tahansa Vieras OS [Azure tukemat](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs on täytettävä [Azure vaatimukset](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (VMM)** | Mikä tahansa työmäärää | Azure | Mikä tahansa Vieras OS [Azure tukemat](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs on täytettävä [Azure vaatimukset](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (VMM)** | Mikä tahansa työmäärää | Toissijainen VMM-sivusto | Mikä tahansa Vieras OS [tukemat Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**VMware VMs** | Mikä tahansa työmäärää AM Windows-käyttöjärjestelmässä | Azure | 64-bittinen Windows Server 2012 R2, Windows Server 2012: ssa ja Windows Server 2008 R2 ja milloin vähintään SP1<br/><br/> VMs on täytettävä [Azure vaatimukset](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Kaikki käynnissä olevat Linux AM työmäärää | Azure | Punainen on Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5 6.6, 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, jossa punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Muistitilaa: tiedostojärjestelmää (EXT3, ETX4, ReiserFS, XFS); Monipolku ohjelmisto-laite Mapper (monipolku)). Levyn hallinta: (LVM2). HP CCISS ohjauskoneen tallennustilan fyysinen palvelinten ei tueta. ReiserFS tiedostojärjestelmän tuetaan vain SUSE Linux Enterprise Server 11 SP3.<br/><br/> VMs on täytettävä [Azure vaatimukset](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Mikä tahansa työmäärää AM Windows-käyttöjärjestelmässä | Toissijainen VMware sivuston | 64-bittinen Windows Server 2012 R2, Windows Server 2012: ssa ja Windows Server 2008 R2 ja milloin vähintään SP1
**VMware VMs** | Kaikki käynnissä olevat Linux AM työmäärää | Toissijainen VMware sivuston | Punainen on Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> Centos 6.5 6.6, 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, jossa punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Muistitilaa: tiedostojärjestelmän (EXT3, ETX4, ReiserFS, XFS); Monipolku ohjelmisto-laite Mapper (monipolku)). Levyn hallinta: (LVM2). HP CCISS ohjauskoneen tallennustilan fyysinen palvelinten ei tueta. ReiserFS tiedostojärjestelmän tuetaan vain SUSE Linux Enterprise Server 11 SP3.
**Fyysinen palvelimet** | Mikä tahansa työmäärää Windows-käyttöjärjestelmässä | Azure | 64-bittinen Windows Server 2012 R2, Windows Server 2012: ssa ja Windows Server 2008, jossa on vähintään SP1
**Fyysinen palvelimet** | Kaikki käynnissä olevat Linux työmäärää | Azure | Punainen on Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, jossa punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Muistitilaa: tiedostojärjestelmää (EXT3, ETX4, ReiserFS, XFS); Monipolku ohjelmisto-laite Mapper (monipolku)). Levyn hallinta: (LVM2). HP CCISS ohjauskoneen tallennustilan fyysinen palvelinten ei tueta. ReiserFS tiedostojärjestelmän tuetaan vain SUSE Linux Enterprise Server 11 SP3.
**Fyysinen palvelimet** | Mikä tahansa työmäärää Windows-käyttöjärjestelmässä | Toissijainen | 64-bittinen Windows Server 2012 R2, Windows Server 2012: ssa ja Windows Server 2008, jossa on vähintään SP1
**Fyysinen palvelimet** | Kaikki käynnissä olevat Linux työmäärää | Toissijainen | Punainen on Enterprise Linux 6.7,7.1,7.2 <br/><br/> Centos 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, jossa punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Muistitilaa: tiedostojärjestelmän (EXT3, ETX4, ReiserFS, XFS); Monipolku ohjelmisto-laite Mapper (monipolku)). Levyn hallinta: (LVM2). HP CCISS ohjauskoneen tallennustilan fyysinen palvelinten ei tueta. ReiserFS tiedostojärjestelmän tuetaan vain SUSE Linux Enterprise Server 11 SP3.


## <a name="provider-versions"></a>Toimittaja-versiot

**Nimi** | **Kuvaus** | **Uusin versio** | **Tuki** | **Tiedot**
---|---|---|---| ---
**Azure sivuston palauttaminen tarjoajan** | Paikallisten palvelimien ja Azure/toissijainen Sivuston välisten yhteyksien koordinaatit <br/><br/> Asennettuihin paikallisen VMM palvelimia tai Hyper-V Jos palvelinta ei ole VMM | 5.1.1700 (käytettävissä-portaalista) | [Uusimmat ominaisuudet ja ratkaisut](https://support.microsoft.com/kb/3155002)
**Azure palauttaminen yhdistetyn asetukset (VMware Azure)** | Paikallisten VMware palvelimien ja Azure välisten yhteyksien koordinaatit <br/><br/> Asennetaan paikallisen VMware palvelimiin | 9.3.4246.1 (käytettävissä-portaalista) | [Uusimmat ominaisuudet ja ratkaisut](https://support.microsoft.com/kb/3155002)
**Mobility-palvelu** | Koordinaattien replikoinnin paikallisten VMware palvelinten/fyysisiä palvelimien ja Azure/toissijainen Sivuston | NA (käytettävissä-portaalista) | Asennuksen kunkin VMware AM tai fyysinen palvelin, johon haluat kopioida. **Microsoft Azure palautus Services (MARS)-agentti** | Koordinaattien replikoinnin Hyper-V VMs ja Azure välillä<br/><br/> Asennetaan paikallisen Hyper-V palvelimiin (kanssa tai ilman VMM-palvelin) | 2.0.8689.0 (käytettävissä-portaalista) | Tämä agentti käyttävät Azure palauttaminen ja Azure varmuuskopiointi). [Lisätietoja] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Seuraavat vaiheet

[Valmistele käyttöönottoa varten](site-recovery-best-practices.md)

