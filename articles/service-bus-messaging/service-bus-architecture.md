<properties 
    pageTitle="Palvelun Bus arkkitehtuuri | Microsoft Azure"
    description="Kuvaa viestin ja välitys käsittelyn Azure palvelun Bus arkkitehtuuri."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Palvelun Bus-arkkitehtuuri

Tässä artikkelissa kuvataan viesti ja välitys käsittelyn Azure palvelun Bus arkkitehtuuri.

## <a name="service-bus-scale-units"></a>Palvelun Bus aikayksikön

Palvelun Bus on järjestetty *aikayksikön*. Asteikon yksikkö projektin käyttöönoton ja sisältää kaikki tarvittavat komponentit suoritetaan palvelu. Kunkin alueen ottaa käyttöön yhden tai usean palvelun Bus aikayksikön.

Palvelun Bus nimitila on yhdistetty asteikko. Asteikko käsittelee palvelun Bus yksiköiden kaikki tyypit: releitä ja se tekstiviesti kohteet (olevien, aiheet ja tilaukset). Palvelun Bus asteikko sisältää seuraavat osat:

- **Yhdyskäytävän solmujen joukko.** Yhdyskäytävän solmujen todennetaan pyynnöt ja käsitellä välitys pyynnöt. Yhdyskäytävän kunkin solmun on julkiseen IP-osoite.

- **Viestintä broker solmujen joukko.** Tekstiviesti broker solmujen käsitellä tekstiviesti kohteiden koskevat pyynnöt.

- **Yhden yhdyskäytävän säilö.** Yhdyskäytävän säilö sisältää tiedot jokaisen kohteen, joka on määritetty tämä asteikko. Yhdyskäytävän säilö on toteutettu SQL Azure-tietokannan päälle.

- **Useita tekstiviesti stores.** Tekstiviesti stores pidä viestit olevien, aiheet ja tilaukset, jotka on määritelty tämän asteikko. Se sisältää myös kaikki tilauksen tiedot. Ellei [osioitua messaging kohteiden](service-bus-partitioning.md) on käytössä, jonossa tai aiheen on yhdistetty yksi tekstiviesti säilö. Tilaukset on tallennettu samaan tekstiviesti säilöön kuin ylemmän tason aiheen. Lukuun ottamatta palvelun Bus [Premium Messaging](service-bus-premium-messaging.md)tekstiviesti stores otettu käyttöön SQL Azure-tietokannoista päälle.

## <a name="containers"></a>Säilöjen

Tekstiviesti kullekin objektille on määritetty tietyn säilön. Säilön on looginen rakennetta, joka käyttää yksi messaging säilö-säilö tämän säilön kaikkia aiheeseen liittyviä tietoja. Kunkin säilön määritetään tekstiviesti broker solmu. Yleensä on enemmän kuin messaging broker solmujen säilöjen. Tämän vuoksi kunkin tekstiviesti broker solmun Lataa useita säilöjen. Säilöjen tekstiviesti broker solmu jakautumisen on järjestetty siten, että kaikki tekstiviesti broker solmut tasaisesti ladataan. Lataa toistumiskaava muutokset (esimerkiksi jokin varattu säilöjen saa) tai jos tekstiviesti broker solmu ei ole tilapäisesti käytettävissä, jos säilöt ovat uudelleen tekstiviesti broker solmut.

## <a name="processing-of-incoming-messaging-requests"></a>Tekstiviesti pyynnöt käsittely

Kun asiakas lähettää pyynnön palvelun Bus, Azure kuormituksen reitittää sen mihin tahansa yhdyskäytävän solmut. Yhdyskäytävän solmu luvalla pyynnön. Jos pyyntö koskee tekstiviesti kohteen (jonossa, aiheen, tilauksen), yhdyskäytävä-solmu kohteen yhdyskäytävän kaupan hakee ja määrittää, mitkä tekstiviesti-kaupan kohteen sijaitsee. Se sitten hakee tekstiviesti broker solmun on tällä hetkellä ylläpidon tämä säilö ja lähettää kutsun tekstiviesti broker solmun. Tekstiviesti broker solmu käsittelee pyynnön ja päivittää kohteen tilan säilö Storessa. Tekstiviesti broker solmu lähettää vastauksen yhdyskäytävä-solmu, joka lähettää tarvittavat vastauksen takaisin asiakkaalle, joka on myöntänyt alkuperäisen pyynnön.

![Tekstiviesti pyynnöt käsittely](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Välitys pyynnöt käsittely

Kun asiakas lähettää pyynnön palvelun Bus, Azure kuormituksen reitittää sen mihin tahansa yhdyskäytävän solmut. Jos kutsu on kuunteleminen pyynnön, yhdyskäytävän solmun Luo uusi välitys. Jos pyyntö on tietyn välitys yhteyden pyynnön, yhdyskäytävä-solmu välittää yhdyskäytävän solmu, joka omistaa välityksen yhteyden pyynnön. Yhdyskäytävän solmu, joka omistaa välityksen lähettää rendezvous kysyy Listenerin, jotta voit luoda yhdyskäytävän solmu, joka on saanut yhteyden pyynnön tilapäinen kanavan kuunteleminen asiakkaalle.

Kun välitys-yhteys on muodostettu, asiakkaat vaihtoon viestejä, jota käytetään rendezvous yhdyskäytävän solmun kautta.

![Välitys pyynnöt käsittely](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet lukenut yleiskatsaus palvelun Bus arkkitehtuuri, Aloita daxista on seuraavissa linkeissä:

- [Palvelun Bus tekstiviesti yleiskatsaus](service-bus-messaging-overview.md)
- [Palvelun Bus perusteet](service-bus-fundamentals-hybrid-solutions.md)
- [Käyttämällä palvelun Bus olevien jonossa tekstiviesti ratkaisu](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
