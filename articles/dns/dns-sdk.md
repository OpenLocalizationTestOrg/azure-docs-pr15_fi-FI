<properties 
   pageTitle="Luo DNS-vyöhykkeet ja tallentaa joukot Azure DNS käyttämällä .NET SDK | Microsoft Azure" 
   description="Azure DNS määrittää DNS-vyöhykkeet ja tietueen luominen käyttämällä .NET SDK." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/19/2016"
   ms.author="jtuliani"/>


# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a>Luo DNS-vyöhykkeet ja tietueen joukkoja .NET SDK

Voit automatisoida toimintojen luominen, poistaminen tai päivittää DNS-vyöhykkeet, tietueen joukot ja tietueet käyttämällä DNS SDK .NET DNS-hallinnan kirjaston kanssa. Koko Visual Studio-projekti on käytettävissä [tähän.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Tilin määrittäminen palvelulle pääasiallista

Yleensä ohjelmallisesti Azure resurssien käytettävyys kautta oman käyttäjätietoja sijaan oma tili. Erillinen tilit kutsutaan "tärkeimmät" palvelutilejä. Käyttämään Azure DNS SDK otoksen projektin, sinun on pääasiallista palvelutilin luominen ja sen oikeat käyttöoikeudet.

1. Noudattamalla [seuraavia ohjeita](../resource-group-authenticate-service-principal.md) tilin määrittäminen palvelulle principal (Azure DNS SDK otoksen projektin olettaa salasanan todennuksen.)

2. Luo resurssiryhmä ([näin miten](../azure-portal/resource-group-portal.md)).

3. Azure RBAC avulla voit myöntää pääasiallista palvelutilin resurssiryhmän 'DNS Zone osallistuja-oikeudet ([näin miten](../active-directory/role-based-access-control-configure.md).)

4. Jos Azure DNS SDK otoksen projektin, Muokkaa 'program.cs' tiedostoa seuraavasti:
    * Lisää oikeat arvot tenantId, clientId (tunnetaan myös nimellä Tilitunnus), salaisuus (service principal tilin salasana) ja subscriptionId, kuten vaiheessa 1.
    * Kirjoita vaiheessa 2 resurssin ryhmänimi.
    * Syötä valintasi DNS-vyöhyke nimi.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-paketit ja nimitilan ilmoitukset

Azure DNS .NET SDK käyttöön, sinun täytyy asentaa **Azure DNS kirjaston** NuGet paketin ja muut tarvittavat Azure paketit.
 
1. Avaa **Visual Studiossa**, projektin tai uusi projekti. 

2. Valitse **Työkalut** **>** **NuGet pakettien hallinta** **>** **NuGet pakettien... ratkaisun hallinta**. 

3. Valitse **Selaa**, ota käyttöön **Sisällytä prerelease** -valintaruutu ja kirjoita **Microsoft.Azure.Management.Dns** hakuruutuun.

4. Valitse paketti, ja valitse **Asenna** lisääminen Visual Studio projektin.
 
5. Toista edellä asentaa myös seuraavat paketit: **Microsoft.Rest.ClientRuntime.Azure.Authentication** ja **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Lisää nimitilan ilmoitukset

Lisää seuraavat nimitilan ilmoitukset

    using Microsoft.Rest.Azure.Authentication;
    using Microsoft.Azure.Management.Dns;
    using Microsoft.Azure.Management.Dns.Models;

## <a name="initialize-the-dns-management-client"></a>Alusta DNS-hallinta-asiakas

*DnsManagementClient* sisältää menetelmät ja ominaisuudet tarvittavat DNS-vyöhykkeet ja tietuejoukkoja hallintaa.  Seuraava koodi pääasiallista palvelutilin kirjautuu sisään ja luo DnsManagementClient objektin.

    // Build the service credentials and DNS management client
    var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
    var dnsClient = new DnsManagementClient(serviceCreds);
    dnsClient.SubscriptionId = subscriptionId;

## <a name="create-or-update-a-dns-zone"></a>Luo tai päivitä DNS-vyöhyke

Luo DNS-vyöhyke, ensin "Alue"-objekti on luotu sisältää DNS-vyöhyke parametrit. DNS-vyöhykkeet ei ole linkitetty tiettyyn alueeseen, koska sijainti on määritetty "Yleinen". Tässä esimerkissä [Azure Resurssienhallinta-tunniste'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) lisätään myös vyöhykkeen.

Todellisuudessa luominen tai päivittäminen Azure DNS-vyöhyke-vyöhyke-objektit, joiden vyöhykkeen parametrit siirretään *DnsManagementClient.Zones.CreateOrUpdateAsyc* -menetelmää.

>[AZURE.NOTE] DnsManagementClient tukee kolmessa toiminnon: synkronisen ("CreateOrUpdate"), asynkroninen ("CreateOrUpdateAsync"), tai asynkroninen käyttämiseen HTTP-vastaus ('CreateOrUpdateWithHttpMessagesAsync').  Voit valita jokin kolmesta tilasta sovelluksen tarpeidesi mukaan.

Azure DNS tukee Optimistinen samanaikainen, jota kutsutaan [Etags](dns-getstarted-create-dnszone.md). Tässä esimerkissä määrittäminen "*" "If-ei mitään-vastaavuuden" Otsikko kertoo Azure DNS Luo DNS-vyöhyke, jos sellaista ei ole jo.  Kutsu epäonnistuu, jos määritetyn niminen vyöhykkeen on jo annetun resurssiryhmä.

    // Create zone parameters
    var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"
    
    // Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
    dnsZoneParams.Tags = new Dictionary<string, string>();
    dnsZoneParams.Tags.Add("dept", "finance");
    
    // Create the actual zone.
    // Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
    // Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    // Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
    var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");

## <a name="create-dns-record-sets-and-records"></a>Luo DNS-tietueen joukot ja tietueet

DNS-tietueiden hallinnan tietuejoukon. Tietuejoukon on sama nimi ja tietuetyypin alueella tietuejoukon.  Tietuejoukon on suhteessa alueen nimi, ei ole täydellinen DNS-nimi.

Luo tai Päivitä tietue määritetty, "Tietuejoukon" parametrit-objekti on luotu ja *DnsManagementClient.RecordSets.CreateOrUpdateAsync*siirretään. DNS-vyöhykkeet, jossa ovat kolmessa toiminnon: synkronisen ("CreateOrUpdate"), asynkroninen ("CreateOrUpdateAsync"), tai asynkroninen käyttämiseen HTTP-vastaus ('CreateOrUpdateWithHttpMessagesAsync').

DNS-vyöhykkeet kanssa tietueen joukot toimenpiteet ovat optimistisen tuki.  Tässä esimerkissä jälkeen eikä If-löydä "If-ei mitään-vastine" ei määritetä, tietuejoukon aina luodaan.  Puhelun korvaa aiemmin luotu tietue määrittäminen samannimisen ja tietuetyypin DNS-vyöhykkeellä.

    // Create record set parameters
    var recordSetParams = new RecordSet();
    recordSetParams.TTL = 3600;

    // Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
    recordSetParams.ARecords = new List<ARecord>();
    recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

    // Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
    recordSetParams.Metadata = new Dictionary<string, string>();
    recordSetParams.Metadata.Add("user", "Mary");

    // Create the actual record set in Azure DNS
    // Note: no ETAG checks specified, will overwrite existing record set if one exists
    var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);

## <a name="get-zones-and-record-sets"></a>Hae vyöhykkeet ja tietueen joukot

*DnsManagementClient.Zones.Get* ja *DnsManagementClient.RecordSets.Get* menetelmiä hakea yksittäisen vyöhykkeet ja tietueen joukot. Tietuejoukkoja tunnistetaan tyypin, nimen ja aikavyöhyke- ja ryhmän ne. Vyöhykkeet tunnistetaan hänen nimensä ja ne resurssiryhmä.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
    
## <a name="update-an-existing-record-set"></a>Päivitä aiemmin luodun tietuejoukon

Päivitä aiemmin DNS-tietuejoukon, ensin noutaa tietuejoukon ja tee päivittää tietuejoukon sisällön, valitse Lähetä muutokset.  Tässä esimerkissä määritetään "Etag' haetut tietue-Jos-löydä parametrin joukosta. Kutsu epäonnistuu, jos samanaikainen toiminto on muokattu tällä välin tietuejoukon.

    var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

    // Add a new record to the local object.  Note that records in a record set must be unique/distinct
    recordSet.ARecords.Add(new ARecord("5.6.7.8"));

    // Update the record set in Azure DNS
    // Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
    recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);

