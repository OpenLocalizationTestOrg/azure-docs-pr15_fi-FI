<properties 
    pageTitle="Palvelun Bus sovellusten vastaan katkokset ja katastrofit lämmittämistä | Microsoft Azure"
    description="Tässä artikkelissa kuvataan avulla voit suojata sovellusten mahdolliset palvelun Bus käyttökatkosta vastaan."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Parhaat käytännöt lämmittämistä sovellusten vastaan palvelun Bus katkokset ja katastrofit

Tärkeiden sovellusten on toimittava jatkuvasti, vaikka suunnittelematon katkokset tai lieventämiseksi kanssa. Tässä ohjeaiheessa kuvataan tapoja, joilla palvelun Bus sovellusten mahdolliset palvelun käyttökatkosta tai tietojen suojaaminen.

Käyttökatkosta määritellään Azure palvelun Bus tilapäinen siitä. Jotkin osat palvelun Bus, kuten tekstiviesti kaupan tai jopa koko palvelinkeskuksen voivat vaikuttaa siihen käyttökatkosta. Kun ongelma on korjattu, palvelun Bus on käytettävissä uudelleen. Yleensä käyttökatkosta aiheuta menetyksiä viestejä tai muita tietoja. Esimerkki osan virhe on tietyn tekstiviesti-säilön ei ole käytettävissä. Esimerkki palvelinkeskuksen koskevia käyttökatkosta on sen palvelinkeskuksen tai viallinen palvelinkeskuksen verkon valitsimen power epäonnistui. Käyttökatkosta voi kestää muutaman minuutin kuluttua-muutamassa päivässä.

Huono määritellään palvelun Bus asteikko tai palvelinkeskuksen menetetään pysyvästi. Sen palvelinkeskuksen saattaa tai saattaa ei ole käytettävissä uudelleen. Yleensä huono aiheuttaa joidenkin tai kaikkien viestejä tai muita tietoja menetetään. Esimerkkejä lieventämiseksi ovat fire samantyyppisten ja maanjäristyksen.

## <a name="current-architecture"></a>Nykyinen arkkitehtuuri

Palvelun Bus käyttää useita tekstiviesti stores olevien tai aiheet lähetetyt viestit. Osittamattoman jonossa tai aiheen määritetään yksi tekstiviesti-kaupasta. Jos tekstiviesti myymälän ei ole käytettävissä, kaikki toiminnot jonossa tai aiheen epäonnistuu.

Palvelun Bus tekstiviesti kohteilla (olevien, aiheet, releitä) sijaitsevat palvelun nimitila, joka liittyy palvelinkeskukseen. Palvelun Bus ei ota käyttöön automaattinen geo-replikoinnin tietojen eikä salli nimitila kattavat useita palvelinkeskusten.

## <a name="protecting-against-acs-outages"></a>ACS katkokset suojautuminen

Jos käytössäsi on ACS tunnistetiedot ja ACS ei ole käytettävissä, asiakkaat eivät enää hankkia tunnukset. Asiakkaat, joilla tunnusta ACS menee aikaa edelleen käyttää palvelun Bus paikkamerkkejä voimassaolo päättyy. Oletusarvoinen suojaustunnuksen elinaika on 3 tuntia.

