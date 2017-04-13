<properties
   pageTitle="Luotettavan palveluiden ilmoitukset | Microsoft Azure"
   description="Käsitteellinen ohjeissa palvelun kangasta luotettava palveluiden ilmoitukset"
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="reliable-services-notifications"></a>Luotettavan palveluiden ilmoitukset

Ilmoitusten Salli asiakkaat voivat jäljittää muutokset, jotka tehdään objektiin, jossa ne ovat kiinnostuneita. Objektien kahdentyyppisiä tukevat ilmoitukset: *Luotettava tilan hallinta* ja *Luotettava sanasto*.

Tilanteet, joissa käytetään ilmoituksia ovat seuraavat:

- Rakennuksen sattui näkymiä, kuten toissijaisia indeksejä tai yhdistetään sen valtion suodatetuissa näkymissä. Esimerkki on lajiteltu indeksi kaikki avaimet luotettava sanaston.
- Lähettäminen seurantatiedot, kuten lisätty edellisen tunnin käyttäjämäärä.

Ilmoitusten tuodaan osana toimintojen käyttämisen. Tämän vuoksi ilmoituksia käsitellään talteen mahdollista ja painikkeen tapahtumien ei kannata lisätä kallista toiminnot.

## <a name="reliable-state-manager-notifications"></a>Luotettava tilan hallinta-ilmoitukset

Luotettava tilan hallinta tarjoaa ilmoituksia seuraavista tilanteista:

- Tapahtuman
    - Vahvista
- Tilan hallinta
    - Muodosta uudelleen
    - Luotettavan valtion lisäys
    - Luotettavan tilan poistaminen

Luotettava tilan hallinta seuraa nykyisen inflight tapahtumat. Toimintotila, joka aiheuttaa ilmoituksen voi käyttää vain muutos on tapahtuma on vahvistettu.

Luotettava tilan hallinta ylläpitää luotettava hyötyä kuten luotettava sanasto ja luotettava jono kokoelma. Luotettava tilan hallinta käynnistymiseen ilmoitukset tämän sivustokokoelman muuttuessa: luotettava tilan lisätään tai poistetaan tai koko kokoelman muodostetaan.
Luotettavan tilan hallinta-sivustokokoelman muodostetaan kolme tapauksissa:

- Palautus: Kun replikan käynnistyy, se palauttaa edelliseen tilaan levyltä. Palautus lopussa se käyttää **NotifyStateManagerChangedEventArgs** Kyselysäännön tapahtuma, joka sisältää palautetun luotettava tiloista.
- Täysi kopio: ennen replikan voivat liittyä kokoonpanoasetusten, siinä on oltava. Joskus tämä edellyttää kopio luotettava tilan esimiehen tila ensisijainen se käyttämättömänä toissijainen se käytettäväksi. Luotettava tilan hallinnassa toissijainen se käyttää **NotifyStateManagerChangedEventArgs** Kyselysäännön tapahtuman, joka sisältää luotettava tiloja, joita se saatu ensisijainen se.
- Palauta: Tietojen palauttaminen tilanteissa sen tilan voi palauttaa varmuuskopiosta **RestoreAsync**kautta. Tässä tapauksessa luotettava tilan hallinnassa ensisijainen se käyttää **NotifyStateManagerChangedEventArgs** Kyselysäännön tapahtuman, joka sisältää luotettava tiloja, joita se palauttaminen varmuuskopiosta.

Rekisteröidy ilmoitukset ja/tai tilan hallinta-ilmoituksia, haluat rekisteröidä **TransactionChanged** tai **StateManagerChanged** tapahtumat luotettava tilan hallinta. Yleisiä paikka rekisteröidä nämä tapahtuman käsittelytoimintoja on tilallisten palvelun konstruktoria. Kun olet rekisteröinyt konstruktoria, äänitallenteen ilmoitus, joka aiheuttaa muutoksen **IReliableStateManager**elinkaaren aikana.

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

**TransactionChanged** tapahtumakäsittelijä käyttää **NotifyTransactionChangedEventArgs** tapahtuman tiedot. Se sisältää action-ominaisuuden (esimerkiksi **NotifyTransactionChangedAction.Commit**), joka määrittää haluamaasi muutosta. Se sisältää myös transaction-ominaisuutta, joka sisältää viittauksen tapahtuman, jotka ovat muuttuneet.

>[AZURE.NOTE] Nykyään **TransactionChanged** tapahtumat ovat korotettuna vain, jos tapahtuma on vahvistettu. Toiminto on sitten **NotifyTransactionChangedAction.Commit**. Mutta jatkossa tapahtumien ehkä korotettuna muuntyyppisten tapahtuman tila muuttuu. On suositeltavaa tarkistaa toiminnon ja käsittelyn tapahtuman vain, jos se on yksi, jossa oletat.

Seuraavassa on esimerkki **TransactionChanged** tapahtumakäsittelijän.

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

**StateManagerChanged** tapahtumakäsittelijä käyttää **NotifyStateManagerChangedEventArgs** tapahtuman tiedot.
**NotifyStateManagerChangedEventArgs** on kaksi aliluokkien: **NotifyStateManagerRebuildEventArgs** ja **NotifyStateManagerSingleEntityChangedEventArgs**.
Action-ominaisuuden käyttäminen **NotifyStateManagerChangedEventArgs** , joka **NotifyStateManagerChangedEventArgs** oikea aliluokan:

