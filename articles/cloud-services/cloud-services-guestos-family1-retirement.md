<properties
   pageTitle="Asiakkaan Käyttöjärjestelmä perheen 1 käytöstä poistaminen Huomaa | Microsoft Azure"
   description="Tietoja siitä, milloin Azure Vieras OS tason 1 käytöstä poistaminen on tapahtunut ja tietoja sen selvittämisestä, jos näin käy"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Vieras OS tason 1 käytöstä poistaminen ilmoitus

OS tason 1 käytöstä poistaminen on ensin ilmoitettiin 1. kesäkuussa 2013.

**2014 2 syyskuuhun** Azure vieras-käyttöjärjestelmän (Vieras-Käyttöjärjestelmä) perheen 1.x, joka perustuu Windows Server 2008-käyttöjärjestelmää, virallisesti poistettu käytöstä. Kaikki yritykset asentaa uusia palveluja tai päivittää aiemmin-palveluita käyttämällä tason 1 epäonnistuu virheilmoituksen siitä, että Vieras OS tason 1 on jätetty pois.

**3. marraskuussa 2014** Extended-tukijakson piirissä Vieras OS tason 1 päättyi ja se on poistettu kokonaan. Kaikkien palveluiden edelleen tason 1 voi vaikuttaa. Microsoft saattaa lopettaa näistä palveluista milloin tahansa. Ei ei takaa, ellet päivitä manuaalisesti ne itsellesi suorittamista jatketaan palvelujen.

Jos sinulla on muita kysymyksiä, siirry [Cloud Services-keskustelupalstat](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) tai [Azure tuelta](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Näin käy?

Jos jokin seuraavista tilanteista koskee Cloud Services-palvelut:

1. Arvo on "osFamily ="1"nimenomaisesti määritetty ServiceConfiguration.cscfg tiedoston Cloud-palveluun.
2. Ei ole määritetty erikseen ServiceConfiguration.cscfg tiedoston pilvipalvelussa sijaitsevaan osFamily arvo. Tällä hetkellä järjestelmä käyttää oletusarvon "1" tässä tapauksessa.
3. Azure perinteinen portaalin Luetteloi vieras-käyttöjärjestelmän perheen arvo "Windows Server 2008: n".

Löydät, joka cloud Services-palvelut ovat käynnissä mitä OS perheen, voit suorittaa komentosarja alla PowerShellin Azure mutta ensin [PowerShellin Azure määrittäminen](../powershell-install-configure.md) . Saat lisätietoja komentosarja [Azure Vieras OS perheen 1 loppu on aika: kesäkuussa 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Cloud Services-palvelut heikentyä mukaan OS tason 1 käytöstä poistaminen komentosarjan tulosteen osFamily sarake on tyhjä tai sisältää "1".

## <a name="recommendations-if-you-are-affected"></a>Jos näin käy suositukset

On suositeltavaa pilvipalvelussa roolien siirtäminen jokin tuettu Vieras OS perheille:

**Vieras OS perheen 4.x** -Windows Server 2012 R2 *(suositus)*

1. Varmista, että sovelluksesi käyttää SDK 2.1 tai uudempi .NET Framework 4.0, 4.5 tai 4.5.1.
2. Määritä osFamily määritettä "4" ServiceConfiguration.cscfg tiedostossa ja ota yhteyttä pilvipalvelussa uudelleen.


**Vieras OS perheen 3.x** -Windows Server 2012: ssa

1. Varmista, että sovelluksesi käyttää SDK 1,8 tai uudempi .NET Framework 4.0 tai 4.5.
2. Määritä "3" osFamily-määritteen ServiceConfiguration.cscfg-tiedostossa ja ota yhteyttä pilvipalvelussa uudelleen.


**Vieras OS perheen 2.x** -Windows Server 2008 R2

1. Varmista, että sovelluksesi käyttää SDK 1.3 ja yllä .NET Framework 3.5 tai 4.0.
2. Määritä "2" osFamily-määritteen ServiceConfiguration.cscfg-tiedostossa ja ota yhteyttä pilvipalvelussa uudelleen.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Extended-tukijakson piirissä Vieras OS tason 1 päättyi 3. marraskuussa 2014
Cloud services Vieras OS tason 1 ei enää tueta. Siirtää tason 1 mahdollisimman pian voit välttää palvelu käytöstä.  

## <a name="next-steps"></a>Seuraavat vaiheet
Tarkista uusimmat [Vieras OS vapauttaa](cloud-services-guestos-update-matrix.md).
