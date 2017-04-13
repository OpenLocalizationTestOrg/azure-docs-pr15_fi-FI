<properties 
    pageTitle="Tapahtuman keskittimet messaging poikkeukset | Microsoft Azure"
    description="Azure tapahtuman keskittimet messaging poikkeukset ja ehdotetut toimenpiteet luettelo."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="event-hubs-messaging-exceptions"></a>Tapahtuman keskittimet messaging poikkeukset

Tässä artikkelissa on lueteltu joitakin poikkeukset Azure-palvelu Bus messaging API. Tämä API ehtojoukko sisältää tapahtuman keskittimet. Viittaus voi muuttua, joten Tarkista päivitykset.

## <a name="exception-categories"></a>Poikkeus-luokat

Tekstiviesti-ohjelmointirajapinnan Luo poikkeukset, jotka voivat kuuluvat seuraavaan luokkaan, sekä liittyvän toiminnon, voit yrittää korjata ne. Huomaa, että merkitys ja poikkeuksen syitä voivat vaihdella tekstiviesti kohteen (releitä, olevien/aiheiden tapahtuman keskittimet) tyypin mukaan:

1.  Käyttäjän coding virhe ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Yleiset toiminto: yrittää korjata koodi, ennen kuin jatkat.

2.  Asennus ja määritys-virheen ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingentitynotfoundexception.aspx), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Yleiset toiminto: Tarkista kokoonpanosi ja muuta tarvittaessa.

3.  Lyhytkestoisia poikkeukset ([Microsoft.ServiceBus.Messaging.MessagingException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx), [Microsoft.ServiceBus.Messaging.ServerBusyException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx) [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingcommunicationexception.aspx)). Yleiset toiminto: Yritä tai ilmoittaa käyttäjille.

4.  Muut poikkeukset ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagelocklostexception.aspx), [Microsoft.ServiceBus.Messaging.SessionLockLostException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sessionlocklostexception.aspx)). Yleiset toiminto: tietyn poikkeustyyppi; Lisätietoja on seuraavassa kohdassa taulukon. 

## <a name="exception-types"></a>Poikkeus tyypit

Seuraavassa taulukossa on lueteltu tekstiviesti poikkeuksen tyypit ja niiden syitä ja voit tehdä muistiinpanoja toimenpide.

