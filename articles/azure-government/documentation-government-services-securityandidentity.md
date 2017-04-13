<properties
    pageTitle="Azure Government-ohjeet | Microsoft Azure"
    description="Tämä sisältää ominaisuuksia ja ohjeita vertailua Azure Government-sovellusten kehittäminen"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/12/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-security-and-identity"></a>Azure Government suojauksen ja tunnistetietojen

##  <a name="key-vault"></a>Avaimen säilö
Lisätietoja tämän palvelun ja sen käyttämisestä on artikkelissa <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure avaimen säilö julkisen ohjeissa.</a>

### <a name="data-considerations"></a>Tietojen huomioon otettavia seikkoja
Seuraavassa on esitetty Azure Government rajan Azure avaimen säilö:

| Sallittu säännelty ja hallita tiedot | Säännelty ja hallita tietojen ei sallita |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kaikki tiedot Azure avaimen säilö avaimella salattujen voivat sisältää Regulated ja hallita tietoja. | Azure avaimen säilö metatietojen ei ole sallittua sisältävät ohjaa vientitiedot. Metatiedot sisältää kaikki kokoonpanotietoja, kun luominen ja ylläpito avaimen säilö.  Älä kirjoita Regulated ja hallita tiedot seuraaviin kenttiin: resurssien ryhmien nimet, avaimen säilö nimiä, tilauksen nimi |

Avaimen säilö on yleensä käytettävissä Azure Government. Julkinen, kuten ei ilman laajennusta niin avaimen säilö on saatavana PowerShellistä ja CLI vain.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoa ja päivitykset tilaaminen <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-blogi.</a>
