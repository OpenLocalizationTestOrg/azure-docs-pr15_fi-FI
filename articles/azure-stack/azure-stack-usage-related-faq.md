<properties
    pageTitle="Käyttö liittyvät kysymykset ja vastaukset | Microsoft Azure"
    description="Azure-pino metriä, vertailu Azure käyttö API, käyttö ja raportoitu ajankohdan virhekoodit luettelo."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="azure-stack-usage-api-faqs"></a>Azure pinon käyttö API usein kysyttyjä kysymyksiä
Tässä artikkelissa vastataan Azure pinon käyttö Ohjelmointirajapinnan joitakin usein kysyttyihin kysymyksiin.

## <a name="what-meter-ids-can-i-see"></a>Mitä mittarin tunnukset voi tarkastella?

Käyttö on tällä hetkellä raportoitu verkon, varasto ja Laske resurssin tarjoajan palveluun.

| **Resurssin tarjoajaan** | **Mittarin tunnus** |**Mittarin nimi** | **Yksikkö** | **Muut tiedot** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Verkon** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Julkiseen IP-osoite käyttö | IP-osoite |                    
| **Tallennustilan**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GT\*tuntia | Taulukoiden käyttämä kokonaiskapasiteetti |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GT\*tuntia | Kokonaiskapasiteetti käyttämä sivun BLOB-objektit |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GT\*tuntia  | Kokonaiskapasiteetti kulutettu jono |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GT\*tuntia  | Estä BLOB käyttämä kokonaiskapasiteetti |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Pyyntö 10,000s lukumäärä   | Taulukon palvelupyyntöjä (-10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Tunkeutumisen tietojen gt | Taulukon palvelun tiedot tunkeutumisen gt: |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | GT Outgress | Taulukon palvelun tiedot juniin gt: |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Pyynnöt laskeminen 10,000s | Blob-objektien palvelupyyntöjä (-10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Tunkeutumisen tietojen gt          | BLOB-palvelun tiedot tunkeutumisen gt: 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | GT Outgress              | BLOB-palvelun tiedot juniin gt: 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Pyynnöt laskeminen 10,000s   | Jonon palvelupyyntöjä (-10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Tunkeutumisen tietojen gt         | Jonon palvelun tiedot tunkeutumisen gt: 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | GT Outgress  | Jonon palvelun tiedot juniin gt: 
| **Laske** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | AM koon tunnit | Virtuaalikoneen kokoa |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Miten Azure pinon käyttö ohjelmointirajapinnan poikkeaa [Azure käyttö API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (parhaillaan julkinen esikatselu)?

-   Vuokraajan käyttö Ohjelmointirajapinnan vastaa Azure-Ohjelmointirajapinnan yhtä poikkeusta lukuun ottamatta: *showDetails* merkintä tällä hetkellä ei tueta Azure Pinotut.

-   Palvelun käyttö Ohjelmointirajapinnan koskee ainoastaan Azure pino.

-   Tällä hetkellä käytettävissä oleva Azure [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) ei ole käytettävissä Azure Pinotut.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Mitä eroa käyttö- ja raportoida aikaa?

Tietoja käyttöraportit on kaksi tärkeimmät aika-arvot:

-   **Raportoidun ajan**. Kun käyttö tapahtuman antamasi käyttö järjestelmän aika

-   **Käyttöaika**. Kun Azure pinon resurssi on käytetty aika

Näyttöön saattaa tulla arvot poikkeavat tietyn käyttö tapahtuman käyttöaika ja raportoida ajan. Viive voi kestää useita tunteina missä tahansa ympäristössä.

Voit suorittaa tällä hetkellä *vain raportoida ajan mukaan*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Mitä käyttö API nämä virhekoodit tarkoittavat?

| **HTTP-tilakoodi** | **Virhekoodi** | **Kuvaus** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| Pyyntö/400 ei kelpaa        | *NoApiVersion*     | *Api-version* kyselyn parametri puuttuu.
| Pyyntö/400 ei kelpaa        | *InvalidProperty*  | Ominaisuus puuttuu tai on virheellinen arvo. Viestin vastaus tekstissä virhekoodi tunnistaa ominaisuus puuttuu.
| Pyyntö/400 ei kelpaa        | *RequestEndTimeIsInFuture*  | *ReportedEndTime* arvo tulevaisuudessa. Arvot myöhemmin, ei sallita tälle argumentille.
| Pyyntö/400 ei kelpaa        | *SubscriberIdIsNotDirectTenant*    | Palvelun API-kutsu käyttää Tilaustunnus, joka ei ole kelvollinen vuokraajan kutsujan.
| Pyyntö/400 ei kelpaa        | *SubscriptionIdMissingInRequest*   | Soittajan tunnus tilauksen puuttuu.
| Pyyntö/400 ei kelpaa        | *InvalidAggregationGranularity*   | Virheellinen kooste rakeisuuden pyydettiin. Kelvolliset arvot ovat päivittäin ja tunnin välein.
| 503                    | *ServiceUnavailable*   | Uudelleenyritettävä virhe sillä palvelu on varattu tai puhelun pakotetaan. |

## <a name="next-steps"></a>Seuraavat vaiheet
[Asiakas, Laskutus- ja Azure Pinotut palautus](azure-stack-billing-and-chargeback.md)

[Palvelun resurssien käyttö Ohjelmointirajapinta](azure-stack-provider-resource-api.md)

[Vuokraaja resurssien käyttö Ohjelmointirajapinta](azure-stack-tenant-resource-usage-api.md)
