<properties
    pageTitle="Hallitse Azure-palvelu Bus käyttämällä Azure automaatio | Microsoft Azure"
    description="Opettele hallinta Azure palvelun Bus Azure automaatio-palvelun avulla."
    services="service-bus, automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="magoedte;csand"/>

# <a name="managing-azure-service-bus-using-azure-automation"></a>Azure-palvelu Bus käyttämällä Azure automaatio hallinta

Opas esittelee voit Azure automaatio-palveluun ja kuinka sen avulla voidaan yksinkertaistaa Azure palvelun Bus hallintaa.

## <a name="what-is-azure-automation"></a>Azure automaatio-ominaisuudet

[Azure automaatio](../automation/automation-intro.md) on Azure-palvelu, yksinkertaistaminen cloud hallinnan automatisointi ja määritysten halutussa tilassa. Käyttämällä Azure Automaattiset, manuaaliset toistuva, pitkään suoritettavien ja virhe voi enää tehtävät voidaan automatisoida niin, että luotettavasti, tehokkuuden ja organisaation aika-arvo.

Azure automaatio on erittäin luotettavia, erittäin käytettävissä työnkulun suorittamisen-ohjelma, joka skaalaa vastaamaan omia tarpeita. Azure automaatio-prosesseja voi olla käynnistynyt manuaalisesti 3rd osapuolen järjestelmien tai tietyin väliajoin niin, että tehtävien tapahtua täsmälleen tarvittaessa.

Pienennä toiminnallisia katseltavan ja vapauttaa IT ja DevOps henkilökunnan keskittyminen työmäärän, joka lisää business arvon siirtämällä cloud hallinta tehtävät suoritetaan automaattisesti Azure automaatio mukaan.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Miten Azure automaatio avulla hallita Azure palvelun Bus?

Voit hallita palvelun Bus kanssa Azure Automation [Service Bus REST API](https://msdn.microsoft.com/library/azure/mt639375.aspx). Azure automaatio sisällä voit suorittaa PowerShell-komentosarjojen monia REST-Ohjelmointirajapinnan käyttäminen palvelun Bus tehtävien suorittamiseen. Voit myös pariliitos REST API puhelut Azure automaatio-Cmdlet-komentoja ja Azure muista palveluista, voit automatisoida monimutkaiset toimet Azure services ja 3 osapuolen järjestelmien välillä.

Seuraavassa on joitakin esimerkkejä Azure palvelun Bus hallinta PowerShellin avulla:

* [Voit hallita Azure palvelun Bus olevien mukautetun PowerShellin cmdlet-komennot](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [Palvelun Bus olevien, aiheet ja tilauksista PowerShell-komentosarjaa luominen](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Luo Azure-palvelu Bus nimitilan PowerShellin avulla](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

PowerShell-moduulin käsittelyyn Azure palvelun bus-automaatio runbooks voi ladata [PowerShell-valikoimassa](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0).


## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut Azure automaatio ja kuinka sen avulla voidaan hallita Azure palvelun Bus, näistä linkeistä saat lisätietoja Azure automaatio.

* Katso Azure automaatio- [opetusohjelman aloittaminen](../automation/automation-first-runbook-graphical.md)
* Katso hallinnasta [Palvelun Bus PowerShellin avulla](service-bus-powershell-how-to-provision.md)
