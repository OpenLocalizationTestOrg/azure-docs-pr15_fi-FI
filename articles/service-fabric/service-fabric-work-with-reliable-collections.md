<properties
    pageTitle="Luotettavan Kokoelmien käyttäminen | Microsoft Azure"
    description="Tutustu parhaisiin käytäntöihin käsittelyyn luotettava sivustokokoelmat."
    services="service-fabric"
    documentationCenter=".net"
    authors="JeffreyRichter"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="03/28/2016"
    ms.author="jeffreyr" />

# <a name="working-with-reliable-collections"></a>Luotettavan Kokoelmien käyttäminen

Palvelun kangasta ehdottaa käytettävissä tilallisten ohjelmoinnin mallin .NET kehittäjät luotettava sivustokokoelmat kautta. Tarkemmin sanottuna palvelun kangasta on luotettava sanasto ja luotettava jonon luokat. Kun käytät kyseisten luokkien, osavaltio on osioitu (for skaalattavuus), replikoida (for saatavuuden) ja tapahtumallinen (for HAPPAMAN semantiikkaan liittyvien)-osioon. Oletetaan, että Tarkista tyypillinen käyttö luotettava dictionary-objekti ja mitä sen todella edistymisestä.

~~~
retry:
try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to  
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) { 
   await Task.Delay(100, cancellationToken); goto retry; 
}
~~~

Luotettavan sanasto-objektit (lukuun ottamatta ClearAsync joka ei voi kumota), kaikki toiminnot vaativat ITransaction objektin. Tämä objekti on liitetty jokin ja kaikki muutokset, kun yrität tehdä minkä tahansa luotettavaa sanastoon ja/tai luotettavaa jonon objektien osio. Muokkaaminen ITransaction objektin soittamalla osio on StateManager's CreateTransaction menetelmää.
 
Yllä olevan koodin ITransaction objektin siirretään luotettava sanaston AddAsync menetelmää. Sisäisesti sanaston menetelmiä, joka hyväksyy näppäintä kestää avaimeen reader/kirjoittajan lukituksen. Jos menetelmä muokkaa avaimen arvo-menetelmä tulee kirjoittaa lukko avaimen ja jos menetelmä lukee vain avaimen arvosta, valitse Lue lukko otetaan avaimen. Koska AddAsync Muokkaa uuden, välitetty-arvon arvo näppäintä, otetaan kirjoittaminen lock näppäintä. Niin, jos 2 (tai useamman) säikeen yritetään lisätä arvoja samalla avaimella samaan aikaan, yksi viestiketjun hankittava kirjoittaminen lukituksen ja estää muiden viestiketjuissa siirtyminen. Oletusarvon mukaan menetelmiä estää hankkia lukko; 4 sekunnin ajan neljän sekunnin jälkeen menetelmiä palauttaa TimeoutException. Menetelmä Osastollasi olemassa, jonka avulla voi välittää eksplisiittinen aikakatkaisuarvon, jos haluat mieluummin.
 
Yleensä voit kirjoittaa koodin vastata TimeoutException pyytämisestä sen ja yritetään koko toiminnon (katso yllä koodissa). Yksinkertainen koodin I 'M vain kutsumista Task.Delay kulkeva 100 millisekuntia aina, kun. Mutta todellisuudessa voi olla parempi käytöstä jokin lisätoimi eksponentiaalisen Edellinen käytöstä viiveen.
 
Kun lukitus on hankittu, AddAsync Lisää avaimen ja arvon objektin viittaukset sisäinen tilapäinen sanastoon ITransaction objektiin. Jos haluat antaa sinulle luku-your-omistaja-kirjoituksia semantiikkaan liittyvien tehdään. Kun olet soittanut AddAsync, myöhemmin kutsu TryGetValueAsync (joko ITransaction samaan objektiin) palauttaa arvon vaikka tapahtuman ei vielä ole sidottu. Seuraavaksi AddAsync serializes key-tuotetunnuksen ja arvon tavu matriisin objektien ja liittää näitä DBCS-matriisin paikallisen solmun lokitiedostoon. Lopuksi AddAsync lähettää DBCS-matriisin toissijainen replikoiden siten, että ne on samat tiedot avain/arvo. Vaikka avain/arvo-tiedot on kirjoitettu lokitiedostoon, tiedot pidetään sanaston osa ei, kunnes tapahtuma, johon ne on liitetty on vahvistettu. 

Yllä olevan koodin CommitAsync puhelu toteuttaa kaikki tapahtuman toiminnot. Tarkemmin sanottuna Vahvista tiedot liittää paikallisen solmun lokitiedosto ja lähettää myös Vahvista tietueen toissijainen replikoita. Kun replikat äänistä (suurimmalla) on vastannut, kaikki muutokset tiedostoilla pysyvä ja kaikki lukitukset liittyvät avaimet, jotka olivat toiminnat ITransaction objektin kautta julkaistaan niin muiden viestiketjuissa siirtyminen/tapahtumien voivat käsitellä samaa näppäimet ja niiden arvot.

