<properties
    pageTitle="Azure julkishallinnon palvelut | Microsoft Azure"
    description="Sisältää ja käytettävissä olevien palveluiden Azure Government-yleiskatsaus"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc" />


#  <a name="security"></a>Tietoturva

##  <a name="principles-for-securing-customer-data-in-azure-government"></a>Periaatteet Azure Government asiakkaiden tietojen suojaaminen

Azure Government on erilaisia ominaisuuksia ja palveluita, joiden avulla voit cloud ratkaisuja säännelty ja hallita tietojen omien tarpeiden. Yhteensopiva asiakas-ratkaisun ei ole enää mitään muuta kuin ulos,-valmiilla Azure Government-ominaisuuksia, tasainen tietojen suojauksen käytäntö on tehokasta.

Jos isännöit Azure Government ratkaisu, Microsoft käsittelee monia näistä vaatimuksista cloud infrastruktuurin tasolla.

Seuraavassa kaaviossa on esitetty Azure linnaa syvyys mallia. Esimerkiksi Microsoft toimittaa sekä asiakas-ominaisuudet, kuten suojauksen laitteiden asiakaskohtainen sovelluksen WWW.microsoft.com-sivustoa kohtaan on basic pilvi-infrastruktuuria WWW.microsoft.com-sivustoa-kohtaan.

![Vaihtoehtoinen teksti](./media/azure-government-Defenseindepth.png)

Tämän sivun ympärille foundational periaatteet suojaaminen palvelut ja sovellukset-ohjeita ja parhaita käytäntöjä antamisen siitä, miten voit käyttää näitä periaatteet; Toisin sanoen kuinka asiakkaat pitäisi tehdä Azure Government Yleiset tehtävät, joita tarvitaan ratkaisun, joka käsittelee ITAR tietoja ja velvoitteiden täyttämiseksi smart käyttö.

 Asiakkaiden tietojen suojaaminen erityissuuntaviivat periaatteet ovat seuraavat:

- Käyttämällä salausta tietojen suojaaminen
- Tietoja hallinta
- Eristystaso tietoja käytön rajoittaminen

###  <a name="protecting-customer-data-using-encryption"></a>Asiakkaiden tietojen käyttämällä salausta suojaaminen

Pienentävät riskien ja kokouksen säädösten velvoitteiden ajat autoa kasvava kohdistus ja salauksen tärkeyttä. Tehokas salaus toteutus avulla voit parantaa nykyisen verkko- ja sovelluksen tietoturvatoimenpiteistä — tai vähentää yleinen riskiä cloud-ympäristössä.

#### <a name="encryption-at-rest"></a>Salauksen rest-palvelussa
Tietoja muiden salaus koskee asiakkaan sisällön säilyttämistä levyn suojaus. Näin voi käydä seuraavilla tavoilla:

#### <a name="storage-service-encryption"></a>Tallennustilan palvelun salaus

Azure tallennustilan palvelun salaus on käytössä tilin tallennustasoa estä BLOB-objektit ja sivun BLOB parhaillaan salattuja automaattisesti, kun kirjoitettu Azure-tallennustilan. Kun tietojen lukeminen Azure-tallennustilan, se puretaan tallennustilan-palvelun ennen palautetaan. Käytä tätä tietojen suojaamiseen eikä sinun tarvitse muokata tai lisätä koodia sovelluksia.

#### <a name="client-side-encryption"></a>Asiakkaan salaus
Asiakkaan salaus on valmiina Java ja .NET tallennustilan asiakas-kirjastot, jotka voivat hyödyntää Azure avaimen säilö API, että yksinkertaista toteuttamisesta. Azure avaimen säilö avulla voit hankkia käyttämään tietoja Azure avain säilöön Azure Active Directoryn avulla tietyille käyttäjille.

#### <a name="encryption-in-transit"></a>Siirron salaus

Azure Government yhteys käytettävissä basic salaus tukee tason TLS (Transport Security) 1.2 protokolla ja X.509-varmenteita. Federal tietojen käsittely FIPS (Standard) 140-2-tason 1 salauksen algoritmit käytetään myös infrastruktuurin verkkoyhteyksien Azure Government palvelinkeskusten välillä.  Windows Server 2012 R2: n ja Windows 8-plus VMs ja Azure tiedostoresurssit käyttää SMB 3.0 salauksen AM ja jaettua tiedostoresurssia välillä. Tiedot salataan, ennen kuin se siirretään varastoon asiakassovellus ja purkaa tiedot jälkeen se on siirretty loppunut asiakaspuolen salausta.

