<properties 
    pageTitle="Voit lisätä koodauksen yksiköt" 
    description="Lue, miten voit lisätä koodauksen yksiköt, jonka .NET"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Miten .NET SDK-koodaus

> [AZURE.SELECTOR]
- [Portal](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [MUILLE KÄYTTÄJILLE](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Yleiskatsaus

>[AZURE.IMPORTANT] Varmista, että tarkistettava [ohjeaiheesta saat lisätietoja skaalaus media käsittelyn aihe](media-services-scale-media-processing-overview.md) .
 
Jos haluat muuttaa varattu yksikkötyyppi ja koodaus käyttämällä .NET SDK varattu yksiköiden määrän, toimi seuraavasti:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Tuen lippu avaaminen

Oletusarvoisesti jokaisella Media Services-tilin voi skaalata enintään 25 koodaus ja 5 tarvittaessa Streaming varatut resurssit. Voit pyytää avaamalla tuki lippu korkeamman raja-arvon.

###<a name="open-a-support-ticket"></a>Avaa tuki-lippu

Avaa tuki lipun seuraavasti:

1. Valitse [tuesta](https://manage.windowsazure.com/?getsupport=true). Jos et ole kirjautunut, voit pyytää Anna tunnistetiedot.

1. Valitse tilauksen.

1. Valitse tuki tyyppi-kohdassa "Tekniset".

1. Napsauta "Luo lippu".

1. Valitse "Azure Media-palvelut" tuoteluettelossa seuraavalla sivulla.

1. Valitse "ongelmatyyppi", joka sopii ongelmaa.

1. Valitse Jatka.

1. Ohjeiden seuraavalla sivulla ja kirjoita sitten tietojen ongelmaa.

1. Valitse Lähetä Avaa lippu.



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