Jos CommitAsync kutsutaan ei (yleensä vuoksi poikkeuksen käytössä), saa poistettu ITransaction-objekti. ITransaction ei ole-objektin myytäessä palvelun kangasta liittää Keskeytä tietoja paikallisen solmun lokitiedoston ja eritellä voidaan lähettää jollekin toissijainen replikoita. Ja valitse sitten kaikki lukitukset näppäimet, joka on toiminnat kautta tapahtumaan liittyvä julkaistaan.

## <a name="common-pitfalls-and-how-to-avoid-them"></a>Yleisiä käsitellään ja niiden välttämisestä
Kun ymmärtää, kuinka luotettava sivustokokoelmat toimivat sisäisesti, voit tarkastella joitakin yleisiä misuses ne. Katso seuraava koodi:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes, 
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
~~~

Kun käsittelet säännöllisesti .NET sanasto, voit lisätä sanastoon avain/arvo ja muuta ominaisuuden (esimerkiksi LastLogin). Kuitenkin koodi ei toimi oikein luotettava sanasto. Muista aiemman keskustelun AddAsync puhelun serializes tavu lukuihin avain/arvo-objektit ja tallentaa matriisin paikallisen tiedoston ja lähettää ne myös toissijaisen replikoita. Jos muutat myöhemmin ominaisuus, muutos ominaisuuden arvoa muistiin. se ei vaikuta paikallisen tiedoston tai replikat lähetettävät tiedot. Jos prosessin kaatuu, mikä on muistiin ilmenee poissa. Kun uusi prosessi käynnistyy tai toinen replika on ensisijainen, vanha ominaisuuden arvo on mikä on käytettävissä. 

Minulla ei kuormituksen tarpeeksi näin helppoa siihen on yllä virheen tyyppi. Ja vain opit virhettä Jos ja kun prosessi etenee. Oikean tavan kirjoittaa koodi on yksinkertaisesti Käännä kaksi riviä:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync(); 
}
~~~

Tässä on esimerkki toiseen Yleinen virhe:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync(); 
   }
}
~~~

Taas säännöllisesti .NET sanastojen yllä olevan koodin toimii moitteettomasti ja yleisiä kaavaa: kehittäjä arvon avaimen avulla. Jos arvo on olemassa, kehittäjä muuttuu ominaisuuden arvon. Kuitenkin kanssa luotettava sivustokokoelmat koodi poistettu käytöstä käytetään sama ongelma, kuten jo käsitellyt: __on muokattava objektin ei, kun olet antanut luotettava kokoelmaan.__
 
Oikean tavan päivittää luotettava sivustokokoelman arvon on olemassa olevaan arvoon viittaaminen ja harkitse pysyvä viittaus viittaa objekti. Luo sitten uusi objekti, joka on alkuperäisen objektin tarkan kopion. Voit nyt muokata tämän uuden objektin ja kirjoittaa uuden objektin kokoelmaan niin, että se saa muuntaa sarjaksi tavuinen lukuihin liitetään paikallinen tiedosto ja replikat lähetetään. Jälkeen vahvistetaan muutokset ladatun objektit, paikallinen tiedosto ja kaikki replikat on tarkka samaan tilaan. Kaikki on hyvä!

Seuraava koodi näkyy oikean tavan päivittää luotettava sivustokokoelman arvon:

~~~
using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser = 
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire 
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync(); 
   }
}
~~~

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a>Määritä pysyvä tietotyyppi, jotta ohjelmointi virhe

Ihannetapauksessa Otamme mielellämme kääntäjä virheistä, kun vahingossa tuottaa tunnus, jolla mutates objekti, joka on määritetty huomioitavia pysyvä tila. Mutta C#-kääntäjä ei ole mahdollisuus seuraavasti. Niin, vältä mahdolliset ohjelmointi virheet, on erittäin suositeltavaa, että määrität tyypit on pysyvä tyypit käyttämällä luotettava sivustokokoelmat. Tarkemmin sanottuna siis helppokäyttöisiä core arvon käyttämiseen (esimerkiksi lukujen Int32, UInt64, jne., päivämäärä ja aika, GUID-tunnus, aikajakson ja tykkää). Ja, voit myös käyttää merkkijono. Kannattaa välttää sivustokokoelman ominaisuudet kuin sarjoitettaessa ja poistettaessa niitä voidaan usein voi hidastaa järjestelmän toimintaa. Jos haluat käyttää sivustokokoelman ominaisuudet, on erittäin suositeltavaa käyttää. VERKON 's pysyvä sivustokokoelmat-kirjastossa (System.Collections.Immutable). Tämä kirjasto on ladattavissa http://nuget.org. Suosittelemme myös sinetöinnin oman luokat ja tehdä kentät vain luku-aina, kun se on mahdollista.

