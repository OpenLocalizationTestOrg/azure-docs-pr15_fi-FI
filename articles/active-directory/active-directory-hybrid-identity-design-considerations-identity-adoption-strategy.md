<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat – Määritä hybrid tunnistetietojen käyttöönottostrategia | Microsoft Azure"
    description="Ohjausobjektin ehdollisen käyttöoikeuden kanssa Azure Active Directory tarkistaa tiettyjen ehtojen, voit valita, kun käyttäjän todentamista ja ennen kuin sallit sovelluksen käyttöä. Ehtojen täyttyessä, kun käyttäjä on todennus ja sovelluksen käyttöoikeus."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Määritä hybrid tunnistetietojen käyttöönottostrategian kehittäminen lynciä varten

Tässä tehtävässä määrität hybrid tunnistetietojen käyttöönottostrategia hybrid tunnistetietojen ratkaisu, joka on käsitellyt business vaatimukset:

- [Määrittää liiketoimintatarpeiden](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Kansion synkronoinnin vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Monimenetelmäisen todentamisen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Yrityksen tarpeet toimintamallin
Ensimmäinen tehtävä osoitteet määritettäessä organisaatiot business on.  Tämä voi olla hyvin laaja ja laajuus viruminen voi ilmetä, jos et ole varovainen.  Pyri yksinkertaisuuteen alkuun mutta muista aina liittyvää rakenne, joka mahtuu ja helpottaa muuta tulevaisuudessa.  Riippumatta siitä, onko se yksinkertainen malli tai erittäin monimutkainen tunnus Azure Active Directory on Microsoft Identity-ympäristö, joka tukee Office 365: ssä, Microsoft Online Services-palveluiden ja -pilvi huomioon sovellukset.

## <a name="define-an-integration-strategy"></a>Toimintamallin-integrointi
Microsoft on kolme tärkeimmät integrointi tilanteita, jotka ovat cloud käyttäjätietoja, synkronoituja käyttäjätietojen ja liitetyt käyttäjätietoja.  Valitse antamista jokin näistä strategiat kannattaa suunnitella.  Voit valita voivat vaihdella ja päätökset valitseminen voi olla minkä tyyppistä haluat antaa, sinulla on joitakin olemassa olevan infrastruktuurin jo luomista ja mikä eniten edullinen käyttäjäkokemuksen strategia.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Tilanteita, joissa on määritelty edellä olevassa kuvassa on:

- **Cloud käyttäjätietojen**: Nämä ovat käyttäjätietojen siellä olevia pelkästään pilvessä.  Azure AD-kyseessä ne sijaitsi erityisesti Azure AD-kansio.
- **Synchronized**: Nämä ovat käyttäjätietojen siellä olevia paikallisen ja pilveen.  Käytä Azure AD Connect, nämä käyttäjät luodaan tai liittyneet aiemmin Azure AD-tilien kanssa.  Käyttäjän salasanan hash on synkronoitu paikallisen ympäristön mitä kutsutaan salasanan hash pilveen.  Kun käyttämällä synkronoida yhden caveat on, jos käyttäjä on poistettu käytöstä paikallisen ympäristön, voi kestää jopa 3 tuntia kyseisen tilin tilan lisääminen Azure AD.  Tämä on synkronoinnin aikaväli.
- **Organisaation ulkopuolisten**: nämä käyttäjätietojen olemassa sekä paikallisen ja pilveen.  Käytä Azure AD Connect, nämä käyttäjät luodaan tai liittyneet aiemmin Azure AD-tilien kanssa.  
 
>[AZURE.NOTE]
Lisätietoja synkronoinnin asetukset lukea [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

Seuraavan taulukon avulla määrittämiseen hyötyjä ja haittoja kaikkien seuraavat strategiat:

| Määrittäminen         | Hyvät puolet                                                                                                                                                                                                                                                  | Huonot puolet                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Cloud käyttäjätietoja** | Pieni organisaation hallinta on helpompaa. <br> Ei mitään--paikallisen-ei tarvita erillistä laitetta asentaminen<br>Käytöstä helposti, jos käyttäjä jättää yrityksen                                                                                                   | Käyttäjät tarvitsevat sisään käytettäessä työmääriä pilveen <br> Salasanoja voi tai ei ehkä sama cloud ja paikallisen käyttäjätietojen                                                                                                                                                                                                                     |
| **Synkronoitu**     | Paikallisen salasana paikallisen sekä todennusta ja cloud hakemistojen selaaminen <br>Helpompi hallita pieni, Normaali tai suuri organisaatioissa <br>Käyttäjillä voi olla seuraavia resursseja kertakirjautuminen (SSO) <br> Microsoft ensisijainen tapa synkronointi <br> Helpompi hallita | Jotkin asiakkaat voivat olla haluttomia niiden kansioiden synkronointi pilveen tiettyyn yritykseen poliisi määräpäivä                                                                                                                                                                                                                                                  |
| **Organisaation ulkopuolinen**        | Käyttäjillä voi olla kertakirjautuminen (SSO) <br>Jos käyttäjä on lopetettu tai jättää, tiliä voi käytöstä välittömästi ja access kumottu<br> Tukee kehittyneitä skenaariot, ei voi, määrittämällä synkronoitu                                           | Asenna ja määritä useita vaiheita <br> Korkeampi ylläpito <br> Vaatia erillistä laitetta STS-infrastruktuuri <br> Asenna liittoutumispalvelimen laitteiston saattaa edellyttää. Lisäohjelmistoa vaaditaan, jos käytössä on AD FS <br> Vaadi SSO laaja asetukset <br> Kriittinen kohdan virheen, jos liitetty viestintä-palvelin ei ole käytettävissä, käyttäjät eivät voi enää tarkistamiseen |

### <a name="client-experience"></a>Asiakas-toiminto
Strategia, jota käytetään valinta määrää käyttäjän kirjautuminen.  Seuraavissa taulukoissa on lisätietoja mitä käyttäjien tulee odottaa kokemuksensa voidaan.  Huomaa, että kaikki liitetyt tunnistetietojen toimittajat tukevat SSO kaikissa tilanteissa.

**Doman liittyneet ja yksityiset verkkosovellukset**:
 

|                              | Synkronoidun tunnistetiedot      | Liitetyt tunnistetiedot                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Selaimet                 | Lomakepohjainen todennus | Kertakirjauksen edelleen, joskus tarvitaan Organisaatiotunnus |
| Outlookin                      | Tunnistetietojen kehote     | Tunnistetietojen kehote                                       |
| Skype for Business (Lync)    | Tunnistetietojen kehote     | Single-etumerkin Lync-tunnistetietoja pyydetään Exchange   |
| SkyDrive Pro                 | Tunnistetietojen kehote     | kertakirjautuminen                                               |
| Office Pro Plus-tilausta | Tunnistetietojen kehote     | kertakirjautuminen                                               |

**Ulkoisen tai epäluotettavana lähteistä**:

|                              | Synkronoidun tunnistetiedot      | Liitetyt tunnistetiedot                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Selaimet                 | Lomakepohjainen todennus |  Lomakepohjainen todennus |
| Outlook-Skype for Business (Lync), Skydrive Pro Office-tilaus| Tunnistetietojen kehote     | Tunnistetietojen kehote                                       |
| Exchange ActiveSync    | Tunnistetietojen kehote     | Single-etumerkin Lync-tunnistetietoja pyydetään Exchange   |
| Mobile-sovellukset                 | Tunnistetietojen kehote     | Tunnistetietojen kehote                                               |

Jos olet määrittänyt tehtävästä on 3, osapuolen IdP tai niiden siirtyä käyttämään antamaan n Azure AD yhdistämisessä 1, sinun on otettava huomioon seuraavat tuetut ominaisuudet:
- Minkä tahansa SAML 2.0-palvelun, joka on yhteensopiva SP Lite profiilin tukevat Azure AD-todennus ja siihen liittyvät sovellukset
- Tukee passiivista todennusta, joka helpottaa auth OWA, SPO jne.
- Exchange Onlinen asiakkaat voivat tuetaan kautta SAML 2.0 parannetun asiakkaan profiilin ECP

On myös oltava tietää, mitä ominaisuuksia ei ole käytettävissä:

- Ilman WS-luota/tukea muut aktiiviset asiakkaat vaihtuvat
 - Tämä tarkoittaa sitä, ei ole Lync-asiakasohjelma, asiakasohjelma, Office-tilaus, Office Mobile ennen Office 2016 OneDrive
- Siirtyminen Office passiivista todennusta ansiosta ne tue täysin SAML 2.0 IdPs, mutta tuki on asiakkaan asiakkaan välein


>[AZURE.NOTE]
Viimeksi päivitetty luettelon lukea artikkelin http://aka.ms/ssoproviders.

## <a name="define-synchronization-strategy"></a>Synkronoinnin toimintamallin
Voit määrittää työkaluja, joita käytetään synkronoimaan tehtävän organisaation paikalliset tiedot cloud ja topologian tulisi käyttää.  Koska useimmat organisaatiot käyttää Active Directory-osoitteen edellä esitettyihin kysymyksiin Azure AD Connect avulla tiedot on tarkoitettu joitakin yksityiskohtaiset tiedot.  Ympäristössä, joka ei ole Active Directory-on lisätietoja FIM 2010 R2 tai MIM 2016 sisällönjakomenetelmän tämän ohjeen avulla.  Kuitenkin Azure AD Connect tulevista versioista tukee LDAP hakemistojen selaaminen, niin aikajanan sen mukaan, näitä tietoja voi auttaa.

###<a name="synchronization-tools"></a>Synkronoinnin Työkalut
Vuosien synkronoinnin työkaluilla on olemassa ja käyttää eri tilanteista.  Azure AD Connect ei tällä hetkellä kaikki tuetut tilanteet työpöytäohjelmia Siirry.  AAD Sync ja DirSync on yhä ympärille ja jopa mahdollisesti ympäristössäsi nyt. 

>[AZURE.NOTE]
Kunkin työkalun uusimmat tiedot koskevat tuetut ominaisuudet- [hakemiston integrointi Työkalut vertailu](active-directory-hybrid-identity-design-considerations-tools-comparison.md) artikkeliin.  

### <a name="supported-topologies"></a>Topologioista
Synkronoinnin strategia määritettäessä topologian, jota käytetään on määritettävä. Sen mukaan, tiedot, jotka on määritelty vaiheessa 2 voit määrittää minkä topologian on oikea käyttämään. Yksittäisen metsää yksittäisen Azure AD-topologian on yleisin ja koostuu yksittäisen Active Directory-metsää ja yksi Azure AD-esiintymä.  Tämä voi käyttää useimpia käyttötavoista suorittaminen ja on odotettavissa topologian, kun Azure AD yhteyden pika-asennuksen avulla alla olevassa kuvassa osoitetulla tavalla.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Yhden metsää skenaario on suuri ja jopa pienen organisaatiot voivat on useita metsien yleisiä kuva 5 mukaisesti.

>[AZURE.NOTE]
Lisätietoja erilaisista paikallisista ja Azure AD-topologioissa kanssa Azure AD Connect synkronoinnin, tutustu artikkeliin [Azure AD Connect topologioissa](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Usean metsää-skenaario

Jos tämä tapauksessa valitse Avainhankkeiden monivaiheisen-forest-yksittäinen Azure AD-topologian voidaan pitää, jos seuraavat ehdot täyttyvät:

- Käyttäjillä on vain 1 käyttäjätietoja kaikkiin puuryhmissä – alla yksilöllisesti yksilöivä käyttäjät-osiossa kuvataan tämä tarkemmin.
- Käyttäjän todentaa, jossa jäsenyys sijaitsee metsää
- Tämä metsää tulevat täydellisen Käyttäjätunnuksen ja tietolähteen ankkurin (pysyvä tunnus)
- Kaikki metsien ovat käytettävissä Azure AD Connect – Tämä tarkoittaa sitä ei tarvitse olla toimialueen liittynyt ja voidaan sijoittaa DMZ Jos tämä helpottaa näin.
- Käyttäjillä on vain yksi postilaatikko
- Metsää käyttäjän postilaatikkoa isännöivän on paras tietojen laadun, määritteiden näkyvissä-Exchange yleinen osoite luettelo (GAL)
- Jos käyttäjä ei ole postilaatikon, valitse kaikki metsää voidaan käyttää osallistuja-arvot
- Jos sinulla on linkitetty postilaatikon, valitse on myös toisen tilin-eri metsää, jolla kirjaudutaan sisään.

>[AZURE.NOTE]
Objektit, jotka järjestelmässä sekä paikallisen ja pilveen "yhdistää" yksilöllinen kautta. Hakemistosynkronoinnin kontekstissa tämä yksilöllinen kutsutaan SourceAnchor. Kontekstissa, kertakirjautuminen tätä kutsutaan nimellä ImmutableId. Lisää huomioitavista SourceAnchor käyttöä koskevat [Azure AD Connect käsitteiden rakenne](active-directory-aadconnect-design-concepts.md#sourceanchor) .

Jos sinulla on useampi kuin yksi aktiivinen tili tai useamman kuin yhden postilaatikon edellä mainitut eivät ole tosi, Azure AD Connect jokin ja Ohita toiseen.  Jos olet linkittänyt postilaatikot, mutta muu tili, näissä tileissä ei viedä Azure AD ja käyttäjän eivät ole minkään ryhmän jäsen.  Tämä on erilainen kuin siitä, miten se on aiemman dirsyncillä ja onko tahallisten entistä paremmin tukea usean metsää tilanteista. Alla olevassa kuvassa näkyy tilanne usean metsää.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Usean metsää usean Azure AD-ympäristössä**

On suositeltavaa on vain yhden kansion Azure AD organisaation, mutta se on tuettu se 1:1-yhteys on käytettävissä Azure AD Connect-synkronoinnin server – Azure AD-kansio.  Jokaisessa Azure AD-esiintymässä on Azure AD Connect asennuksen.  Myös Azure AD-rakenne on erillään ja esiintymän Azure AD-käyttäjät eivät näe käyttäjien toisessa esiintymässä.

Se on mahdollista ja tuettujen yksi paikallisen Active Directory-esiintymä muodostaa useita Azure AD-kansioita alla olevassa kuvassa osoitetulla tavalla:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Yhden metsää suodatus-skenaario**

Jotta voit tehdä seuraavat pitävät paikkansa:

- Azure AD Connect synkronointi-palvelimet on määritettävä siten, että jokainen on toisensa poissulkevia objektijoukon suodattamista varten.  Tämä tekemän, esimerkiksi tietyn toimialueen tai OU kunkin palvelimen vaikutusalueen määrittäminen.
- DNS-toimialueen voidaan rekisteröidä vain yhdessä Azure Active directory jotta UPNs paikallisen käyttäjille AD on käytettävä erillistä nimitilan
- Esiintymän Azure AD-käyttäjät vain voi tarkastella niiden esiintymästä käyttäjiä.  He eivät näe käyttäjien muissa tapauksissa
- Vain yksi Azure AD-kansioiden käyttöön Exchange-hybridiratkaisuun, jossa on sekä paikallisia AD
- Takaisinkirjoituksen koskee myös keskinäistä merkkikohtaisia.  Näin joitakin takaisinkirjoituksen ominaisuuksia ei tueta tässä topologian kanssa, koska nämä oletetaan, että yhden paikallisen määritys.  Tämä vaihtoehto sisältää:
 - Takaisinkirjoituksen oletusasetuksen sisältävä ryhmä
 - Laitteen takaisinkirjoituksen


Huomioon, että seuraavat ei tue ja ei olisi valittava toteutus:

- Se ei ole tuettu on useita Azure AD Connect synkronointi palvelimia yhteyden samaan Azure AD-kansioon, myös silloin, kun ne on määritetty synkronoimaan objektin toisensa poissulkevia määrittäminen
- Se ei tue sama käyttäjä voi useita Azure AD-kansioiden synkronointi. 
- Se ei tue myös tehdä määritykset, jos haluat, että käyttäjät yksi Azure AD yhteystiedot toisesta Azure AD-luettelossa näkyy muuttaminen. 
- Kannattaa myös muokata muodostaa yhteyden useisiin Azure AD-kansioiden synkronointi Azure AD Connect ei tueta.
- Azure AD-kansiot ovat eristetty rakenne. Ei tueta voit muuttaa Azure AD Connect synkronointi voi lukea tietoja toiseen Azure AD-kansioon ja yrittää luoda yleisiä ja yhdistetty GAL kansioiden välillä määrityksiä. Se myös ei tue voit viedä yhteystiedot käyttäjien toiseen paikallisen AD käyttämällä Azure AD Connect Synkronoi.


>[AZURE.NOTE]
Jos organisaatiosi rajoittaa verkosta Internet-yhteyden, tämä artikkeli sisältää päätepisteet (täydelliset toimialuenimet, IPv4 ja IPv6-osoitealueet), Sisällytä lähtevien että Salli luettelot ja Internet Explorerin Luotetut sivustot-asiakasohjelman tietokoneiden voit varmistaa tietokoneiden onnistuneesti voi käyttää Office 365: ssä. Lue lisätietoja [Office 365: n URL-osoitteet ja IP-osoitealueet](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

## <a name="define-multi-factor-authentication-strategy"></a>Monimenetelmäisen todentamisen toimintamallin
Tässä tehtävässä voit määrittää monimenetelmäisen todentamisen määrittäminen käyttämään.  Azure Monimenetelmäisen todentamisen on kaksi eri versiota.  Yksi on pilvipohjainen ja toinen paikallisen mukaan Azure MFA palvelimen avulla.  Laskennan perusteella yläpuolella, voit voit määrittää, mikä ratkaisu on oikeat yrityksen määrittäminen.  Alla olevan taulukon avulla voit selvittää parhaan vaihtoehdon rakenne täyttävät yrityksen suojauksen vaatimus:

Multi-factor rakenteen asetukset:

| Jos haluat suojata resurssi                                               | MFA pilveen | MFA paikallinen |
|---------------------------------------------------------------|------------------|----------------|
| Microsoft-sovellukset                                                | Kyllä              | Kyllä            |
| SaaS sovellukset app-valikoimassa                                  | Kyllä              | Kyllä            |
| IIS-sovellukset, jotka on julkaistu Azure AD-sovelluksen välityspalvelimen kautta         | Kyllä              | Kyllä            |
| IIS-sovelluksia ei julkaista Azure AD-sovelluksen välityspalvelimen kautta | Ei               | Kyllä            |
| Etäkäyttö kuin VPN-RDG                                     | Ei               | Kyllä            |

Vaikka ehkä olet selvittänyt ratkaisua yrityksen määrittäminen varten, sinun on käyttää edellä arviointia, jossa käyttäjien sijaitsevat.  Tämä saattaa aiheuttaa ratkaisu, jos haluat muuttaa.  Käytä alla olevaa taulukkoa auttamaan määritettäessä näin:

| Käyttäjän sijainti                                                       | Ensisijainen rakenne                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Usean-FactorAuthentication pilveen |
| Azure AD ja paikallisen AD federation käyttäminen AD FS             | Molemmat                                    |
| Azure AD ja paikallisen AD käyttämällä Azure AD Connect ei ole salasanan synkronointi | Molemmat                                    |
| Azure AD ja paikallisen Azure AD Connect käyttämisestä salasanan synkronointi  | Molemmat                                    |
| Paikallisen AD                                                      | Monimenetelmäisen todentamisen Server      |

>[AZURE.NOTE]
Sinun tulee myös varmistaa, että monimenetelmäisen todentamisen rakenne-vaihtoehto, jonka valitsit tukee esimerkiksi seuraavia toimintoja, joita tarvitaan rakenteeseen.  Lue lisätietoja [multi-factor suojaus-ratkaisuun, valitse](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Multi-factor Auth toimittaja
Monimenetelmäisen todentamisen on oletusarvoisesti yleinen järjestelmänvalvojille, jotka on Azure Active Directory-vuokraajan. Jos haluat laajentaa monimenetelmäisen todentamisen kaikki käyttäjät ja/tai haluat Yleiset järjestelmänvalvojat voivat ottaa uusia ominaisuuksia, kuten hallinta-portaalin, mukautetut tervehdykset ja raportteja, valitse voit kuitenkin on ostaa ja multi-factor käyttöoikeustarkistuspalvelun asetusten määrittäminen.

>[AZURE.NOTE]
Sinun tulee myös varmistaa, että monimenetelmäisen todentamisen rakenne-vaihtoehto, jonka valitsit tukee esimerkiksi seuraavia toimintoja, joita tarvitaan rakenteeseen. 

##<a name="next-steps"></a>Seuraavat vaiheet
[Tietojen suojauksen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
