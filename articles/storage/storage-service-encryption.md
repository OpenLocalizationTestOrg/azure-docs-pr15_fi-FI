<properties
    pageTitle="Tietoja muiden Azure tallennustilan palvelun salaus | Microsoft Azure"
    description="Azure-tallennustilan palvelun salaus-toiminnolla voit salata Azure Blob-säiliö palvelun reunassa, kun tietojen tallentamista ja salausta, kun tiedot noudetaan."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Tietoja muiden Azure tallennustilan palvelun salaus

Azure tallennustilan palvelun salaus (SSE) tietojen, loput auttaa suojaamaan ja vastaa organisaation suojaus ja velvoitteiden noudattaminen tietoja. Tämän ominaisuuden Azuren tallennustilaan automaattisesti salaa ennen toteaa tallennustilan tiedot ja purkaa ennen hakemista. Salauksen, salauksen ja hallintaa ovat täysin läpinäkyvä käyttäjille.

Seuraavissa osissa on yksityiskohtaiset ohjeet käyttämisestä tallennustilan palvelun salauksen ominaisuuksia sekä tuetut tilanteet ja käyttäjä kohtaa.

## <a name="overview"></a>Yleiskatsaus

Azuren tallennustila on kattavaa tietoturvaominaisuudet, jotka mahdollistavat yhdessä kehittäjät voivat luoda suojatun sovelluksia. Tietoja voi suojata siirron sovelluksen ja Azure välillä käyttämällä [Asiakaspuolen salausta](storage-client-side-encryption.md), HTTPs tai SMB 3.0. Tallennustilan palvelun salaus on salaus rest-palvelussa, käsittely-salausta tai salauksen hallintaa täysin läpinäkyvä tavalla. Kaikki tiedot salataan 256 bittistä [AES-salausta](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jokin käytettävissä vahvinta Cipher.

SSE toimii salaamalla tiedot kirjoitetaan Azuren tallennustilaan ja voi käyttää estä BLOB-sivun BLOB-objektit ja liittää BLOB-objektit. Se toimii seuraavasti:

-   Yleisiä tarkoitus tallennustilan ja Blob storage-tilit
-   Vakio tallentamisesta ja tallennustilaa Premium 
-   Kaikki redundancy tasoilla (LRS, ZRS, GRS, RA GRS)
-   Azure Resurssienhallinta tallennustilan tilit (mutta ei perinteinen) 
-   Kaikkien alueiden

Ottaa käyttöön tai poistaa tallennustilaa palvelun salaus tallennustilan tilin, kirjaudu sisään [Azure portal](https://azure.portal.com) ja valitse tallennustilan-tili. Etsi Blob-palvelun kohdassa kuvatulla tavalla tässä näyttökuvassa asetukset-sivu ja valitse salausta.

![Portaalin näyttökuva salaus-asetusta](./media/storage-service-encryption/image1.png)

Kun olet valinnut salaus-asetus, voit ottaa käyttöön tai käytöstä tallennustilan palvelun salausta.

![Portaalin näyttökuva salauksen ominaisuudet](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Salauksen skenaariot

Tallennustilan palvelun salaus voidaan ottaa käyttöön tallennustilan tilin tasolla. Se tukee asiakkaan seuraavissa tilanteissa:

-   Estä BLOB salaamista liittää BLOB-objektit ja sivun BLOB-objektit.

-   Arkistoidut Näennäiskiintolevyjen ja Azure-paikallisen toi malleihin salauksen.

-   Luotu oman näennäiskiintolevyjen IaaS VMs pohjana OS ja tietojen levyjen salausta.

SSE on seuraavat rajoitukset:

-   Perinteinen tallennustilan tilien salausta ei tueta.

-   Perinteinen tallennustilaa tilien siirtää Resurssienhallinta tallennustilan tileille salausta ei tueta.

-   Olemassa olevien tietojen - SSE salaa juuri luomasi tiedot vain, kun salaus on otettu käyttöön. Jos esimerkiksi luoda uuden Resurssienhallinta-tallennustilan tilin, mutta älä ota käyttöön salaus ja sitten tallennustilan tilin BLOB tai arkistoidut Näennäiskiintolevyjen lataaminen ja ota SSE, kyseiset blob ei salata, paitsi jos ne on kirjoitettava uudelleen tai kopioida.

-   Marketplace-tukeen – Ota salaamista VMs, joka on luotu [Azure portal](https://portal.azure.com), PowerShell ja Azure CLI Marketplacesta. Näennäiskiintolevyn kuvan pysyy salaamattomana; kuitenkin kaikki kirjoituksia valmis jälkeen AM on kehrätty määrittäminen salataan.

-   Taulukko, olevien, ja tiedostojen tietoja ei salata.

##<a name="getting-started"></a>Käytön aloittaminen

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Vaihe 1: [Luo uusi tallennustilan tili](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Vaihe 2: Ota käyttöön salausta.

Voit ottaa [Azure portal](https://portal.azure.com)-salausta.

> [AZURE.NOTE] Jos haluat ottaa käyttöön tai poistaa tallennustilan tilin tallennustilan Service-salausta ohjelmallisesti, voit [Azure tallennustilan resurssin tarjoajan REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx), [.NET tallennustilan resurssin tarjoajan asiakkaan kirjastoon](https://msdn.microsoft.com/library/azure/mt131037.aspx), [PowerShellin Azure](../powershell-install-configure.md)tai [Azure CLI](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Vaihe 3: Tietojen kopioiminen tallennustilan tilin

Jos otat käyttöön SSE tallennustilan tilille ja kirjoittaa sitten BLOB storage tilin, salataan BLOB-objektit. BLOB-objektit, jo sijaitsee tallennustilan tilin ei salata, kunnes ne on kirjoitettava uudelleen. Voit kopioida tiedot SSE salattu, jossa yksi tallennustilan tilin tai jopa ottaa SSE ja yksi säilö BLOB-objektit kopioiminen toiseen, varmista, että edellisen tiedot salataan. Voit tehdä tämän jollakin seuraavista työkaluista.

#### <a name="using-azcopy"></a>AzCopy käyttäminen

AzCopy on tarkoitettu tietojen kopioiminen ja sieltä pois Microsoft Azure-Blob, tiedoston ja taulukon tallennustilan yksinkertainen komentojen avulla saat parhaan mahdollisen suorituskyvyn Windows komentorivin apuohjelma. Tämän avulla voit kopioida oman BLOB storage-tililtä toiseen, jossa on käytössä SSE. 

Lisätietoja on owa_for_androidiin [tiedonsiirron AzCopy komentorivivalitsimet-apuohjelman avulla](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>Tallennustilan asiakas-kirjastojen avulla

Voit kopioida blob-tietojen avulla ja Blob-objektien tallennustilaan tai käyttämällä Microsoftin monipuoliset tallennustilan asiakas-kirjastossa, mukaan lukien .NET, C++, Java, Android, Node.js, PHP, Python ja Ruby tallennustilan tilien välillä.

Lisätietoja on käy Microsoftin [Azure-Blob-säiliö käyttämällä .NET käytön aloittaminen](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Tallennustilan Resurssienhallinnassa

Voit käyttää tallennustilan explorer tallennustilan tilien luominen, lataa ja lataa tiedot, BLOB sisällön tarkasteleminen ja selata kansioita. Voit tehdä jommankumman lataaminen BLOB storage tiliisi salausta. Ja tallennustilaa joitakin hallinnasta voit myös kopioida tietoja aiemmin Blob-objektien tallennustilaan-tallennustilan tilin tai uusi tallennustilan-tili, jolla on käytössä SSE säilöön.

Lisätietoja on Siirry [Azure-tallennustilan hallinnasta](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Vaihe 4: Kyselyn salatun tiedon tila

Tallennustilan asiakas-kirjastojen päivitetty versio on otettu käyttöön, jonka avulla voit kyselyn tilan objektin määrittääksesi, jos se on salattu vai ei. Esimerkkejä lisätään tämän asiakirjan lähitulevaisuudessa.

Tässä vaiheessa voit soittaa [Hae tilin ominaisuudet](https://msdn.microsoft.com/library/azure/mt163553.aspx) ja varmista, että tallennustilan-tilillä on salausta tai tarkastele tallennustilan tilin ominaisuudet Azure-portaalissa.

##<a name="encryption-and-decryption-workflow"></a>Salauksen ja salauksen työnkulku

Seuraavassa on lyhyt kuvaus/salauksen työnkulun:

-   Asiakkaan mahdollistaa salauksen tallennustilan tilin.

-   Kun asiakas kirjoittaa Blob-objektien tallennustilaan, uudet tiedot (Blob-objektien valitseminen, SIJOITA estä, SIJOITA sivun jne.) jokaisen kirjoitus on salattu 256 bittistä [AES-salausta](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jokin käytettävissä vahvinta Cipher.

-   Asiakkaan täytyy käyttämään tietoja (HAE Blob jne.), kun tiedot on automaattisesti purettu ennen käyttäjälle, joka palauttaa.

-   Jos salausta ei ole käytössä, uusi kirjoituksia enää salataan ja aiemmin salatut tiedot on salattu, kunnes kirjoitettava uudelleen käyttäjän. Kun salaus on otettu käyttöön, kirjoittaa Blob-objektien tallennustilaan salataan. Tilan tiedot eivät muutu VAIHTO välillä ottaminen käyttöön tai poistaminen käytöstä-tallennustilan tilin salaus käyttäjän kanssa.

-   Kaikki salausavaimet on tallennettu, salataan ja hallitsee Microsoft.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Usein kysyttyjä kysymyksiä tallennustilan palvelun salaus tietojen, loput

**Kysymys: saan on perinteinen tallennustilan tili. SSE käyttöön sitä**

A: ei, SSE tuetaan vain Resurssienhallinta tallennustilan tilit.

**K: miten tiedot voit salata perinteinen tallennustilan-tilini?**

V: Voit luoda uuden Resurssienhallinta-tallennustilan tilin ja kopioida tietoja avulla [AzCopy](storage-use-azcopy.md) aiemmin perinteinen tallennustilan tililtä tiliisi juuri luomasi Resurssienhallinta-tallennustilan. 

Toinen vaihtoehto on siirrettävä perinteinen tallennustilan tilin tallennustilan tilin, resurssien hallinta. Lisätietoja on artikkelissa [Ympäristö tuettu siirron, IaaS resurssit klassinen resurssien hallinta](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/).

**Kysymys: saan on Resurssienhallinta tallennustilan tili. SSE käyttöön sitä**

V: Kyllä, mutta vain äskettäin kirjoitettu BLOB salataan. Ei siirry takaisin ja salata tiedot, jotka on jo olemassa. 

**Kysymys: saan haluat salata aiemmin Resurssienhallinta tallennustilan tilin nykyiset tiedot?**

V: Voit ottaa SSE Resurssienhallinta-tallennustilan tilin milloin tahansa. Kuitenkin BLOB-objektit, jotka olivat jo ole salattu. Salaa näiden BLOB-objektit, kopioi ne toinen nimi tai säilö ja poista sitten salaamaton versiot.

**Kysymys: saan käytän Premium tallennustilan; Voinko käyttää SSE?**

V: Kyllä, SSE tuetaan vakio tallennustilan ja Premium-tallennustilan.

**K: Jos voin tallennustilan uuden tilin luominen ja SSE ottaminen käyttöön ja valitse sitten Luo uusi AM, tallennustilan tilin avulla, tämä tarkoittaa Omat AM salataan?**

V: Kyllä. Minkä tahansa luotu levyjä, jotka käyttävät uuden tallennustilan tilin salataan, kunhan luodaan, kun SSE on otettu käyttöön. Jos AM on luotu käyttämällä Azure Marketplace-Näennäiskiintolevyn kuvan pysyy salaamattomana; kuitenkin kaikki kirjoituksia valmis jälkeen AM on kehrätty määrittäminen salataan.

**K: Voinko luoda tallennustilan käyttäjätilejä SSE käytössä PowerShellin Azure ja Azure CLI kanssa?**

V: Kyllä.

**K: miten paljon Azuren tallennustilaan maksaa Jos SSE on käytössä?**

A: ei ilman lisäkustannuksia.

**K: kuka hallitsee salausavaimet?**

A: näppäimet hallitsee Microsoft.

**K: Voinko käyttää omaa salausavaimet?**

A: olemme työskentelevät asiakkaat voivat tuoda Omat salausavaimet ominaisuudet.

**K: Voinko peruuttaa salausavaimet käyttöoikeudet?**

A: ei tällä hetkellä; Microsoft hallitsee täysin näppäimet.

**K: on oletusarvoisesti käytössä SSE luodessasi uuden tallennustilan tilin?**

A: SSE ei ole käytössä oletusarvoisesti. Azure portaalin avulla voit ottaa sen käyttöön. Voit ottaa tämän ominaisuuden avulla tallennustilan resurssin tarjoajan REST API myös ohjelmallisesti.

**K: miten tämä on sama kuin Azure-salausta?**

Vastaus: tätä toimintoa käytetään salaamaan tiedot Azure-Blob-objektien tallennustilaan. Azure-levynsalaus käytetään salaa OS ja tietojen levyjen IaaS VMs. Lisätietoja on käy Microsoftin [Tallennustilan Security Guide](storage-security-guide.md).

**K: mitä jos SSE, ja valitse Siirry ja ulos ottaa käyttöön käyttöön Azure salauksen kiintolevyillä?**

A: se toimii saumattomasti. Tiedot salataan lopettamista.

**Kysymys tallennustilan tilin määrittämisen jälkeen replikoida geo-toisena. Jos voin käyttöön SSE, tarpeettomat versiotani myös salataan?**

V: Kyllä, kaikki kopiot-tallennustilan tilin salataan ja kaikki redundancy asetukset – paikallisesti (LRS tarpeettomat tallennustilan), vyöhykkeen Redundant Storage (ZRS), Geo Redundant Storage (GRS) ja lukuoikeudet Geo Redundant Storage (RA GRS) – ovat tuettuja.

**Kysymys: saan ei voi ottaa salauksen tallennustilan tilini.**

A: on Resurssienhallinta-tallennustilan tilin? Perinteinen tallennustilan tilit eivät ole tuettuja. 

**K: onko SSE sallittu vain tiettyjä alueita?**

V: SSE on saatavilla kaikilla alueilla. 

**K: Miten voin ottaa yhteyttä joku jos minulla on ongelmia tai haluat antaa palautetta?**

A:, ota yhteyttä [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) varten tallennustilan palvelun salaus liittyvät ongelmat.

##<a name="next-steps"></a>Seuraavat vaiheet

Azuren tallennustila on kattavaa tietoturvaominaisuudet, jotka mahdollistavat yhdessä kehittäjät voivat luoda suojatun sovelluksia. Lisätietoja on seuraavassa [Tallennustilan Security Guide](storage-security-guide.md).