#### <a name="best-practices-for-encryption"></a>Salauksen parhaat käytännöt

- IaaS VMs: Azure levyn salausta. Tallennustilan palvelun salaus salata, joiden avulla voit varmuuskopioida ne levyjen Azuren tallennustilaan näennäiskiintolevytiedostoja käyttöön, mutta tämä vain salaa äskettäin kirjoitettujen tiedot. Tämä tarkoittaa, että jos luot AM ja tallennustilaa palvelun salauksen ottaminen käyttöön, joka sisältää Näennäiskiintolevytiedosto tallennustilan tilin, vain muutokset salataan, ei alkuperäinen Näennäiskiintolevytiedosto.
- Asiakkaan salaus: Tämä on turvallisin tapa salaamiseen tietoja, koska se salaa sitä ennen salataanko siirrettävät ja salaa muiden tiedoissa. Kuitenkin edellyttävät koodin lisääminen käyttämällä tallennustilaa, jotka ehkä halua tehdä sovellusten. Tällöin voit HTTPs salataanko siirrettävät ja tallennustilaa palvelun salaus tietojen, loput tiedot salataan. Asiakkaan salaus liittyy myös useita kuormituksen asiakkaan – asiakas tämän skaalattavuus-palvelupakettien, erityisesti, jos ole salaaminen ja siirtäminen paljon tietoja on.

###  <a name="protecting-customer-data-by-managing-secrets"></a>Asiakkaiden tietojen suojaaminen toteuttavat tietoja

Suojatun hallintaa on tärkeää suojaaminen pilveen. Asiakkaiden olisi pyri yksinkertaistamaan hallintaa ja hallita näppäinyhdistelmien cloud-sovellusten ja palveluiden käyttämän salaamaan tiedot.

#### <a name="best-practices-for-managing-secrets"></a>Tietoja hallinnan parhaista käytännöistä

- Avaimen säilö avulla voit pienentää julkaisemisen koodattu määritysten tiedostot-komentosarjojen kautta tai lähdekoodin tietoja riskeistä. Azure avaimen säilö salaa näppäimet (kuten Azure salauksen salausavaimet) ja tietoja (esimerkiksi salasanat), tallentamalla ne FIPS 140-2 tason 2 vahvistettu laitteiston suojauksen moduulit (HSMs). Lisätty assurance voit tuoda tai luo näppäimet nämä HSMs.
- Sovelluksen koodin ja mallit olisi vain viittauksia URI tietoja (mikä tarkoittaa todellinen tietoja eivät ole koodi, määrittäminen tai lähdekoodin säilöjen tietoihin). Tällöin avaimen tietojen kalastelu-sisäisiä ja ulkoisia repos, kuten korjuun-bots-GitHub.
- Käytä vahva RBAC ohjausobjektien avaimen säilö. Jos luotettu operaattori jättää yrityksen tai siirrot yrityksen omistamien uuteen ryhmään, olisi estettävä ei voi käyttää tietoja.

Lisätietoja <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure avaimen säilö julkisen ohjeissa.</a>

###  <a name="isolation-to-restrict-data-access"></a>Eristystaso tietoja käytön rajoittaminen

Eristystaso 's all about käyttämällä rajat Segmentointi ja säilöjen tietojen käyttörajoitukset vain valtuutettujen käyttäjien, palvelut ja sovellukset. Esimerkiksi erottaminen toisistaan alihallinnat, jotka on olennainen suojaus-järjestelmä multitenant cloud ympäristöjen, kuten Microsoft Azure. Loogisen eristystaso auttaa estämään yhdestä vuokraajasta häiritse muiden vuokraajan toimintoja.

#### <a name="environment-isolation"></a>Ympäristön eristystaso
Azure Government-ympäristö on fyysinen ‑esiintymä, jossa on erillinen muusta Microsoftin verkossa. Tämä saavutetaan on siirryttävä fyysistä ja loogista ohjausobjekteja, jotka ovat seuraavat:

- Suojaaminen fyysiset Biometrialaitteet ja kamerat.
- Käyttö tietyn tunnistetiedot ja monimenetelmäisen todentamisen Microsoftin henkilöstön tuotantoympäristössä looginen pääsyä.
- Kaikki service-infrastruktuurin Azure Government sijaitsee Yhdysvalloissa.

#### <a name="per-customer-isolation"></a>Asiakas eristystaso
Azure toteuttaa verkon käytönhallinta ja eriytymistä kautta VLAN eristystaso käyttöoikeusluettelot, lataa tasoitusmääritykset ja IP-suodattimet