## <a name="list-zones-and-record-sets"></a>Luettelon vyöhykkeet ja tietueen joukot

Luettelo alueita, *DnsManagementClient.Zones.List...* tavoilla, jotka tukevat joko kaikki alueet tietyn resurssiryhmä- tai kaikki alueet tietyn Azure tilauksen-luettelo (yli resurssiryhmiä.) Luettelo tietueen joukot, *DnsManagementClient.RecordSets.List...* tavoilla, jotka tukevat joko luettelon kaikki tietueen joukot tietyllä alueella tai vain ne tietueen määrä tietyntyyppinen.

Huomautus Kun luettelon vyöhykkeet ja tietueen joukot, jonka tuloksena on sivut numeroitu.  Seuraavassa esimerkissä esitetään, miten käydä läpi tulosten sivut. ("2" keinotekoisesti pieni sivukoon käytetään Pakota sivutus; käytännössä tämä parametri jätetään ja sivun oletuskoko.)

    // Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
    // In practice, to improve performance you would use a large page size or just use the system default
    int recordSets = 0;
    var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
    recordSets += page.Count();

    while (page.NextPageLink != null)
    {
        page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
        recordSets += page.Count();
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Lataa [Azure DNS .NET SDK otoksen projekti](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), joka sisältää muita esimerkkejä käyttämisestä Azure DNS .NET SDK-paketissa, mukaan lukien muiden DNS-tietuetyypit esimerkkejä.
