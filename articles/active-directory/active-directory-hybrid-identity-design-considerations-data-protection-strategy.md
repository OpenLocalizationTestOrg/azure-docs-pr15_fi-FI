<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - on tietojen suojauksen toimintamallin | Microsoft Azure"
    description="Määrität tietojen suojauksen strategia hybrid identity-ratkaisun business vaatimukset, jotka on määritetty."
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


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Tietojen suojauksen strategia hybrid identity-ratkaisun määrittäminen

Tässä tehtävässä määrität hybrid tunnistetietojen ratkaisu business vaatimukset, jotka on määritetty-tietojen suojauksen strategia:

- [Tietojen suojauksen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Sisällön hallinta vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Access-ohjausobjektin vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Tapauksen vastauksen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Tietoja salasanasuojauksen asetusten määrittäminen
Se on artikkelissa kuvatulla [määrittäminen directory-synkronoinnin vaatimukset](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD voi synkronoida tekemäsi Active Directory-toimialueen palveluista (AD DS) sijaitsee paikallisen. Integroinnin avulla organisaatiot voivat hyödyntää Azure AD vahvistamiseksi käyttäjän tunnistetiedot yrittäessäsi yrityksen resurssien käytön. Tämän voi tehdä sekä skenaarioissa: tietojen paikalliseen muille käyttäjille ja pilveen.  Tietojen käyttäminen Azure AD vaatii todennuksen suojauksen suojaustunnuksen palvelu (STS) kautta.

Kun todennus-käyttäjätunnus (UPN) luetaan todennus-tunnuksen ja replikoitua osio ja vastaa käyttäjän domain säilö määritetään. Tietoja käyttäjän olemassaolosta, käytössä-tila ja roolin käytetään luvan järjestelmä määrittääksesi, onko kohdevuokraajassa pyydetty käyttöoikeus oikeus kyseisen käyttäjän tässä istunnossa. Valtuutettu tiettyjen toimintojen (käyttäjän salasanan luominen tarkemmin sanottuna) luo kirjausketju, joka voidaan käyttää vuokraajan järjestelmänvalvoja yhteensopivuuden tehokkuutta tai tutkimuksia hallitsemiseen.

Liukuva yhteyttä paikalliseen palvelinkeskuksen Azure varastoon Internet-yhteyden kautta tietoja ei aina ole mahdollista vuoksi data-asema, kaistanleveyden käytettävyyttä tai muita huomioon otettavia seikkoja. [Tuo tai Vie Azure Tallennuspalvelu](../storage/storage-import-export-service.md) tarjoaa laitteisto-pohjainen asetus, sijoittaminen ja noudettaessa suurista tietomääristä Blob-objektien tallennustilaan. Voit lähettää [BitLocker salattujen](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) kiintolevyaseman levyasemien suoraan Azure palvelinkeskukseen, jossa cloud operaattorit Lataa sisältö tallennustilan tilin tai he voivat ladata Azure tietojen levyjen haluat palauttaa. Vain salattuja levyjä hyväksytään tämän prosessin (luomien palvelun itse työn asennuksen aikana BitLocker avaimen avulla). BitLocker-avain annetaan Azure erikseen, mikä antaa ulos Luokittele avaimen jakaminen.

Koska salataanko siirrettävät tiedot voidaan hallita eri skenaarioissa, on myös tärkeää tietää, että Microsoft Azure käyttää [virtual verkko](https://azure.microsoft.com/documentation/services/virtual-network/) eristää alihallinnat liikenne toisistaan, joissa toimenpiteitä kuten Host (isäntä)- ja Vieras-tason palomuurit, IP-pakettisuodatus, portin estäminen ja HTTPS päätepisteet. Useimmat Azure's sisäinen viestinnän, mukaan lukien infrastruktuurin infrastruktuuri ja infrastruktuurin asiakkaan (paikallisen), myös kuitenkin salattu. Toinen tärkeä skenaario on communications sisällä Azure palvelinkeskusten; Microsoft hallitsee verkkojen varmistaaksesi, että ei AM tekeytyminen tai eavesdrop toisen IP-osoite. TLS/SSL käytetään Azuren tallennustilaan tai SQL-tietokantoja käytettäessä tai muodostettaessa yhteyttä pilvipalveluihin. Tässä tapauksessa asiakas-järjestelmänvalvoja on vastuussa TLS/SSL-varmenteen hankkiminen ja niiden vuokraajan infrastruktuuri voidaan ottaa käyttöön. Tietoliikenne siirtäminen näennäiskoneiden saman käyttöönoton tai yksittäisen käyttöönoton Microsoft Azure Virtual verkon kautta vuokraajiin välillä voidaan suojata salattua yhteyttä-protokollia, kuten HTTPS tai SSL/TLS kautta.

Sinun pitäisi määrittää, kuinka haluat tietojen suojaaminen ja miten hybrid identity-ratkaisun auttaa sinua, valitse sen mukaan, miten vastata kysymyksiin [määrittäminen tietojen suojausvaatimukset](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md). Taulukossa näkyy kunkin tietojen suojauksen skenaarion käytettävissä olevat vaihtoehdot Azure tukemat.


| Tietojen suojauksen asetukset         | Milloin muiden pilveen | Muut tiloissa | Siirron aikana |
|---------------------------------|----------------------|---------------------|------------|
| BitLocker-salaus      | X                    | X                   |            |
| SQL Server-tietokannan salaaminen | X                    | X                   |            |
| AM AM salaus             |                      |                     | X          |
| SSL/TLS                         |                      |                     | X          |
| VPN                             |                      |                     | X          |


>[AZURE.NOTE]
Lue lisätietoja kaltaisilta, kunkin Azure-palvelu on standardien kanssa [Microsoft Azure-Valvontakeskus](https://azure.microsoft.com/support/trust-center/) [Yhteensopivuus-toiminto](https://azure.microsoft.com/support/trust-center/services/) .
Tietojen suojaaminen asetukset käyttää monikerroksisesta tapaa, koska vertailu näistä asetuksista eivät ole käytettävissä tämän tehtävän. Varmista, että osavaltioiden tiedot tulevat kaikki vaihtoehdot ovat hyödyntäminen.

## <a name="define-content-management-options"></a>Sisällön hallinta-asetusten määrittäminen
Azure AD avulla voit hallita hybrid identity-infrastruktuuria etu siitä, että prosessi on täysin läpinäkyvä käyttäjän näkökulmasta. Käyttäjä yrittää käyttää jaettuun resurssiin, resurssi edellyttää käyttöoikeuden tarkistusta, käyttäjä voi lähettää todennuspyynnön Azure AD tunnuksen hankkiminen ja käyttää resurssia. Päivitysprosessin tapahtuu taustalla ilman käyttäjän toimia. On myös mahdollista myöntää [ryhmän](active-directory-manage-groups.md#getting-started-with-access-management) , jotta he voivat suorittaa tiettyjä yleisiä toimintoja.

Organisaatiot, jotka koskevat tietojen tietosuoja tietoja yleensä Vaadi tietojen luokittelu niiden ratkaisu. Jos niiden nykyisen paikallisen infrastruktuurin on jo käytössä tietojen luokittelu, on mahdollista hyödyntää Azure AD tärkeimmät säilöön kuin käyttäjän käyttäjätiedot. Yleisiä työkalu, että se on käytetty paikallisen tietojen luokittelu kutsutaan [Tietojen luokittelu-työkalujen](https://msdn.microsoft.com/library/Hh204743.aspx) Windows Server 2012 R2. Tämä työkalu auttaa tunnistaa, luokitella ja tietojen yksityinen cloud palvelimissa tiedosto. On myös mahdollista hyödyntää Windows Server 2012: ssa voit tehdä tämän [Automaattinen tiedoston luokitus](https://technet.microsoft.com/library/hh831672.aspx) .

Jos organisaatiosi ei ole tietojen luokittelu paikassa, mutta se on suojaa luottamukselliset tiedostot lisäämättä uusi palvelinten paikallisen, he voivat käyttää Microsoft [Azure Rights Management-palvelu](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS käytetään salausta, käyttäjätiedot ja luvan käytännöt tiedostojen ja sähköpostin suojaaminen ja se toimii kaikissa useilla eri laitteilla, puhelimissa, tableteissa ja tietokoneissa. Koska Azure RIGHTS on pilvipalveluun, ei tarvitse erikseen määrittää luottamussuhteet muiden organisaatioiden kanssa, ennen kuin voit jakaa ne suojatun sisällön. Jos ne on jo Office 365- tai Azure AD-hakemistosta, yhteistyön organisaatioissa tuetaan automaattisesti. Voit myös synkronoida vain Azure RIGHTS on yleisiä tunnistetietojen tuki paikallisen Active Directory-tilit käyttämällä Azure Active Directory-synkronointipalvelut (AAD Sync) tai Azure AD Connect hakemiston määritteet.

Sisällönhallinta osa on ymmärtää, kuka on käyttää mitä resurssia, joten monipuolisia virheloki-ominaisuus on tärkeää identity management-ratkaisun. Azure AD on lokin yli 30 päivää, mukaan lukien:

- Roolien jäsenyyden muutokset (ex: Yleisen järjestelmänvalvojan rooliin lisättävä käyttäjä)
- Tunnistetietojen päivitykset (ex: salasanan)
- Domain management (ex: tarkistetaan mukautetun toimialueen, toimialueen poistaminen)
- Lisäämällä tai poistamalla sovelluksia
- Käyttäjien hallinta (ex: lisäämällä, poistamalla, käyttäjän päivitys)
- Lisäämällä tai poistamalla käyttöoikeuksia

>[AZURE.NOTE]
Lue [Microsoft Azure suojaus ja valvonta hallinta](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) lisätietoja kirjaamisen Azure-ominaisuudet.
Sen mukaan, miten vastata kysymyksiin [määrittäminen sisällönhallinta vaatimukset](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)sinun pitäisi määrittää, miten sisällön voi hallita hybrid identity-ratkaisussa. Kun kaikki vaihtoehdot tarjoamia taulukko 6 pystyvät ja Azure AD-on tärkeää määrittää, joka on enemmän sovellu yrityksesi tarpeisiin.

| Sisällön hallinta-asetukset                                                               | Hyvät puolet                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Huonot puolet                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Keskitetty paikallisen (Active Directory Rights Management Server)                      | Täydet oikeudet vastuussa luokittelussa tiedot server-infrastruktuuri <br> Sisäistä toimintoa Windows Server ei tarvita ylimääräistä käyttöoikeuden tai -Tilaustani? <br> Voit integroida Azure AD hybrid tilanne <br> Tukee tiedot sisältöoikeuksien hallinta (IRM) ominaisuuksia Microsoft Online Services-palveluissa, kuten Exchange Onlinen ja SharePoint Onlinen sekä Office 365: ssä <br> Tukee paikallisen Microsoft-palvelintuotteiden, kuten Exchange-palvelimeen, SharePoint Serverin ja tiedoston palvelimissa, jotka suoritetaan Windows Server ja tiedoston luokitus infrastruktuuri (FCI). | Suurempi, ylläpito (sido määrittäminen määritys-päivitykset ja mahdolliset päivitykset), koska se omistaa palvelimeen <br> Vaadi paikalliseen server-julkaisuinfrastruktuuri<br> Doesn'tleverage Azure ominaisuuksia grafiikkatiedostomuotoja                                     |
| Keskitetty pilvipalvelussa (Azure RMS)                                                     | Paikallisen ratkaisun helpompi hallita verrattuna <br> Voit integroida AD DS: ÄÄN hybrid tilanne <br>  Täysin integroitu Azure AD <br> Ei edellytä palvelimen paikallisen palvelun käyttöönotossa <br> Tukee paikallisen Microsoft-palvelintuotteiden, kuten Exchange Server, SharePoint Server tai tiedostopalvelimet, suorita Windows Server ja tiedoston luokitus-infrastruktuurin (FCI) <br> IT, voi olla käyttäjän vuokraajan avaimeen BYOK ominaisuuksien täydet oikeudet.                                                                                    | Organisaatiolla on oltava cloud tilauksessa, joka tukee RMS <br> Organisaatiolla on oltava Azure AD-kansion käyttöoikeuksien tuesta RMS                                                                                  |
| Hybridi (Azure RIGHTS integroitu, paikallisen Active Directory Rights Management Server) | Tässä skenaariossa kasvaa eduista sekä, keskitetty paikallisia ja pilveen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Organisaatiolla on oltava cloud tilauksessa, joka tukee RMS <br> Organisaatiolla on oltava Azure AD-kansion käyttöoikeuksien tuesta RMS, <br> Edellyttää Azure pilvipalvelussa välisen yhteyden ja paikallisen infrastruktuuri |

## <a name="define-access-control-options"></a>Määritä ohjausobjektin käyttöasetukset
Hyödyntäminen todennus-todennus ja Accessin ohjata toiminnot ovat käytettävissä Azure AD osaat käyttöön yrityksesi käyttää keskitetyn tunnistetietojen säilön samalla kun käyttäjät ja kumppanit käyttämään kertakirjautuminen (SSO) alla olevassa kuvassa osoitetulla tavalla:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Keskitetyn hallinnan ja täysin integrointi muihin kansioihin

Azure Active Directory tarjoaa kertakirjauksen tuhansia SaaS sovellukset käyttöön ja paikallisen verkkosovelluksia. Lue [Azure Active Directory federation yhteensopivuuden luettelon: kolmannen osapuolen palvelut, jotka voidaan ottaa käyttöön kertakirjautumisen](https://msdn.microsoft.com/library/azure/jj679342.aspx) artikkelissa lisätietoja SSO, kolmannen osapuolen, joka on testattu Microsoft. Tämä ominaisuus mahdollistaa organisaation toteuttamisesta B2B skenaarioita ja säilyttää ohjausobjektin tunnistetietojen ja käytön hallinta. Kuitenkin aikana B2B suunnitteleminen prosessi on tärkeää ymmärtää, jotka käyttävät kumppanin sekä Jos tämä menetelmä tukee Azure todentamismenetelmän. Nämä ovat tällä hetkellä tue Azure AD:

- Suojauksen vahvistus Markup Language (SAML)
- OAuth
- Kerberos
- Tunnusten
- Varmenteet


>[AZURE.NOTE] [Azure Active Directory käyttöoikeuksien protokollat](https://msdn.microsoft.com/library/azure/dn151124.aspx) tietää tarkempia tietoja kunkin protokolla ja sen Azure-ominaisuuksien lukeminen

Azure AD-tuki, mobile yrityssovellusten voit käyttää samaa helposti Mobile-palvelut-todennus kokemus työntekijät voivat kirjautua mobile verkkosovelluksistaan niiden yrityksen Active Directory-tunnuksilla. Tämän ominaisuuden Azure AD tuetaan tunnistetietojen toimittaja Mobile-palveluihin, kanssa muut tunnistetietojen toimittajat jo tuetaan (joka sisältää Microsoft Accounts, Facebook-tunnuksen, Google ja Twitter-tunnus). Jos paikallisen sovellusten käyttää käyttäjän tunnistetietojen osoitteessa yrityksen AD DS: ÄÄN, kumppaneiden ja käyttäjät, jotka tulevat pilveen käytöltä pitäisi olla läpinäkyvä. Voit hallita käyttäjän ehdollinen käytönvalvonta (pilvipohjainen) verkkosovellusten, verkko-Ohjelmointirajapinnan, Microsoftin pilvipalveluihin, 3 SaaS parannetulla ja alkuperäisen (mobiilisovelluksen)-sovelluksia ja on suojaus, edut, valvonta, raportointi yhdessä paikassa. On suositeltavaa tarkistaa tämän-tuotantoympäristössä tai rajoitetun käyttäjien kanssa.


>[AZURE.TIP] on tärkeää mainita, jotta Azure AD ei tarvita Ryhmäkäytäntö kuin AD DS: ÄÄN. Jotta voit pakottaa käytännön laitteiden on mobiililaitteen management-ratkaisun, kuten [Microsoft Intuneen](https://technet.microsoft.com/library/jj676587.aspx).

Kun käyttäjä todennetaan Azure AD-on tärkeää arvioida, että käyttäjällä on sen käyttöoikeustaso. Käyttöoikeustaso, käyttäjän nimen kohdalla näkyy resurssin voi vaihdella, kun Azure AD lisätä lisäsuojauksen kerroksen resurssien käytön hallinnasta, sinun on myös pitää mielessä, että itse resurssi voi olla myös omassa käyttöoikeusluettelo erikseen, kuten tiedostopalvelimeen sijaitsevien tiedostojen käyttöoikeuksien hallinta. Seuraavassa kuvassa on esitetty tasoja voi olla hybrid tilanne käyttöoikeuksien valvonta:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Kunkin vuorovaikutus osoittanut kuva x kaavion edustaa yhtä, joka voidaan kattaa Azure AD ohjausobjektin skenaario. Alla on kuvaus kutakin skenaariota:

1. ehdollinen Access-sovelluksia, jotka ovat isännöidään paikallisen: Voit käyttää rekisteröity laitteiden kanssa käytäntöjen sovelluksissa, jotka on määritetty käytettäväksi Windows Server 2012 R2 AD FS. Lisätietoja paikallisen ehdollisen käyttöoikeuden määrittämisestä on artikkelissa [paikallisen ehdollinen käytön Azure Active Directory laitteen rekisteröinti on määritetty](active-directory-conditional-access-on-premises-setup.md).
2. käyttää ohjausobjektin Azure hallinta-portaalin: Azure on myös ominaisuuksien hallinta-portaalin hallintaa käyttämällä RBAC (roolin perusteella käyttöoikeuksien valvonta). Tämä menetelmä mahdollistaa yrityksen rajoittaa toiminnoista, joita henkilö voi tehdä, kun hän on pääsy Azure hallinta-portaalin määrää. RBAC avulla voit hallita-portaaliin, IT-järjestelmänvalvojien ca edustajakäyttöoikeudet käyttämällä access hallinta seuraavilla tavoilla:

 - Ryhmä-pohjainen roolimääritys: Voit määrittää access Azure AD-ryhmiä, jotka on synkronoitu paikallisen Active Directory. Näin voit hyödyntää aiemmin sijoitukset, jotka organisaatiosi on määrittänyt sillä ja prosessien hallinnassa ryhmät. Voit käyttää myös Azure AD Premium valtuutetun ryhmän hallinta-toiminnolla.
 - Suorituskykykertoimen Azure-roolien hallinta: Voit käyttää kolme roolia, omistaja, avustaja ja Reader varmistaa, että käyttäjät ja ryhmät ovat vain ne tehtävät, ne on suoritettava työnsä oikeutta.
 - Hajautetun resurssien käytön: Voit määrittää roolit käyttäjät ja ryhmät-tilauksessa, resurssiryhmä tai yksittäisen Azure resurssin esimerkiksi tietokannan tai sivuston. Näin voit varmistaa, että käyttäjillä on käyttöoikeudet kaikille resursseille, jotka he tarvitsevat ja resurssit, joihin heillä ei olisikaan hallinta ei voi käyttää.

>[AZURE.NOTE] [Roolipohjainen käyttöoikeuksien valvonta Azure-tietokannassa](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) tietää tarkempia tietoja tästä ominaisuudesta luku. Sovelluskehittäjille, jotka ovat sovellusten luominen ja mukauttaminen niiden käyttöoikeudet on myös mahdollista käyttää Azure AD-sovelluksen roolit luvan. Tarkista tässä [Web App-sovelluksen RoleClaims-DotNet Esimerkki](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) siitä, miten voit luoda sovelluksen voit käyttää tätä ominaisuutta.

3 Office 365-sovellusten ehdollinen käytön Microsoft Intune: IT-järjestelmänvalvojat voivat valmistella ehdollisen käyttöoikeuden laitteen käytännöt suojaamiseen yritysresurssit, kun samanaikaisesti salliminen tietotyöntekijät yhteensopiva laitteissa voit käyttää palveluita. Lisätietoja on artikkelissa [Ehdollinen käytön laitteen käytännöt Office 365-palveluissa](active-directory-conditional-access-device-policies.md).

4 ehdollisen Saas-sovellusten käyttö: [tämän ominaisuuden](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) avulla voit määrittää sovelluksen kohti monimenetelmäisen todentamisen käyttösäännöt ja estää ei-luotettujen verkon käyttäjien mahdollisuus. Voit käyttää monimenetelmäisen todentamisen säännöt kaikille käyttäjille, jotka on määritetty sovellukseen tai vain ryhmän suojausryhmien. Käyttäjät voidaan jättää ulkopuolelle monimenetelmäisen todentamisen vaatimus, jos ne ovat käyttäminen sovelluksen IP-osoite-organisaation sisällä verkko.

Koska käyttöoikeuksien valvonta asetusten käyttäminen monikerroksisesta lähestymistapa, vertailu näistä asetuksista eivät ole käytettävissä tämän tehtävän. Varmista, että kaikki vaihtoehdot kunkin tilanne, jossa edellyttää, joiden avulla voit hallita resurssien käytettävissä ovat hyödyntäminen.

## <a name="define-incident-response-options"></a>Määritä tapauksen Kokousvastaus-kohdan asetukset
Azure AD auttaa IT-tunnistetietojen mahdollisia tietoturvauhkia ympäristössä mukaan käyttäjän valvominen, se voi hyödyntää Azure AD Access ja käyttöraportit ominaisuuksien ja myönnä näkyvyys eheys ja organisaation hakemistoon turvallisuus. Nämä ohjeet järjestelmänvalvoja paremmin selviää missä mahdollisia tietoturvauhkia saattaa ovat niin, että ne suunnittelevat riittävästi pienentämään näitä riskejä.  [Azure AD Premium-tilaus](active-directory-get-started-premium.md) on joukko suojaus-raportteja, jotka voit ottaa käyttöön IT, voit hankkia nämä tiedot. [Azure AD-raporttien](active-directory-view-access-usage-reports.md) luokitellaan alla kuvatulla tavalla:

- **Poikkeavuuksista raportit**: sisältävät tapahtumia, jotka on erheellisiin löydetyillä sisäänkirjautuminen. Tavoitteenamme on sinut tietoinen tällaisen toiminnan ja, joiden avulla voit tehdä siitä, onko tapahtuman epäilyttävistä määrittäminen.
- **Integroitu sovellus raportin**: antaa kyselyjä miten cloud sovellusten käytetään organisaatiossa. Azure Active Directory on tuhansia cloud sovellusten integrointi.
- **Virheraportit**: Ilmaise virheet, joita voi ilmetä valmistelu ulkoisiin sovelluksiin tilit.
- **Käyttäjän määrittämä raportit**: Näyttää tietyn käyttäjän tehtävätietojen laitteen ja kirjaudu.
- **Lokeja**: sisältää kaikki valvottuja tapahtumia viimeisen 24 tuntia, viimeisen seitsemän päivän tai viimeisten 30 päivän aikana, sekä ryhmän toiminnan muutokset ja salasanan palauttaminen ja rekisteröintiä tehtävän.

>[AZURE.TIP]
Toisen raportin, joka auttaa myös tapauksen parissa tapahtumaa vastauksen ryhmän on [käyttäjän vuodetun tunnuksilla](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) raportti.  Tämä raportti pintoja vastaavat nämä vuodetun tunnistetiedot-luettelossa ja alihallintaan välillä.

Muita tärkeitä Azure AD tapauksen vastauksen tutkimuksen aikana käyttämä raporttien hallinta ja ovat:

- **Salasanan tehtävän**: järjestelmänvalvoja antaa tietoja kyselyjä miten aktiivisesti salasanan on käytössä organisaatiossa.
- **Salasanan rekisteröinti tehtävän**: sisältää tiedot, johon käyttäjät on rekisteröity niiden salasanan, menetelmät ja mitä menetelmiä niiden on valittuna.
- **Ryhmätehtävän**: tarjoaa muutoslokin ryhmään (ex: käyttäjät lisätään tai poistetaan), jotka on aloitettu Access-ruudussa.

Lisäksi core raportoinnin mahdollisuutta käyttää Azure AD-Premium, joka voi olla hyödyntää aikana tapahtumaa vastauksen tutkimuksen IT voi myös hyödyntää valvontaraportin hankkimaan tietoja seuraavasti:

- Roolien jäsenyyden muutokset (ex: Yleisen järjestelmänvalvojan rooliin lisättävä käyttäjä)
- Tunnistetietojen päivitykset (ex: salasanan)
- Domain management (ex: tarkistetaan mukautetun toimialueen, toimialueen poistaminen)
- Lisäämällä tai poistamalla sovelluksia
- Käyttäjien hallinta (ex: lisäämällä, poistamalla, käyttäjän päivitys)
- Lisäämällä tai poistamalla käyttöoikeuksia

Koska tapauksen vastauksen asetukset käyttöön monikerroksisesta lähestymistapa vertailu näistä asetuksista eivät ole käytettävissä tämän tehtävän. Varmista, että kunkin skenaarion, joka edellyttää käyttämällä raportoinnin Azure AD-toimintoa yrityksen tapauksen vastauksen yhteydessä kaikki vaihtoehdot ovat hyödyntäminen.

## <a name="next-steps"></a>Seuraavat vaiheet
[Määrittää hybrid käyttäjätietojen hallintatehtävät](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
