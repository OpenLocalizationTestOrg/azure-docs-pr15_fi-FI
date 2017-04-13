
<properties
    pageTitle="Sovelluksen vaatimukset Azure RemoteApp | Microsoft Azure"
    description="Lisätietoja sovellukset, jotka haluat käyttää Azure RemoteApp vaatimukset"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>Sovelluksen vaatimukset

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp tukee streaming 32-bittisen tai 64-bittinen Windows-sovellukset Windows Server 2012 R2 kuvasta. Useimmat aiemmin 32-bittisen tai 64-bittinen Windows-sovellukset toimivat "on" Azure RemoteApp (Etätyöpöytäpalvelut tai aiemmin nimeltään Päätepalvelut) ympäristössä. Kuitenkin on käynnissä ja toiminnan erotuksen - sovelluksia toimi oikein ja suorittaa, kun muut käyttäjät eivät. Seuraavassa on ohjeita Etätyöpöytäpalvelut ympäristössä-sovellusten kehittäminen ja yhteensopivuuden varmistamiseksi testaaminen.

Vihje: Pyrimme parhaillaan korjaamaan luominen joitakin esimerkkejä sovellukset. Näkyviin tulee uusi aiheisiin, joissa käsitellään käyttämällä Microsoft Accessin QuickBooks ja sovelluksen V RemoteApp.

## <a name="requirements"></a>Vaatimukset
Nämä kolme vaatimukset, jos seuraa, Ohje suorittaa hyvin RemoteApp sovelluksen:

1.  Sovellukset, jotka täyttävät kaikki [Windows-työpöytäsovellusten sertifikaatin vaatimukset](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) ja noudattaa [Etätyöpöytäpalvelut](https://msdn.microsoft.com/library/aa383490.aspx) ohjelmointi ohjeita on valmis RemoteApp-yhteensopivuus.
2.  Sovellusten olisi koskaan tallentaa tiedot paikallisesti kuva tai RemoteApp esiintymät, voidaan menettää.  Kun olet luonut RemoteApp-sivustokokoelman, esiintymät on kopioitu ja ovat tilattomien ja saa sisältää vain sovellukset. Tallentaa tietoja ulkoisesta tietolähteestä tai käyttäjän profiiliin.
3.  Mukautetun kuvia ei koskaan tulisi sisältää tietoja, jotka voidaan menettää.  

## <a name="testing-your-apps"></a>Yrityksen sovellusten testaaminen
Käytä näitä vaiheita sovellusten testaaminen:

1.  Asenna Windows Server 2012 R2 ja sovelluksen
2.  Etätyöpöytä ottaminen käyttöön
3.  Luo kaksi käyttäjätilit, UserA ja UserB, sekä käyttäjätilien lisääminen Etätyöpöytä suojaus-ryhmä.
4.  Usean istunnon yhteensopivuuden tarkistaminen luomalla kaksi samanaikaisesti RDS istuntoon ja tietokoneen samalla, kun sovellus käynnistetään.
5.  Vahvista sovelluksen toiminta

## <a name="application-development-guidelines"></a>Sovellusten kehittämisen ohjeet
Seuraavien ohjeiden avulla voit kehittää RemoteApp sovelluksia.

### <a name="multiple-users"></a>Useiden käyttäjien

- [Yksittäisen käyttäjän sovelluksen ](https://msdn.microsoft.com/library/aa380661.aspx)asentaminen luoda ongelmia yhteiskäyttö ympäristössä.
- Sovellusten kannattaa [tallentaa käyttäjäkohtainen](https://msdn.microsoft.com/library/aa383452.aspx) käyttäjäkohtainen sijainneissa, erillään yleisestä tietoja, jotka koskevat kaikkia käyttäjiä.
- RemoteApp käyttää useita [nimitilan ydin objektien](https://msdn.microsoft.com/library/aa382954.aspx); Yleinen nimitila käytetään ensisijaisesti services asiakkaan ja palvelimen-sovelluksissa.
- Ei ole Turvalliset oletetaan, että tietokonenimi tai [IP-osoite](https://msdn.microsoft.com/library/aa382942.aspx) , tietokoneeseen on liitetty yksittäisen käyttäjän koska useat käyttäjät voivat kirjauduttava samanaikaisesti Remote Desktop-istunnon isäntään (RD istunnon isäntä)-palvelimeen.

### <a name="performance"></a>Suorituskyky
- Poista käytöstä [graafiset tehosteet](https://msdn.microsoft.com/library/aa380822.aspx) , ennen kuin lisäät sovelluksen RemoteApp.
- Suurenna Suoritin käytettävyys kaikille käyttäjille, poista [taustatehtävät](https://msdn.microsoft.com/library/aa380665.aspx) käytöstä tai luo tehokas taustan tehtäviin, jotka eivät ole paljon resursseja.
- Kannattaako paranna ja saldo yhteiskäyttö usean suorittimen ympäristössä sovelluksen [viestiketjun käyttö](https://msdn.microsoft.com/library/aa383520.aspx) .
- Suorituskyvyn parantamiseksi on hyvä [olevien](https://msdn.microsoft.com/library/aa380798.aspx) sovellusten onko ne ovat käytössä asiakas-istunnossa.