Asiakkaat edelleen erottaa resursseille tilaukset, resurssiryhmiä, virtual verkkojen ja aliverkosta välillä.

## <a name="screening"></a>Seulonta

Äskettäin ilmoitettu FedRAMP suuri ja hyväksymisen Linnaa osaston (Yhdysvaltain puolustusministeriön käyttöön) vaikutus tasolle 4. Tämä on korotettuna tietosuojaan ja vaatimustenmukaisuuteen-palkin välillä Azure Government-ympäristössä.

On nyt ovat seulonta Microsoftin osoitteessa kansallinen virasto Tarkista operaattorit laki ja luottotietojen (NACLC), osan 5.6.2.2, Yhdysvaltain puolustusministeriön käyttöön Cloud tietojenkäsittely suojaus vaatimukset opas (SRG):

>[AZURE.NOTE] Pienin taustan tutkimuksen vaaditaan CSP henkilökunnan kirjautumisoikeuden tasolle 4 ja 5 tietojen perusteella "ei-kriittinen luottamukselliset" (esimerkiksi Yhdysvaltain puolustusministeriön käyttöön 's ADP-2) on kansallinen virasto tarkistuksen laki ja luottotietojen (NACLC) (, "ei-kriittinen luottamukselliset" alihankkijat) tai Keskitaso riskin taustan tutkimuksen (MBI), "Keskitaso riskin" sijainnin nimeäminen.

Seuraavassa taulukossa on yhteenveto sekä nykyisen seulonta Azure Government operaattoreita:

Azure gov – seulontajätteet ja taustan tarkistus | Kuvaus|
---|---|
Yhdysvaltojen kansalaisuus |Yhdysvaltojen kansalaisuus vahvistus.
Microsoft cloud taustan tarkistus (joka toinen vuosi)|Henkilötunnus haun rikosoikeudellisia historia-valintaruudun, Office ulkomaan resurssien hallinnan luettelo (OFAC), alan Bureau ja suojaus (a), Office, Linnaa kaupan ohjausobjektien Debarred henkilöiden luettelosta.
Kansallinen virasto valintaruudun laki ja luottotietojen (NACLC) (joka viiden vuosi) | Lisää tunnistetieto taustan tarkistus FBI: lle-tietokannoissa. Saat lisätietoja Siirry<a href="https://www.opm.gov/investigations/background-investigations/federal-investigations-notices/1997/fin97-02/"> Office henkilökunnan Management-sivustossa</a>. | 
<a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/CJIS">Rikosoikeudellisten Information Services (CJIS)</a> | CJIS on tila, paikallinen ja FBI: lle government seulonta mitkä prosessit-tunnisteen tietueiden ja vahvistaa rikosoikeudellisia hammashoitoa toiminnallisia henkilökunnalle, joilla voidaan säätää kriittinen rikosoikeudellisten tiedot (CJI) tietojen käytön käyttöön.  Osavaltioiden onko omia taustan valinta- ja kaikki henkilöt, joilla päästä CJI myöhemmin hyväksyminen.|

Accessin seuraavien periaatteiden koskevat henkilökunnan Azure toiminnot:

- Tehtävien on selkeästi määritellyt kanssa eri Yleiset tehtävät voidaan pyytää, hyväksyminen ja käyttöönotto muutokset.
- Access on määritetty liittymät, joissa on tiettyjä toimintoja.
- Accessin on juuri-in-time (JIT) ja myönnetään vain tapahtumaa kohti perusteella tai tietyn ylläpitotapahtuman ja aina rajoitetun ajan.
- Access on roolipohjainen, määritetty rooleihin, jotka on määritetty vain vianmääritys tarvittavista käyttöoikeuksista.

Seulonta standardeja ovat Yhdysvaltojen kansalaisuus kaikista Microsoft-tuen ja toiminnallisia henkilökunnan vahvistus ennen käytettävyys Azure Government isännöimä järjestelmiin. Tietojen siirtäminen käyttäville tukihenkilöstöön käyttöä suojatun Azure hallituksen sisältä. Tietojen suojaaminen siirto edellyttää joukko todentamisen tunnistetietoja saat käyttöösi. Jos haluat käyttää järjestelmän metatietoja, toimintojen henkilökunnan käyttää esimerkiksi tietyn verkkopohjaisia sisäinen hallintatyökalut, vain luku-ohjelmointirajapinnan ja JIT korkeus.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätiedot ja päivitykset Ota tilaa varten <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-blogi.</a>