| **Poikkeustyyppi**                                                                                                                                                                                                                                                                                | **Kuvaus/syy ja esimerkkejä**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | **Toimenpide-ehdotus**                                                                                                                                                                                                                                                                                                                                                                                                          | **Huomautus Automaattiset heti uudelleen**                                                                                             |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)                                                                                                                                                                                                           | Palvelin ei vastannut toimintoa määritetyn ajan, joka ohjaa [OperationTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx)kuluessa. Palvelin on suoritettu toimintoa. Tämä voi johtua verkko- tai muita infrastruktuuri viiveitä vuoksi.                                                                                                                                                                                                                                                                   | Järjestelmätilan yhdenmukaisuuden ja yritä uudelleen tarvittaessa.                                                                                                                                                                                                                                                                                                                                                            | Joissakin tapauksissa voi olla apua uudelleen Yritä logiikan lisääminen koodi.                                                                      |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx)                                                                                                                                                                                         | Pyydetty Käyttäjätoiminto ei ole sallittu sisällä palvelimeen tai palveluun. Katso lisätietoja Poikkeusviesti. Esimerkiksi [valmiina](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) luo tätä poikkeusta, jos viesti on vastaanotettu **ReceiveAndDelete** -tilassa.                                                                                                                                                                                                                                                                                                     | Tarkista koodi ja ohjeisiin. Varmista, että toimintoa on voimassa.                                                                                                                                                                                                                                                                                                                                         | Yritä ei avulla.                                                                                                          |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx)                                                                                                                                                                                       | Ohjelma yritetään käynnistää toiminnon objekti, joka on jo suljettu, on keskeytetty tai poistettu. Vain harvoin ympäröivä tapahtumaan on jo poistettu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Tarkista koodi ja varmista, että se ei ole ongelma toimenpiteet poistettua objektia.                                                                                                                                                                                                                                                                                                                                          | Yritä ei avulla.                                                                                                          |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx)                                                                                                                                                                                     | [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objektia ei voi hankkia tunnusta, tunnus on virheellinen tai tunnuksen ei sisällä saatavat toiminnon suorittaminen edellyttää.                                                                                                                                                                                                                                                                                                                                                                                                  | Varmista, että suojaustunnuksen toimittaja luodaan oikeat arvot. Tarkista käyttöoikeuksien valvonta-palvelun määritykset.                                                                                                                                                                                                                                                                                                   | Joissakin tapauksissa voi olla apua uudelleen Yritä logiikan lisääminen koodi.                                                                      |
| [ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) | Vähintään yksi menetelmän annetut argumentit ovat virheelliset. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) tai [Luo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.create.aspx) yhdistettävän URI sisältää polku mukaisten. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) tai [Luo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.create.aspx) yhdistettävän URI-malli on virheellinen. Ominaisuuden arvo on suurempi kuin 32 Kilotavua. | Tarkista puheluja koodi ja varmista, että argumentit ovat oikein.                                                                                                                                                                                                                                                                                                                                                           | Yritä ei avulla.                                                                                                          |
| [MessagingEntityNotFoundException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingentitynotfoundexception.aspx)                                                                                                                                             | Toiminnon liittyvää kohdetta ei ole olemassa tai se on poistettu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Varmista, että kohde on olemassa.                                                                                                                                                                                                                                                                                                                                                                                              | Yritä ei avulla.                                                                                                          |
| [MessageNotFoundException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagenotfoundexception.aspx)                                                                                                                                                             | Yrittää vastaanotat viestin, jonka tietyn järjestysnumero. Viestiä ei löydy.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Varmista, että viestiä ei ole jo vastaanotettu. Tarkista perille toimittamattomien sanomien jonon nähdäksesi, jos viesti on deadlettered.                                                                                                                                                                                                                                                                                              | Yritä ei avulla.                                                                                                          |
| [MessagingCommunicationException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingcommunicationexception.aspx)                                                                                                                                               | Asiakas ei pysty muodostamaan yhteyttä tapahtumaa-toiminnossa.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Varmista, että annettu isäntänimi on määritetty oikein ja isäntä on tavoitettavissa.                                                                                                                                                                                                                                                                                                                                                    | Yritä voi olla apua, jos varattuina ongelmia.                                                               |
| [ServerBusyException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx)                                                                                                                                                                       | Palvelu ei ole voivat käsitellä pyyntöä tällä hetkellä.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Asiakas voi Odota tietyn ajan ja sitten uudelleen.                                                                                                                                                                                                                                                                                                                                                           | Asiakas voi uudelleen tietyn ajanjakson jälkeen. Jos yritä tuloksena eri poikkeuksen, valitse kyseisen poikkeuksen uudelleen toimintaa. |
| [MessageLockLostException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagelocklostexception.aspx)                                                                                                                                                             | Lukitse tunnuksen liittyvä viesti on vanhentunut tai Lukitse-tunnuksen ei löydy.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Poistaa viestin.                                                                                                                                                                                                                                                                                                                                                                                                      | Yritä ei avulla.                                                                                                          |
| [SessionLockLostException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sessionlocklostexception.aspx)                                                                                                                                                             | Lukitse tässä istunnossa liittyvät menetetään.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Keskeytä [MessageSession](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.aspx) objekti.                                                                                                                                                                                                                                                                                           | Yritä ei avulla.                                                                                                          |
| [MessagingException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx)                                                                                                                                                                         | Yleinen messaging poikkeus, joka on antanut seuraavissa tapauksissa: yritetään suorittaa luominen [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) nimi ja polku, johon kuuluu eri kohteen tyyppi (esimerkiksi aihe). Yritetään suorittaa viestin lähettäminen suurempi kuin 256 Kilotavua. Palvelimeen tai palveluun havaitsi virheen käsiteltäessä pyynnön. Katso lisätietoja Poikkeusviesti. Tämä on yleensä lyhytkestoisia poikkeuksen.                                                                                                               | Tarkista koodi ja varmistaa, että vain voi sarjoittaa objekteja käytetään viestin tekstin (tai käytä mukautettu Sarjatoiminto). Ohjeissa tuettu arvo ominaisuudet ja vain käyttöä tueta tietotyypeistä. Tarkista [IsTransient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx) -ominaisuus. Jos se on **Tosi**, voit yrittää toiminnon uudelleen. | Yritä toiminta ei ole määritetty ja ei voi olla apua.                                                                               |
| [MessagingEntityAlreadyExistsException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingentityalreadyexistsexception.aspx)                                                                                                                                   | Yritä luoda kohteen nimi, joka on jo käytössä toisen yksikön kyseisen palvelun nimitilan.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Poista olemassa olevan kohteen tai valitse toinen nimi-kohteen luominen.                                                                                                                                                                                                                                                                                                                                       | Yritä ei avulla.                                                                                                          |
| [QuotaExceededException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx)                                                                                                                                                                 | Tekstiviesti kohteen sallitun enimmäiskoko on saavutettu. Tämä voi johtua vastaanottajia, kuinka monta (joka on 5) on jo avattu kuluttaja ryhmän tasolla.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Kohteen tai sen osa olevien viestien vastaanottaminen luoda kohteen tilaa.                                                                                                                                                                                                                                                                                                                                       | Yritä voi olla apua, jos viestit on poistettu sillä välin.                                                           
|
| [SessionCannotBeLockedException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sessioncannotbelockedexception.aspx)                                                                                                                                                 | Yritä Hyväksy istunnon tietyn istunnon tunnuksella, mutta istunto on lukittu toisen asiakkaan.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Varmista, että muut asiakkaat lukituksen istunto.                                                                                                                                                                                                                                                                                                                                                                       | Yritä voi olla apua, jos sillä välin on julkaistu istunto.                                                             |
| [TransactionSizeExceededException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transactionsizeexceededexception_methods.aspx)                                                                                                                                     | Liian monta toiminnot ovat osa tapahtumaa.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Vähennä osan muodostavia tapahtuma-toimintoa.                                                                                                                                                                                                                                                                                                                                                        | Yritä ei avulla.                                                                                                          |
| [MessagingEntityDisabledException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingentitydisabledexception.aspx)                                                                                                                                             | Pyyntö runtime-toiminnon käytöstä yksikön.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | Aktivoi kohteen.                                                                                                                                                                                                                                                                                                                                                                                                      | Yritä voi olla apua, jos kohde on aktivoitu välin.                                                             |
| [MessageSizeExceededException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesizeexceededexception.aspx)                                                                                                                                                     | Viestin paketti sallitut 256K. Huomaa, että 256 k rajoitus viestien kokonaiskokoa, jotka voivat sisältää järjestelmän ominaisuudet- ja minkä tahansa .NET katseltavan.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Viestin tiedot koon pienentäminen ja yritä uudelleen.                                                                                                                                                                                                                                                                                                                                                         | Yritä ei avulla.                                                                                                          |
| [TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx)                                                                                                                                                                                      | Ympäristön tapahtuman (*Transaction.Current*) on virheellinen. Se on ollut valmis tai keskeytetty. Sisemmän poikkeuksen voi antaa muita tietoja.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                           | Yritä ei avulla.                                                                                                          | -
| [TransactionInDoubtException](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx)                                                                                                                                                                        | Toiminto on liikaa tapahtuman, joka on aina varmempi ratkaisu tai yritetään suorittaa toteuttamaan tapahtuman ja tapahtuman muuttuu epävarma.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | Sovellus on käsiteltävä tätä poikkeusta (kuten määräten tapauksen), kun tapahtuma on jo saattanut vahvistettu.                                                                                                                                                                                                                                                                                                      | -                                                                                                                              |