Voit suojautua ACS katkokset käyttämällä jaettu Access allekirjoitus (SAS) tunnusten. Tässä tapauksessa asiakkaan todentaa suoraan palvelun Bus kanssa kirjautumalla itse minted tunnuksen salausavaimen kanssa. Puhelut ACS ei enää tarvita. Saat lisätietoja SAS tunnusten [palvelun Bus todennusta][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Olevien ja aiheet vastaan messaging kaupan virheet suojaaminen

Osittamattoman jonossa tai aiheen määritetään yksi tekstiviesti-kaupasta. Jos tekstiviesti myymälän ei ole käytettävissä, kaikki toiminnot jonossa tai aiheen epäonnistuu. Osioitu jonon koostuu toisaalta, useita osia. Kukin osa on tallennettu eri tekstiviesti-säilössä. Kun viesti on lähetetty osioitua jonossa tai aihe-palvelun Bus määrittää viestin johonkin osat. Vastaavat tekstiviesti kaupan ei ole käytettävissä, jos palvelun Bus kirjoittaa viestin eri osa, jos mahdollista. Saat lisätietoja osioitua kohteiden [osioitu tekstiviesti kohteita][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Palvelinkeskuksen katkokset tai lieventämiseksi suojautuminen

Sallimaan automaattisesti kahden palvelinkeskusten välillä, voit luoda palvelun Bus-palvelun nimitila kunkin palvelinkeskukseen. Esimerkiksi palvelun Bus palvelun nimitilan **contosoPrimary.servicebus.windows.net** voivat sijaita USA: Pohjois-Keski-alue ja **contosoSecondary.servicebus.windows.net** voivat sijaita US Etelä ja Keski-alue. Jos kohteen messaging Service-Bus on edelleen käytettävissä palvelinkeskuksen käyttökatkosta kanssa, voit luoda kyseisen kohteen sekä nimitilan.

Lisätietoja [asynkroninen tekstiviesti kuvioiden][]ja suuri käytettävyys-palvelun Bus sisällä Azure palvelinkeskuksen virhe"-osassa.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Välitys päätepisteet palvelinkeskuksen katkokset tai lieventämiseksi suojaaminen

Välitys päätepisteistä GEO-replikoinnin avulla palvelu, joka paljastaa välitys-päätepiste on tavoitettavissa, kun palvelua Bus katkokset. Palvelun on luotava välitys päätepisteet eri nimitilan geo replikoinnin saavuttamiseksi. Nimitilan on sijaittava eri palvelinkeskusten ja päätepisteet on oltava eri nimiä. Esimerkiksi ensisijainen päätepisteen tavoittaa kohdassa **contosoPrimary.servicebus.windows.net/myPrimaryService**samalla, kun toissijainen vastaavaan tavoittaa **contosoSecondary.servicebus.windows.net/mySecondaryService**-kohdassa.

Palvelun seuraa sitten molemmat päätepisteet ja asiakas voi käynnistää palvelun kautta joko päätepiste. Asiakassovellus satunnaisesti valitsee jonkin releitä kuin ensisijaisen päätepiste, ja lähettää sen pyynnön aktiivisen päätepisteen. Jos vieminen epäonnistuu, joka sisältää virhekoodin, tämän virheen ilmaisee, että välitys-päätepisteen ei ole käytettävissä. Avaa varmuuskopio päätepisteelle kanavan ja lähetetään pyyntö uudelleen tietokantalähteelle. Senhetkinen aktiivisen ja varmuuskopion päätepisteet vaihtaa rooleja: asiakassovellus katsoo vanha aktiivisen päätepisteen uuden varmuuskopion päätepisteen ja vanha varmuuskopion päätepisteen on uusi aktiivisen päätepisteen. Jos sekä lähettää epäonnistuvat, kaksi kohteiden roolit pysyy muuttumattomana ja funktio palauttaa virheen.

[Geo-replikoinnin palvelun Bus välittäminen viestien mukana][] -esimerkissä esitetään, kuinka voit toistaa releitä.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Olevien ja aiheet palvelinkeskuksen katkokset tai lieventämiseksi suojaaminen

Tavoitteet toimintakykyyn palvelinkeskuksen katkokset vastaan, kun käyttämällä se messaging-palvelun Bus tukee kahdella tavalla: *aktiivinen* ja *Passiivinen* replikoinnin. Kunkin lähestymistavan Jos jonon tai aiheen on edelleen käytettävissä palvelinkeskuksen-käyttökatkosta kanssa voit luoda sen-sekä nimitilan. Kumpaankin voi olla samaa nimeä. Esimerkiksi ensisijainen jonon tavoittaa kohdassa **contosoPrimary.servicebus.windows.net/myQueue**samalla, kun toissijainen vastaavaan tavoittaa **contosoSecondary.servicebus.windows.net/myQueue**-kohdassa.

Jos sovellus ei edellytä pysyvä lähettäjän ja vastaanottajan viestintä-sovelluksen voit toteuttaa asiakkaan kestävät jonon estää viestin tappio ja suojata lyhytkestoisia palvelun Bus virheitä lähettäjän.

## <a name="active-replication"></a>Aktiivinen sallittuja

Aktiivinen replikointi käyttää kohteita sekä nimitilan jokaisen toiminnolle. Asiakas, joka lähettää viestin lähettää saman viestin kaksi kopiota. Ensimmäinen kopio lähetetään ensisijainen kohde (esimerkiksi **contosoPrimary.servicebus.windows.net/sales**) ja toinen viestin kopio lähetetään toissijaisen yksikön (esimerkiksi **contosoSecondary.servicebus.windows.net/sales**).

Asiakkaan vastaanottaa viestit sekä olevien. Vastaanottajan käsittelee ensimmäisen viestin kopion, ja toinen kopio ei jätetä pois. Jos et halua viesteistä, lähettäjän on tunnisteen kukin viesti, jossa on yksilöllinen tunnus. Molemmat kopion pitää olla merkitty on sama tunniste. Voit merkitä viestin [BrokeredMessage.MessageId][] tai [BrokeredMessage.Label][] -ominaisuuksia ja mukautetun ominaisuuden avulla. Vastaanottaja on säilytettävä viesteihin, jotka on jo vastaanottanut luettelo.

[Palvelun Bus se viestien mukana Geo-replikoinnin][] -esimerkissä aktiivinen replikoinnin viestien kohteita.

> [AZURE.NOTE] Aktiivinen replikoinnin lähestymistapa kaksinkertaistuu toimintoja, vuoksi tämän menetelmän voi johtaa suurempiin kustannukset.

## <a name="passive-replication"></a>Passiivinen sallittuja

Passiivinen replikointi käyttää vain yhteen kaksi tekstiviesti kohteiden vika vapaa-tapauksessa. Asiakkaan lähettää viestin aktiivisen kohteen. Jos aktiivisen kohteen toiminto epäonnistuu, joka sisältää virhekoodin, joka ilmaisee, joka isännöi aktiivisen kohteen palvelinkeskuksen ei ehkä ole käytettävissä, asiakastietokone lähettää viestin kopio varmuuskopio-kohteeseen. Senhetkinen aktiivisen ja varmuuskopion kohteiden vaihtaa rooleja: lähettäminen asiakkaalle katsoo vanha aktiivisen kohteen varmuuskopioinnin uuden kohteen voi ja vanha varmuuskopioinnin kohde on uuden aktiivisen kohteen. Jos sekä lähettää epäonnistuvat, kaksi kohteiden roolit pysyy muuttumattomana ja funktio palauttaa virheen.

Asiakkaan vastaanottaa viestit sekä olevien. Koska kokeilemaan vastaanottaja saa ilmoituksen kopioiden saman viestin vastaanottaja on estää viesteistä. Voit estää kaksoiskappaleet samalla tavalla aktiivinen replikoinnin ohjeiden mukaisesti.

Passiivinen replikoinnin on yleensä taloudellisia enemmän kuin aktiivinen replikoinnin, koska useimmissa tapauksissa vain yksi toiminto suoritetaan. Viive, liikenteen ja raha kustannukset ovat samat-replikoida-skenaario.

Passiivinen replikointia käytettäessä seuraavissa tilanteissa viestit voidaan menettää tai vastaanotettu kaksi kertaa:

-   **Viestin viive tai tappion**: oletetaan, että lähettäjän lähetetty viesti m1 ensisijainen jonossa ja sitten jonossa ole käytettävissä, ennen kuin vastaanottaja saa ilmoituksen m1. Lähettäjän lähettää valintasi m2 toissijainen jonossa. Jos ensisijainen jonossa on tilapäisesti poissa käytöstä, vastaanottaja saa ilmoituksen m1, kun jonossa käytettävyys uudelleen. Jos huono, vastaanottaja voi tulla koskaan m1.

-   **Kaksoiskappaleiden vastaanotto**: oletetaan, että lähettäjä lähettää viestin m ensisijainen jonossa. Palvelun Bus onnistui käsittelee m, mutta ei lähetä vastaus. Kun lähetystoiminto aikakatkaistaan, lähettäjän lähettää m samanlaiset kopio toissijainen jonossa. Jos vastaanottaja voi vastaanottaa m ensimmäisen kopio ennen ensisijainen jonossa ei ole käytettävissä, vastaanottaja saa molemmat kopiot m likimain yhtä aikaa. Jos vastaanottaja ei voi vastaanottaa m ensimmäisen kopio ennen ensisijainen jonossa ei ole käytettävissä, vastaanottaja saa aluksi vain m toisen kopion, mutta vastaanottaa m toisen kopion sitten, kun ensisijainen jonossa on käytettävissä.

[Palvelun Bus se viestien mukana Geo-replikoinnin][] -esimerkissä passiivinen replikoinnin viestien kohteita.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja tietojen palauttamisesta on seuraavissa artikkeleissa:

- [Azure SQL-tietokannan Liiketoiminnan jatkuvuus][]
- [Azure vikasietoisuudelle tekniset ohjeet][]

  [Bus todentaminen]: service-bus-authentication-and-authorization.md
  [Osioitu tekstiviesti yksiköt]: service-bus-partitioning.md
  [Asynkroninen tekstiviesti kuvioiden ja suuri käytettävyys]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Palvelun Bus GEO-replikointi viestien välittäminen]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Palvelun Bus GEO-replikointi se viestit]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Azure SQL-tietokannan Liiketoiminnan jatkuvuus]: ../sql-database/sql-database-business-continuity.md
  [Azure vikasietoisuudelle tekniset ohjeet]: ../resiliency/resiliency-technical-guidance.md
