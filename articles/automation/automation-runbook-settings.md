<properties 
   pageTitle="Runbookin asetukset"
   description="Tässä artikkelissa kuvataan runbookin Azure automaatio ja niiden avulla sekä Windows PowerShellin Azure hallinta-portaalin muuttamisesta asetukset."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />

# <a name="runbook-settings"></a>Runbookin asetukset

Azure automaatio kunkin runbookin on useita asetuksia, jotka auttavat tunnistauduttava ja muuta sen kirjaaminen-asetuksia. Jokainen näistä asetuksista on jäljempänä seuraa ohjeita siitä, miten voit muokata niitä.

## <a name="settings"></a>Asetukset

### <a name="name-and-description"></a>Nimi ja kuvaus

Et voi muuttaa nimi, runbookin, kun se on luotu. Kuvaus on valinnainen, ja se voi olla enintään 512 merkkiä.

### <a name="tags"></a>Tunnisteet

Tunnisteiden avulla voit määrittää tietyt sanat ja ilmaisut runbookin tunnistaminen. Esimerkiksi runbookin [Runbookin valikoima](https://msdn.microsoft.com/library/dn781422.aspx)lähettäessäsi määrittämäsi tietyn tunnisteet tunnistavan: n runbookin luetellut luokat. Voit määrittää useita tunnisteita runbookin erottamalla ne toisistaan pilkulla.

### <a name="logging"></a>Lokiin kirjaaminen

Oletusarvon mukaan yksityiskohtainen ja edistymisen tietueita ei ole kirjoitettu Työhistoria. Voit muuttaa tietyn runbookin asetusten kirjautua nämä tietueet. Saat lisätietoja näiden tietueiden [Runbookin tulostus ja viestit](https://msdn.microsoft.com/library/dn879148.aspx).

## <a name="changing-runbook-settings"></a>Runbookin asetusten muuttaminen

### <a name="changing-runbook-settings-with-the-azure-management-portal"></a>Asetusten muuttaminen runbookin Azure hallinta-portaalissa

Voit muuttaa asetuksia runbookin Azure hallinta-portaalissa: n runbookin **määrittäminen** -sivulla.

1. Azure hallinta-portaalissa Valitse **Automaattiset** ja valitse sitten automaatio-tilin nimi.
1. Valitse **Runbooks** -välilehti.
1. Valitse nimi, runbookin.
1. Valitse **määritys** -välilehti.

### <a name="changing-runbook-settings-with-windows-powershell"></a>Windows PowerShellin runbookin asetusten muuttaminen

[Määritä AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690275.aspx) cmdlet-komennon avulla voit runbookin asetusten muuttaminen. Jos haluat määrittää useita tunnisteita, voit joko antaa matriisi tai yhden merkkijonon tunnisteet-parametrin luetteloerotin arvoilla. Saat nykyiset tunnisteet [Get-AzureAutomationRunbook](https://msdn.microsoft.com/library/dn690278.aspx)kanssa.

Esimerkki seuraavista komennoista Näytä määrittämisestä runbookin ominaisuuksia. Tässä esimerkissä Lisää kolme tunnisteet olemassa olevia tunnisteita ja määrittää, että yksityiskohtainen tietueiden kirjautunut.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
- [Runbookin tulostus ja viestit](../automation-runbook-output-and-messages) 
- [Luominen tai Runbookin tuominen](https://msdn.microsoft.com/library/dn643637.aspx) 