- **NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**
- **NotifyStateManagerChangedAction.Add** ja **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**

Seuraavassa on esimerkki **StateManagerChanged** ilmoitus-käsittelijä.

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a>Luotettavan sanasto-ilmoitukset

Luotettavan sanaston tarjoaa ilmoituksia seuraavista tilanteista:

- Rakenna uudelleen: Kutsutaan, kun **ReliableDictionary** on palautettu tilaan palautetun tai kopioitu paikallinen tila tai varmuuskopiointi.
- Poista: Kutsutaan, kun **ReliableDictionary** tila on poistettu – **ClearAsync** -menetelmää.
- Lisää: Kutsutaan, kun kohde on lisätty **ReliableDictionary**.
- Päivitys: Nimi, kun kohteen **IReliableDictionary** on päivitetty.
- Poista: Kutsutaan, kun kohteen **IReliableDictionary** on poistettu.

Jos haluat luotettava sanaston ilmoitusten saaminen- **IReliableDictionary** **DictionaryChanged** tapahtumakäsittelijän rekisteröityä. Näiden tapahtumien käsittely rekisteröitymään yleisiä sijainti on **ReliableStateManager.StateManagerChanged** Lisää ilmoituksen.
Kun **IReliableDictionary** lisätään **IReliableStateManager** rekisteröiminen varmistaa, että kaikki ilmoitukset eivät jää huomaamatta.

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

>[AZURE.NOTE] **ProcessStateManagerSingleEntityNotification** on malli, joka **OnStateManagerChangedHandler** esimerkin kutsuu.

Yllä olevaa koodia asettaa **IReliableNotificationAsyncCallback** -liittymän sekä **DictionaryChanged**. Koska **NotifyDictionaryRebuildEventArgs** sisältää **IAsyncEnumerable** --käyttöliittymään on lueteltu asynkronisesti – Muodosta uudelleen ilmoitukset tuodaan kautta **RebuildNotificationAsyncCallback** **OnDictionaryChangedHandler**sijaan.

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

>[AZURE.NOTE] Yllä olevaa koodia, osana käsittelyn muodosta, ilmoitus-ensin ylläpidetään koostetun tila ole valittu. Koska luotettava sivustokokoelman muodostetaan tila on uusi, kaikki edellisen ilmoitukset eivät ole merkitystä.

**DictionaryChanged** tapahtumakäsittelijä käyttää **NotifyDictionaryChangedEventArgs** tapahtuman tiedot.
**NotifyDictionaryChangedEventArgs** on viisi aliluokkien. **NotifyDictionaryChangedEventArgs** toiminto-ominaisuuden avulla voit määrittää **NotifyDictionaryChangedEventArgs** oikea aliluokan:

- **NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**
- **NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**
- **NotifyDictionaryChangedAction.Add** ja **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**
- **NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**
- **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a>Suosituksia

- *Älä* tee ilmoitus tapahtumien mahdollisimman nopeasti.
- *Älä* Suorita kallista toiminnot (esimerkiksi i/o-toimintoja) osana synkronoidut tapahtumat.
- *Älä* Tarkista toiminnon tyyppi, ennen kuin voit käsitellä tapahtuma. Uusi tyypit mahdollisesti lisätään myöhemmin.

Seuraavat asiat kannattaa ottaa huomioon:

- Ilmoitusten tuodaan toiminnon suorittaminen osana. Esimerkiksi palauttaminen ilmoituksen käynnistyy palautustyön viimeisessä vaiheessa. Palautuksen ei loppuun, ennen kuin ilmoitus tapahtuma käsitellään.
- Koska ilmoitusten tuodaan kohdistaa toimintoja osana, asiakkaat näe vain paikallisesti vahvistettu toimintojen ilmoitukset. Ja koska toiminnot ovat taata vain paikallisesti vahvistettu (toisin sanoen lokiin), ne voi tai ei ehkä peruuttaa tulevaisuudessa.
- Tee polulla yhdellä ilmoituksella käynnistyy jokaisen käytetyn toiminnolle. Tämä tarkoittaa, että tapahtuman T1 sisältää Create(X), Delete(X) ja Create(X), pääset yhden ilmoituksen X, yksi poistamista varten ja toinen uudelleen luontia varten luontia varten tässä järjestyksessä.
- Tapahtumien, jotka sisältävät useita toimintoja toimintoja käytetään siinä järjestyksessä, jossa ne on vastaanotettu ensisijainen se käyttäjältä.
- Osana käsittelyn EPÄTOSI edistymisen jotkin toiminnot voivat voi kumota. Ilmoitukset ovat korotettuna tällaisten Kumoa-toimintoja, se tilan peruutetaan vakaata pisteeseen. Yksi tärkeä ero Kumoa ilmoitusten on tapahtumat, joissa on päällekkäisiä näppäimet yhdistetään. Jos tapahtuma T1 on peruutetaan, näet yksittäisen ilmoitus Delete(X).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Luotettavan sivustokokoelmat](service-fabric-work-with-reliable-collections.md)
- [Luotettavan palvelut-pikaopas](service-fabric-reliable-services-quick-start.md)
- [Luotettavan palveluiden varmuuskopiointi ja palauttaminen (palauttaminen)](service-fabric-reliable-services-backup-restore.md)
- [Luotettavan loppu Sovelluskehittäjän opas](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