## <a name="quotaexceededexception"></a>QuotaExceededException

[QuotaExceededException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx) ilmaisee, että tietyn kohteen kiintiö on ylitetty.

Tämä voi johtua vastaanottajia (5) enimmäismäärä on jo avattu kuluttaja ryhmän tasolla.

### <a name="event-hubs"></a>Tapahtuman keskittimet

Tapahtuman keskittimet 20 ryhmiä kohti tapahtumaa-toiminnossa enimmäismäärä on. Kun yrität luoda lisää, saat [QuotaExceededException](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.quotaexceededexception.aspx). 

## <a name="timeoutexception"></a>TimeoutException 

[TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) ilmaisee, että käyttäjän käynnistämä toiminto kestää kauemmin kuin toiminnon aikakatkaisu. 

### <a name="event-hubs"></a>Tapahtuman keskittimet

Tapahtuman keskittimien aikakatkaisun on määritetty joko osana yhteysmerkkijonon tai [ServiceBusConnectionStringBuilder](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.aspx)läpi. Virhesanoma itse saattavat vaihdella, mutta se on aina nykyisen toiminnon aikakatkaisuarvo. 

### <a name="common-causes"></a>Tavallisia syitä

Kaksi yleistä syytä tämän virheen: virheellisistä määrityksistä tai lyhytkestoisia palveluvirhe.

1. **Epäonnistuneen konfiguroinnin** 
    toiminnon aikakatkaisu voi olla liian pientä toiminnallisia ehto. Toiminnon aikakatkaisu SDK-asiakasohjelmassa oletusarvo on 60 sekuntia. Tarkista, onko koodisi arvon jotain pieni. Huomaa, että verkko- ja suorittimen käyttö kunto voivat vaikuttaa latausaikaan tietyn toiminnon suorittaminen edellyttää, jotta toiminnon aikakatkaisu ei voi määrittää vähän arvon.

2. **Lyhytkestoisia palveluvirhe** 
    joskus tapahtuman keskittimet palvelun kohdata viipeitä käsittely; esimerkiksi aikana suuri tietoliikenne. Tässä tapauksessa voit yrittää uudelleen toiminto viiveen jälkeen, kunnes toiminto onnistuu. Jos sama toiminto epäonnistuu useiden yritysten jälkeen, siirry [Azure palvelun tila-sivusto](https://azure.microsoft.com/status/) ole mitään tunnetut palvelukatkoksia.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso täydellinen palvelun Bus ja tapahtuman keskittimet .NET API viittaus, [Azure viitata MSDN-sivuston](https://msdn.microsoft.com/library/azure/mt419900.aspx).