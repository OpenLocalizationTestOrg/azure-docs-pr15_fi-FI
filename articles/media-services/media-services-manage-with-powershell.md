<properties 
    pageTitle="Azure Media Services tilien PowerShellin hallinta" 
    description="Opi hallitsemaan Azure Media Services-tili ja PowerShellin cmdlet-komennot." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"
    ms.author="juliako"/>


#<a name="manage-azure-media-services-accounts-with-powershell"></a>Azure Media Services tilien PowerShellin hallinta

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShellin](media-services-manage-with-powershell.md)
- [MUILLE KÄYTTÄJILLE](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Voi luoda Azure Media Services-tilin, tarvitset Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A8A8397B5" target="_blank">Azure maksuttoman kokeiluversion</a>.

##<a name="overview"></a>Yleiskatsaus 

Tässä artikkelissa esitellään Azure PowerShellin cmdlet-komennot Azure Media Services (AMS) Azure Resurssienhallinta puitteissa. Cmdlet-komentoja ovat **Microsoft.Azure.Commands.Media** nimitilan.

## <a name="versions"></a>Versiot

**ApiVersion**: "2015-10-01"
               

## <a name="new-azurermmediaservice"></a>Uusi AzureRmMediaService

Luo media-palvelun.

### <a name="syntax"></a>Syntaksi

Parametrin määrittäminen: StorageAccountIdParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccountId] <string> [-Tags <hashtable>]  [<CommonParameters>]

Parametrin määrittäminen: StorageAccountsParamSet

    New-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Location] <string> [-StorageAccounts] <PSStorageAccount[]> [-Tags <hashtable>]  [<CommonParameters>]

### <a name="parameters"></a>Parametrit

**-ResourceGroupName &lt;merkkijono&gt;**

Määrittää nimen, johon kuuluu media palvelua resurssiryhmä.

Tunnukset | ei mitään
---|---
Pakollinen?   |  TOSI
Sijoita?   |  0
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä?  |EPÄTOSI

**-AccountName &lt;merkkijono&gt;**

Määrittää media-palvelun nimi.

Tunnukset |Nimi
---|---
Pakollinen? |TOSI
Sijoita? |1
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |EPÄTOSI
Hyväksy yleismerkkejä? |EPÄTOSI

**-Sijainti &lt;merkkijono&gt;**

Määrittää media-palvelun resurssin sijainti.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita? |2
Oletusarvo  |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-StorageAccountId &lt;merkkijono&gt;**

Määrittää ensisijainen tallennustilan-tili, jolla media-palveluun liitetyn.

- Tallennustilan luomaan uusi tili (Resurssienhallinta API) tukee vain.

- Tallennustilan tilin on oltava olemassa, ja se on samassa sijainnissa media-palvelussa.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita? |3
Oletusarvo  |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Parametrin nimi |StorageAccountIdParamSet
Hyväksy yleismerkkejä?|EPÄTOSI

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Määrittää tallennustilan tilit, jotka media-palveluun liitetyn.

- Tallennustilan luomaan uusi tili (Resurssienhallinta API) tukee vain.

- Tallennustilan tilin on oltava olemassa, ja se on samassa sijainnissa media-palvelussa.

- Vain yksi tallennustilan tili voidaan määrittää ensisijaisena.

Tunnukset |ei mitään
---|---
Pakollinen?  |TOSI
Sijoita?  |3
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Parametrin nimi |StorageAccountsParamSet
Hyväksy yleismerkkejä? |EPÄTOSI

**-Tunnisteet &lt;hajautustaulukko&gt;**

Määrittää tunnisteita, jotka liittyvät media-palvelun hash sisällysluettelon.

