<properties
    pageTitle="Hallitse Azure avaimen säilö käyttämällä Azure automaatio | Microsoft Azure"
    description="Lue lisätietoja siitä, miten Azure automaatio-palvelun avulla voidaan hallita Azure avaimen säilö."
    services="Key-Vault, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

#<a name="managing-azure-key-vault-using-azure-automation"></a>Azure avaimen säilö käyttämällä Azure automaatio hallinta

Opas esittelee voit Azure automaatio-palveluun ja kuinka sen avulla voidaan yksinkertaistaa hallintaa näppäimet ja Azure avain säilöön tietoja.

## <a name="what-is-azure-automation"></a>Azure automaatio-ominaisuudet

[Azure automaatio](../automation/automation-intro.md) on Azure-palvelu, yksinkertaistaminen cloud hallinnan automatisointi ja määritysten halutussa tilassa. Käyttämällä Azure Automaattiset, manuaaliset toistuva, pitkään suoritettavien ja virhe voi enää tehtävät voidaan automatisoida niin, että luotettavasti, tehokkuuden ja organisaation aika-arvo.

Azure automaatio on erittäin luotettavia, erittäin käytettävissä työnkulun suorittamisen-ohjelma, joka skaalaa vastaamaan omia tarpeita. Azure automaatio-prosesseja voi olla käynnistynyt manuaalisesti 3rd osapuolen järjestelmien tai tietyin väliajoin niin, että tehtävien tapahtua täsmälleen tarvittaessa.

Pienennä toiminnallisia katseltavan ja vapauttaa IT ja DevOps henkilökunnan keskittyminen työmäärän, joka lisää business arvon siirtämällä cloud hallinta tehtävät suoritetaan automaattisesti Azure automaatio mukaan.


## <a name="how-can-azure-automation-help-manage-azure-key-vault"></a>Miten Azure automaatio avulla hallita Azure avaimen säilö?

Avaimen säilö voi hallita Azure automaatio-[AzureRM avaimen säilö cmdlet] (https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) ja [Azure perinteinen avaimen säilö cmdlet-komennot](https://msdn.microsoft.com/library/azure/dn868052.aspx). Azure hallinta perinteinen avain säilöön-moduuli on automaattisesti Azure automaatio käytettävissä ja voit tuoda [AzureRM KeyVault moduulin](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) Azure automaatio niin, että voit suorittaa useita palvelun avaimen säilö oman hallintatehtäviä. Voit myös pariliitos näiden cmdlet-komennoista Azure automaatio Cmdlet-komentoja Azure muista palveluista, voit automatisoida monimutkaiset toimet Azure services ja 3 osapuolen järjestelmien kanssa.

Voit tehdä muun muassa seuraavat tehtävät suorittamalla Azure avaimen säilö cmdlet-komennot: 

- Luo ja määritä avaimen säilö
- Luo tai tuo avain
- Luo tai päivitä salaisuus
- Päivitä avaimen määritteet
- Hae avaimen tai salaisuus
- Salainen tai avaimen poistaminen

Seuraavassa on joitakin esimerkkejä avaimen säilö hallinta PowerShellin avulla:  

* [Azure avaimen säilöön - Step by Step](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step)
* [Ja määrittämisestä Azure avaimen säilö](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault)


## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut Azure automaatio ja kuinka sen avulla voidaan hallita Azure avaimen säilö, näistä linkeistä saat lisätietoja Azure automaatio.

* Katso Azure automaatio- [opetusohjelman aloittaminen](../automation/automation-first-runbook-graphical.md).
* Katso [Azure avaimen säilö PowerShell-komentosarjojen](https://gallery.technet.microsoft.com/scriptcenter/site/search?query=azure%20key%20vault&f%5B0%5D.Value=azure%20key%20vault&f%5B0%5D.Type=SearchText&ac=5).