UserInfo laji esitellään, miten voit määrittää pysyvä tyyppi on edellä mainittujen suositusten hyödyntäminen.

~~~
[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;
 
   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }
 
   [DataMember]
   public readonly String Email;
 
   // Ideally, this would be a readonly field but it can't be because OnDeserialized 
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
~~~

ItemId tyyppi on myös pysyvä tyyppi, kuten kuvassa:

~~~
[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
~~~

## <a name="schema-versioning-upgrades"></a>Rakenteen versiotietojen (päivitykset)

Sisäisesti-luotettavia sivustokokoelmat muuntaa sarjaksi käyttämällä objektit. VERKON 's System.Runtime.Serialization. Sarjoitettu objektit säilyvät ensisijainen se käyttäjän paikalliseen levyasemaan ja siirretään myös toissijaisen replikoita. Kun palvelun kehittyy, todennäköisesti haluat muuttaa sen tyyppistä tietoa (rakenne) palvelu vaatii. Versiotietojen hyvin varovainen tietojen on oltava lähteä ratkomaan. Keskustelupalstoja sinun on aina oltava voi poistaa vanhoja tietoja. Tarkemmin sanottuna tämä tarkoittaa sarjoituksen-koodi on oltava infinitely yhteensopivia: versio 333 service-koodi on voitava muodostaa kirjoittamat tiedot luotettava sivustokokoelman palvelukoodin 1-version 5 vuotta sitten toimivat.

Lisäksi palvelukoodi on päivitetty päivityksen toimialueen kerrallaan. Päivityksen aikana sinulla on kaksi erilaista samanaikainen palvelun koodin. On Vältä palvelukoodin uuden version käyttää uutta mallia vanhat palvelun koodin välttämättä pysty käsittelemään uutta mallia. Jos mahdollista, kannattaa suunnitella jokaisessa versiossa on 1 versiolla eteenpäin yhteensopiva palvelu. Tarkemmin sanottuna tämä tarkoittaa, että palvelukoodin V1 pitäisi ohittaa kaikki rakenne-elementtien, se ei käsittele erikseen. Kuitenkin on oltava voi tallentaa kaikki tiedot, se ei erikseen tunnista ja riittää, että se takaisin, kun päivität sanaston avaimen tai arvon kirjoittaminen. 

>[AZURE.WARNING] Voit muokata näppäintä rakennetta, sinun on varmistettava, että avain hash- ja yhtä suuri kuin algoritmit ovat vakaata. Jos muutat jompikumpi näistä algoritmeista siitä, miten toimia, osaat ei sisällä luotettava sanaston avaimen hakemiseen niitä uudelleen.
  
Vaihtoehtoisesti voit suorittaa mitä yleensä kutsutaan 2 jakson päivitys. 2-vaiheessa päivitys, jossa olet päivittänyt palvelun V1 V2: V2 on koodi, joka tietää, kuinka voit käsitellä uuden rakenteen muuttaminen mutta koodi ei suorita. Kun V2 koodi lukee V1 tiedot, se toimii ja kirjoittaa V1 tietoja. Sitten, kun päivitys on valmis, kaikkien päivityksen toimialueilla, voit voi aiheuttaa signaalin käynnissä V2 esiintymissä päivitys on valmis. (Yksi tapa signaalin tämä on aloittamaan määritysten päivitys, tämä on minkälainen tämä 2 vaiheen päivityksen.) Nyt V2 esiintymät voit lukea V1 tietoja, V2 tietojen muuntaminen, käsitellä sitä ja kirjoittaa V2 tietoina. Kun muut esiintymät lukea V2 tietoja, niitä ei tarvitse muuntaa sen, ne vain käsitellä sitä, ja kirjoittaa V2 tiedot pois.

## <a name="next-steps"></a>Seuraavat vaiheet
Perustietoja eteenpäin yhteensopivat tietojen sopimuksia luomisesta on artikkelissa [Yhteensopivia tietojen sopimuksia](https://msdn.microsoft.com/library/ms731083.aspx).

Lisätietoja versiotietojen tietojen sopimuksia parhaista käytännöistä on artikkelissa [Tietojen sopimuksen versiotietojen](https://msdn.microsoft.com/library/ms731138.aspx). 

Lisätietoja version varmatoimisempia tietojen sopimuksia ottamisesta käyttöön on artikkelissa [Versio varmatoimisempia Sarjatoiminto takaisinkutsuja](https://msdn.microsoft.com/library/ms733734.aspx). 

Lisätietoja tietojen rakenteen, josta voit yhteiskäyttö eri versioissa on artikkelissa [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