- Esimerkki:@{"tag1"="value1";"tag2"=:value2"}

Tunnukset |ei mitään
---|---
Pakollinen?  |EPÄTOSI
Sijoita?  |nimetty
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |EPÄTOSI
Hyväksy yleismerkkejä? |EPÄTOSI

**&lt;CommandParameters&gt;**

Tämä cmdlet-komento tukee yleisiä parametreja:-virheenkorjaus - ErrorAction - ErrorVariable - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - yksityiskohtainen, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Syötteiden

Syötteen tyyppi on objekteja, jotka olet pipe-cmdlet-komennolla voit tyyppi.

### <a name="outputs"></a>Tulostus

Tulosteen tyyppi on haluamasi objekteja, jotka tietokoneesta kuuluu äänimerkki-cmdlet-komennolla.

## <a name="set-azurermmediaservice"></a>Määritä AzureRmMediaService

Päivittää media-palvelun.

### <a name="syntax"></a>Syntaksi

    Set-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string> [-Tags <hashtable>] [-StorageAccounts <PSStorageAccount[]>]  [<CommonParameters>]

### <a name="parameters"></a>Parametrit

**-ResourceGroupName &lt;merkkijono&gt;**

Määrittää nimen, johon kuuluu media palvelua resurssiryhmä.

Tunnukset |ei mitään
---|---
Pakollinen?  |TOSI
Sijoita?  |0
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-AccountName &lt;merkkijono&gt;**

Määrittää media-palvelun nimi.

Tunnukset |Nimi
---|---
Pakollinen? |TOSI
Sijoita? |1
Oletusarvo |Ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-StorageAccounts &lt;PSStorageAccount\[\]&gt;**

Määrittää tallennustilan tilit, jotka media-palveluun liitetyn.

- Tallennustilan luomaan uusi tili (Resurssienhallinta API) tukee vain.

- Tallennustilan tilin on oltava olemassa, ja se on samassa sijainnissa media-palvelussa.

- Vain yksi tallennustilan tili voidaan määrittää ensisijaisena.

Tunnukset |ei mitään
---|---
Pakollinen? |EPÄTOSI
Sijoita? |Nimetty
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Parametrin nimi |StorageAccountsParamSet
Hyväksy yleismerkkejä? |EPÄTOSI

**-Tunnisteet &lt;hajautustaulukko&gt;**

Määrittää hash sisällysluettelon tunnisteita, jotka liittyvät media-palvelusta.

- Asiakkaan määritetty arvo korvataan tunnisteita, jotka liittyvät media-palvelussa.

Tunnukset |ei mitään
---|---
Pakollinen? |EPÄTOSI
Sijoita?  |Nimetty
Oletusarvo |Ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**&lt;CommandParameters&gt;**

Tämä cmdlet-komento tukee yleisiä parametreja:-virheenkorjaus - ErrorAction - ErrorVariable - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - yksityiskohtainen, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Syötteiden

Syötteen tyyppi on objekteja, jotka olet pipe-cmdlet-komennolla voit tyyppi.

### <a name="outputs"></a>Tulostus

Tulosteen tyyppi on haluamasi objekteja, jotka tietokoneesta kuuluu äänimerkki-cmdlet-komennolla.

## <a name="remove-azurermmediaservice"></a>Poista AzureRmMediaService

Poistaa media-palvelun.

### <a name="syntax"></a>Syntaksi

    Remove-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrit

**-ResourceGroupName &lt;merkkijono&gt;**

Määrittää nimen, johon kuuluu media palvelua resurssiryhmä.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita? |0
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-AccountName &lt;merkkijono&gt;**

Määrittää media-palvelun nimi.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita? |2
Oletusarvo |Ei mitään
Hyväksy myyntijakso syötteen?  |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**&lt;CommandParameters&gt;**

Tämä cmdlet-komento tukee yleisiä parametreja:-virheenkorjaus - ErrorAction - ErrorVariable - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - yksityiskohtainen, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Syötteiden

Syötteen tyyppi on objekteja, jotka olet pipe-cmdlet-komennolla voit tyyppi.

### <a name="outputs"></a>Tulostus

Tulosteen tyyppi on haluamasi objekteja, jotka tietokoneesta kuuluu äänimerkki-cmdlet-komennolla.

## <a name="get-azurermmediaservice"></a>Hae AzureRmMediaService

Hakee kaikki media-palvelujen resurssiryhmä tai mediapalvelu, jolla on annettu nimi.

### <a name="syntax"></a>Syntaksi

ParameterSet: ResourceGroupParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string>  [<CommonParameters>] 

ParameterSet: AccountNameParameterSet

    Get-AzureRmMediaService [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrit

**-ResourceGroupName &lt;merkkijono&gt;**

Määrittää nimen, johon kuuluu media palvelua resurssiryhmä.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita?  |0
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Parametrin nimi |ResourceGroupParameterSet, AccountNameParameterSet
Hyväksy yleismerkkejä?   EPÄTOSI

**-AccountName &lt;merkkijono&gt;**

Määrittää media-palvelun nimi.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita?  |1
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Parametrin nimi  |AccountNameParameterSet
Hyväksy yleismerkkejä? |EPÄTOSI

**&lt;CommandParameters&gt;**

Tämä cmdlet-komento tukee yleisiä parametreja:-virheenkorjaus - ErrorAction - ErrorVariable - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - yksityiskohtainen, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Syötteiden

Syötteen tyyppi on objekteja, jotka olet pipe-cmdlet-komennolla voit tyyppi.

### <a name="outputs"></a>Tulostus

Tulosteen tyyppi on haluamasi objekteja, jotka tietokoneesta kuuluu äänimerkki-cmdlet-komennolla.

## <a name="get-azurermmediaservicekeys"></a>Hae AzureRmMediaServiceKeys

Saa näppäimet media-palvelun.

### <a name="syntax"></a>Syntaksi

    Get-AzureRmMediaServiceKeys [-ResourceGroupName] <string> [-AccountName] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrit

**-ResourceGroupName &lt;merkkijono&gt;**

Määrittää nimen, johon kuuluu media palvelua resurssiryhmä.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita?  |0
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-AccountName &lt;merkkijono&gt;**

Määrittää media-palvelun nimi.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita? |1
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**&lt;CommandParameters&gt;**

Tämä cmdlet-komento tukee yleisiä parametreja:-virheenkorjaus - ErrorAction - ErrorVariable - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - yksityiskohtainen, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Syötteiden

Syötteen tyyppi on objekteja, jotka olet pipe-cmdlet-komennolla voit tyyppi.

### <a name="outputs"></a>Tulostus

Tulosteen tyyppi on haluamasi objekteja, jotka tietokoneesta kuuluu äänimerkki-cmdlet-komennolla.

## <a name="set-azurermmediaservicekey"></a>Määritä AzureRmMediaServiceKey

Luo media-palvelun ensisijaisen tai toissijaisen näppäintä uudelleen.

### <a name="syntax"></a>Syntaksi

    Set-AzureRmMediaServiceKey [-ResourceGroupName] <string> [-AccountName] <string> [-KeyType] <KeyType> {Primary | Secondary}  [<CommonParameters>]

### <a name="parameters"></a>Parametrit

**-ResourceGroupName &lt;merkkijono&gt;**

Määrittää nimen, johon kuuluu media palvelua resurssiryhmä.

Tunnukset |ei mitään
---|---
Pakollinen?  |TOSI
Sijoita?  |0
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen?  |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-AccountName &lt;merkkijono&gt;**

Määrittää media-palvelun nimi.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita?  |1
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen?   |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-KeyType &lt;KeyType&gt;**

Määrittää avaimen media-palvelun.

- Ensisijaisen tai toissijaisen

Tunnukset |ei mitään
---|---
Pakollinen?  |TOSI
Sijoita?  |2
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |EPÄTOSI
Hyväksy yleismerkkejä? |EPÄTOSI

**&lt;CommandParameters&gt;**

Tämä cmdlet-komento tukee yleisiä parametreja:-virheenkorjaus - ErrorAction - ErrorVariable - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - yksityiskohtainen, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Syötteiden

Syötteen tyyppi on objekteja, jotka olet pipe-cmdlet-komennolla voit tyyppi.

### <a name="outputs"></a>Tulostus

Tulosteen tyyppi on haluamasi objekteja, jotka tietokoneesta kuuluu äänimerkki-cmdlet-komennolla.

## <a name="sync-azurermmediaservicestoragekeys"></a>Synkronoi AzureRmMediaServiceStorageKeys

Synkronoi tallennustilan tilin näppäimet media-palveluun liitetyn tallennustilan tilin.

### <a name="syntax"></a>Syntaksi

    Sync-AzureRmMediaServiceStorageKeys [-ResourceGroupName] <string> [-MediaServiceAccountName] <string>    [-StorageAccountId] <string>  [<CommonParameters>]

### <a name="parameters"></a>Parametrit

**-ResourceGroupName &lt;merkkijono&gt;**

Määrittää nimen, johon kuuluu media palvelua resurssiryhmä.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita? |0
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-AccountName &lt;merkkijono&gt;**

Määrittää media-palvelun nimi.

Tunnukset |ei mitään
---|---
Pakollinen? |TOSI
Sijoita? |1
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**-StorageAccountId &lt;merkkijono&gt;**

Määrittää media-palveluun liitetyn tallennustilan tilin.

Tunnukset |Tunnus
---|---
Pakollinen? |TOSI
Sijoita?  |2
Oletusarvo |ei mitään
Hyväksy myyntijakso syötteen? |      TRUE(ByPropertyName)
Hyväksy yleismerkkejä? |EPÄTOSI

**&lt;CommandParameters&gt;**

Tämä cmdlet-komento tukee yleisiä parametreja:-virheenkorjaus - ErrorAction - ErrorVariable - InformationAction, - InformationVariable, - OutVariable,-OutBuffer, - PipelineVariable, - yksityiskohtainen, - WarningAction, ja - WarningVariable.

### <a name="inputs"></a>Syötteiden

Syötteen tyyppi on objekteja, jotka olet pipe-cmdlet-komennolla voit tyyppi.

### <a name="outputs"></a>Tulostus

Tulosteen tyyppi on haluamasi objekteja, jotka tietokoneesta kuuluu äänimerkki-cmdlet-komennolla.

## <a name="next-step"></a>Seuraava vaihe 

Tutustu oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
