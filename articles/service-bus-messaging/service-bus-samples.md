<properties 
    pageTitle="Palvelun Bus messaging esimerkit yleiskatsaus | Microsoft Azure"
    description="Luokittelee ja kuvataan palvelun Bus messaging linkit jokaiseen näytteiden."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>Palvelun Bus tekstiviesti-objektit

Palvelun Bus tekstiviesti mallit näytetään [palvelun Bus messaging](https://azure.microsoft.com/services/service-bus/) (pilvipalvelu) ja [Palvelun Bus Windows Server](https://msdn.microsoft.com/library/dn282144.aspx)tärkeimmistä ominaisuuksista. Tässä artikkelissa luokittelee ja vertaamalla linkit jokaiseen mallit.

>[AZURE.NOTE] Palvelun Bus mallit eivät asennu SDK: N kanssa. Voit hankkia mallit, käy [Azure SDK näytteiden sivun](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Lisäksi on päivitetty joukko palvelun Bus messaging näytteiden [tähän](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (vuodesta tämän virkkeet, ne ei tässä artikkelissa kuvattuja).  

Esimerkkejä välitys on [palvelun Bus välitys esimerkkejä](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Palvelun Bus viestintä

Seuraavat esimerkit osoittavat viestin kirjoittaminen palvelun Bus messaging käyttävistä sovelluksista.

Huomaa, että tekstiviesti mallit edellyttävät yhteysmerkkijonon käyttämään palvelua Bus nimitilan.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Voit hankkia yhteysmerkkijonoa Azure palvelun Bus

1. Kirjaudu [Azure portal](http://portal.azure.com).

1. Valitse vasemmanpuoleisessa sarakkeessa **Palvelun Bus**.

1. Napsauta oman nimitilan luettelon nimeä.

3. Valitse **jaettu käytäntöjen**nimitilan-sivu.

4. Valitse **RootManageSharedAccessKey** **jaettu käytäntöjen** -sivu.

6. Kopioi yhteysmerkkijono Leikepöydälle.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Voit hankkia yhteysmerkkijonon palvelun Bus Windows Server

1. Suorittaa seuraavan PowerShell cmdlet-komennon:

    ```
    get-sbClientConfiguration
    ```

2. Liitä yhteysmerkkijonon otosten App.config-tiedostoa.

### <a name="getting-started-samples"></a>Käytön aloittaminen-mallit

Näitä esimerkkejä kuvaavat tekstiviesti perustoiminnot.

|Esimerkki nimestä|Kuvaus|Pienin SDK-versio|Käytettävyys|
|---|---|---|---|
|[Aloittaminen: Viestintään olevien](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Esitellään, miten Microsoft Azure palvelun Bus avulla voit lähettää ja vastaanottaa viestejä jonossa.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Aloittaminen: Viestintään aiheita](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Esitellään, miten Microsoft Azure palvelun Bus avulla voit lähettää ja vastaanottaa viestejä aiheen, useita tilauksia.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|

### <a name="exploring-features"></a>Tutustuminen ominaisuudet

Seuraavat esimerkit esittelevät palvelun Bus eri ominaisuuksia.

|Esimerkki nimestä|Kuvaus|Pienin SDK-versio|Käytettävyys|
|---|---|---|---|
|[HTTP-tunnus-palveluntarjoajat](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Esitellään eri tavoista, joilla on todennustapa HTTP/REST-asiakkaan palvelun Bus kanssa.|2.1|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Palvelun Bus HTTP-asiakas](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Käsittelee viestien lähettäminen ja vastaanottaminen-palvelun Bus HTTP/REST kautta.|2.3|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Palvelun Bus Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Esitellään, miten voit automaattisesti lähettää viestejä jonossa, tilauksen tai perille toimittamattomien sanomien jonon toiseen jonossa tai aihe. Se myös esitellään, miten voit lähettää viestin jonossa tai aiheen siirto jonon kautta.|2.3|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: WCF kanavan istunnon malli](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Esitellään, miten voit käyttää Microsoft Azure-palvelu Bus Windows Communication Foundation (WCF) kanavien käyttäminen. Otosten näkyvät WCF kanava lähettää ja vastaanottaa viestejä palvelun Bus jonon kautta. Malli näkyy istunnon ja istunnon viestintä palvelun Bus päälle.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: tapahtumat](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Esitellään, miten käyttämään Microsoft Azure-palvelu Bus messaging ominaisuuksia tapahtuman alueella varmistamiseksi erissä messaging toiminnot on vahvistettu atomically.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Hallintatoiminnot käyttämällä muille käyttäjille](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Käsittelee Suorita hallinta palvelun Bus käyttämällä muille käyttäjille.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Resurssin tarjoajaan REST API](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Esitellään, miten uusi palvelu Bus RDFE REST API avulla voit hallita nimitilan ja tekstiviesti kohteet.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: WCF palvelun istunnon malli](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Esitellään, miten voit käyttää Microsoft Azure-palvelu Bus WCF-palvelun mallin. Otosten näkyvät suoritettava palvelun Bus jonon istunnon-viestintä WCF service-mallin.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Vastaus](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Esitellään, miten Microsoft Azure palvelun Bus ja pyynnön ja vastauksen-toimintoja. Malli näkyy yksinkertainen asiakkaiden ja palvelinten viestintä palvelun Bus jonon kautta.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: perille kirjain jonossa](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Esitellään, miten Microsoft Azure palvelun Bus ja tekstiviesti "perille kirjain jonon"-toimintoja. Esimerkissä yksinkertainen lähettäjä ja vastaanottaja viestintä palvelun Bus jonon kautta.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Myöhemmäksi viestit](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Esitellään, miten Microsoft Azure palvelun Bus viestin kannetuista-toiminnolla. Otosten näkyy yksinkertainen lähettäjän ja vastaanottajan viestintä palvelun Bus jonon kautta.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Istunnon viestien](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Esitellään, miten Microsoft Azure palvelun Bus ja Messaging istunnon toimintoja. Malli näkyy yksinkertainen lähettäjien ja vastaanottajien viestintä palvelun Bus jonon kautta.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Pyyntö aiheessa](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Esittelee ottamisesta käyttöön Microsoft Azure palvelun Bus aiheet ja tilaukset pyynnön ja vastauksen kuvio. Malli näkyy yksinkertainen asiakkaiden ja palvelinten viestintä palvelun Bus aiheen kautta.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Pyynnön vastaus jonossa](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Esitellään, miten Microsoft Azure palvelun Bus ja pyynnön ja vastauksen-toimintoja. Malli näkyy yksinkertainen asiakkaiden ja palvelinten viestintä kaksi palvelun Bus olevien kautta.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Kaksoiskappaleiden](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Esitellään, miten Microsoft Azure palvelun Bus kaksoiskappaleiden viestien tunnistaminen käytettäväksi olevien. Se luo kaksi olevien, joista kaksoiskappaleiden käytössä ja toiseen ilman kaksoiskappaleiden.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Asynkroninen viestintä](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Esitellään, miten Microsoft Azure palvelun Bus avulla voit lähettää ja vastaanottaa viestejä asynkronisesti jonossa. Jonossa sisältää erilliset, asynkroninen välisessä lähettäjän ja vastaanottajan haluamasi määrän (tässä kohdassa yksittäisen vastaanottajan).|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Erikoissuodattimet](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Esitellään, miten voit käyttää Microsoft Azure palvelun Bus Julkaise ja tilaa Erikoissuodattimet. Se luo aiheen ja 3 tilaukset eri suodatinta määritykset, lähettää viestejä aihetta ja niistä kaikki viestit tilaukset.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Se Messaging: Viestien esihaun](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Esitellään, miten Microsoft Azure palvelun Bus viestien esihaun-toiminnon käyttäminen. Esimerkissä yhteydessä vastaanota viestit esihaun-toiminnon käyttämisestä.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|

## <a name="service-bus-reference-tools"></a>Palvelun Bus viitetyökalut

Seuraavat esimerkit esittelevät eri palvelun muita ominaisuuksia.

|Esimerkki nimestä|Kuvaus|Pienin SDK-versio|Käytettävyys|
|---|---|---|---|
|[Palvelun Bus Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Palvelun Bus Resurssienhallinnan avulla käyttäjät voivat muodostaa yhteyttä palvelun Bus-palvelun nimitila ja hallita tekstiviesti kohteiden helposti tavalla. Työkalu sisältää lisäominaisuuksia, kuten Tuo/Vie toiminnot ja voi testata tekstiviesti yksiköt ja välitys palvelut.|1.8|Microsoft Azure-palvelu Bus; Windows Server Service Bus|
|[Todennus: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|Tässä esimerkissä voit luoda ja hallita Microsoft Azure Active Directory käyttöoikeuksien hallinta (tunnetaan myös nimellä Access Control-palvelu tai ACS)-palvelun käyttäjätietojen palvelun Bus käytettäväksi.|PUUTTUU|Microsoft Azure-palvelu Bus|

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavissa artikkeleissa saat palvelun Bus käsitteellinen yleiskatsauksen.

- [Palvelun Bus tekstiviesti yleiskatsaus](service-bus-messaging-overview.md)
- [Palvelun Bus-arkkitehtuuri](service-bus-architecture.md)
- [Palvelun Bus perusteet](service-bus-fundamentals-hybrid-solutions.md)
