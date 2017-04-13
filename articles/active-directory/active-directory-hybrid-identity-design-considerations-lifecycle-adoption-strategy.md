
<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - toimintamallin hybrid tunnistetietojen elinkaari käyttöönoton | Microsoft Azure"
    description="Avulla määrittää hybrid tunnistetietojen hallintatehtäviä vaihtoehdot elinkaari kunkin vaiheen mukaan."
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


# <a name="determine-hybrid-identity-lifecycle-adoption-strategy"></a>Hybrid tunnistetietojen elinkaari käyttöönoton määrittäminen
Tässä tehtävässä määrität käyttäjätietojen hallintastrategia hybrid identity-ratkaisun business vaatimukset, jotka olet määrittänyt [määrittäminen hybrid tunnistetietojen hallintatehtäviä](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md).


Määritä hybrid tunnistetietojen hallintatehtäviä mukaisesti esitetty aiemmin tässä vaiheessa lopusta loppuun-tunnistetietojen elinkaari on harkitse elinkaari kunkin vaiheen käytettävissä olevat vaihtoehdot.

## <a name="access-management-and-provisioning"></a>Käyttöoikeuksien hallinnan ja valmistelu
Hyvä tilin access management-ratkaisun organisaation voit seurata tarkasti kenellä on oikeus mitä tietoja koko organisaation.

Käyttöoikeuksien hallinta on keskitetty, yhden pisteen valmistelu järjestelmän kriittinen funktio. Lisäksi luottamuksellisten tietojen suojaaminen käyttöoikeuksien näyttää aiemmin luotujen tilien, joilla hyväksymistä odottavat lupa tai, eivät enää tarvitse. Voit hallita vanhentuneen tilien valmistelu järjestelmän linkit yhdessä tilin tiedot tilit omistavat käyttäjät tärkeitä tietoja. Tärkeimpien käyttäjän tunnistetiedot säilytetään yleensä tietokantojen ja hakemistojen henkilöresursseja.

Tyylikkään IT yrityksille-tilit ovat satoja viranomaisten määrittävät parametrit ja nämä tiedot voivat valvoa valmistelu järjestelmässä. Uudet käyttäjät voidaan tunnistaa tiedoilla, jotka tarjoavat tärkeimpien lähteestä. Accessin pyynnön hyväksymisen ominaisuuksien käynnistää prosesseja, jotka hyväksyä (tai hylätä) valmistelu niiden resurssi.


| Elinkaari hallinta vaihe          | Paikallinen                                                                                                                                                                                                                                                       | Cloud                                                                                                                                                                                                                                                                                                                     | Hybrid                                                                                   |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Tilinhallinta ja valmistelu | ® Active Directory-toimialueen palveluiden (AD DS) rooli avulla voit luoda skaalattava, turvallinen ja helpommin hallittaviin infrastruktuurin käyttäjien ja Resurssienhallinta ja directory käyttävistä sovelluksista, kuten Microsoft® Exchange Server tukevat. <br><br> [Voit valmistella ryhmien AD DS: n kautta käyttäjätietojen hallinta](https://technet.microsoft.com/library/ff686261.aspx) <br>[Voit valmistella AD DS: n käyttäjille](https://technet.microsoft.com/library/ff686263.aspx) <br><br> Järjestelmänvalvojat voivat käyttää käytönvalvonta tietoturvasyistä jaettujen resurssien käyttäjien käyttöoikeuksien hallinta. Active Directory käyttöoikeuksien valvonta hallitaan tasolla mukaan eri tasoilla määrittäminen Accessin tai käyttöoikeudet-objekteja, kuten täydet oikeudet-, kirjoitus-, luku- tai ei käyttöoikeutta. Käyttöoikeuksien hallinta Active Directoryn määrittää miten eri käyttäjät voivat käyttää Active Directory-objekteja. Oletusarvon mukaan Active Directoryn objektien käyttöoikeudet on määritetty turvallisin asetus. | Sinun on luotava jokaiselle käyttäjälle, joka käyttää Microsoft-pilvipalvelussa tili. Voit myös muuttaa käyttäjätilit tai poistaa niitä, kun olet ei enää tarvita. Oletusarvon mukaan käyttäjillä ei ole järjestelmänvalvojan oikeudet, mutta voit halutessasi määrittää niitä. Lisätietoja on artikkelissa [Azure AD käyttäjien hallinta](active-directory-create-users.md). <br><br> Azure Active Directory-yksi tärkeimmistä ominaisuuksista on mahdollisuus resurssien käyttöoikeuksien hallinta. Nämä resurssit voivat kuulua hakemiston, kuten käyttöoikeuksia objektit-kansiossa tai hakemiston, kuten SaaS sovellukset ja SharePoint-sivustojen Azure palvelujen tai paikalliseen resurssien ulkopuolisiin resursseihin rooleilla kirjainkokoa. <br><br> Keskitä Azure Active Directoryn access hallintaratkaisu on käyttöoikeusryhmän. Resurssien omistajan (tai järjestelmänvalvoja hakemiston) määrittää ryhmän tiettyjen käyttää oikealta ne ole resursseja. Ryhmän jäsenet antaa käyttöoikeuden ja resurssin omistaja voi delegoida oikeuden hallita toiselle henkilölle – kuten osaston esimies tai tukipalvelu järjestelmänvalvojan ryhmän jäsenet-luettelo<br> <br> Azure AD-artikkelin hallinta-ryhmiä on lisätietoa ryhmien käyttöoikeuksien hallinta.| Laajentaa Active Directory-käyttäjätietojen synkronointi ja federation pilveen |

## <a name="role-based-access-control"></a>Roolipohjainen käyttöoikeuksien valvonta
Roolipohjainen käyttöoikeuksien hallinta (RBAC) käyttää roolit ja valmistelu käytäntöjä, jos haluat käsitellä, Testaa ja liiketoimintaprosessien ja käyttöoikeuksien myöntäminen käyttäjille säännöt. Avaimen Järjestelmänvalvojat luovat valmistelu käytännöt ja määritettävä käyttäjille roolit ja, joka määrittää oikeuksista rooli resursseja. RBAC laajentaa ohjelmiston perustuva prosessit ja vähentää manuaalinen vuorovaikutteisuuden valmistelun prosessin identity management-ratkaisun.
Azure AD-RBAC mahdollistaa yrityksen rajoittaa toiminnoista, joita henkilö voi tehdä, kun hän on pääsy Azure hallinta-portaalin määrää. RBAC avulla voit hallita-portaaliin, IT-järjestelmänvalvojien ca edustajakäyttöoikeudet käyttämällä access hallinta seuraavilla tavoilla:

- **Ryhmä-pohjainen roolimääritys**: Voit määrittää access Azure AD-ryhmiä, jotka on synkronoitu paikallisen Active Directory. Näin voit hyödyntää aiemmin sijoitukset, jotka organisaatiosi on määrittänyt sillä ja prosessien hallinnassa ryhmät. Voit käyttää myös Azure AD Premium valtuutetun ryhmän hallinta-toiminnolla.
- **Suorituskykykertoimen Azure-roolien hallinta**: Voit käyttää kolme roolia, omistaja, avustaja ja Reader varmistaa, että käyttäjät ja ryhmät ovat vain ne tehtävät, ne on suoritettava työnsä oikeutta.
- **Resurssien käytön jauhe**: Voit määrittää roolit käyttäjät ja ryhmät-tilauksessa, resurssiryhmä tai yksittäisen Azure resurssin esimerkiksi tietokannan tai sivuston. Näin voit varmistaa, että käyttäjillä on käyttöoikeudet kaikille resursseille, jotka he tarvitsevat ja resurssit, joihin heillä ei olisikaan hallinta ei voi käyttää.

## <a name="provisioning-and-other-customization-options"></a>Valmistelu ja muut mukautusasetukset
Ryhmän voit käyttää business-Palvelupaketit ja vaatimukset päättämään, kuinka paljon identity-Ratkaisun mukauttamiseen. Esimerkiksi suuren yrityksen saattaa edellyttää vaiheittaisen lisensointi suunnitelma työnkulut ja sovittimia, joka perustuu aikajana valmisteluun vaiheittainen sovellukset, joita käytetään yleisesti paikkojen yli. Kahden tai useamman sovellusten jakaminen koko organisaation onnistuneen testauksen jälkeen antaa toisen mukauttaminen suunnitelman. Käyttäjä-sovelluksen vuorovaikutus voi mukauttaa ja ohjeet valmistelu resursseja voidaan muuttaa sopimaan automaattinen valmistelu.

Jos haluat poistaa palvelun tai osan voit deprovision. Esimerkiksi valmistelun tilin poistaminen tarkoittaa, että tili poistetaan resurssin.

Hybrid mallin valmistelu resursseja yhdistää pyynnön ja Roolipohjainen tavoista, jossa tuetaan Azure AD mukaan. Työntekijöiden tai hallittujen järjestelmien osajoukko-yrityksen halutessasi voit automatisoida tehtävän Roolipohjainen käytön. Muut käyttöoikeuspyyntöjen tai poikkeukset pyynnön-mallin avulla voi myös käsitteleminen yrityksen. Jotkin yritykset voi alkaa Manuaalinen määritys ja kehityttävä kohti hybrid-malli täysin Roolipohjainen käytön myöhemmin aiot kanssa.

Muiden yritysten voi etsiä hankalaa liiketoimintaa saavuttamiseksi valmis Roolipohjainen valmistelu ja kohdistaa hybrid-menetelmän osoittava tavoitteena. Edelleen muissa yrityksissä voi olla tyytyväinen vain pyynnön perustuva valmistelu, ja haluat sijoittaa muita työtä Roolipohjainen, automaattisten valmistelun käytäntöjen määrittäminen ja hallinta.

## <a name="license-management"></a>Käyttöoikeuksien hallinta
Ryhmä-pohjainen käyttöoikeuksien hallinta Azure AD-järjestelmänvalvojat voivat määrittää käyttäjiä käyttöoikeusryhmän ja Azure AD määrittää käyttöoikeudet automaattisesti kaikille ryhmän jäsenille. Jos käyttäjä on myöhemmin lisätään tai poistetaan ryhmästä, käyttöoikeuden määritetään automaattisesti tai poistetaan tarvittaessa.

Voit käyttää ryhmiä synkronoit paikallisen AD tai hallita Azure AD. Laiteparin tämä Azure AD-premium omatoimisen Group Management kanssa voit helposti delegoida tarvittavat päätöksentekijöille käyttöoikeusmääritykset. Muista, että ongelmien ratkaiseminen ja puuttuvat sijainnin tiedot lajitellaan automaattisesti.

## <a name="self-regulating-user-administration"></a>Itse sääntelytoimenpiteet käyttäjien hallinta
Kun organisaation Varaa resurssit sisäinen organisaatioissa, voit ottaa itse sääntelytoimenpiteet käyttäjän hallinta-ominaisuus. Voit ehkäistä eduista ja valmistelu käyttäjien edut organisaation rajojen. Tässä ympäristössä käyttäjän tila muuttuu automaattisesti näkyy käyttöoikeudet organisaation rajat ja paikkojen. Voit pienentää valmistelun kustannuksia ja nopeuttaa access ja hyväksyntä-prosesseja. Käyttöönoton toteuttaa hyväksi käyttöönoton Roolipohjainen käyttöoikeuksien valvonta-kattavan käyttöoikeushallinta organisaatiossa. Voit pienentää kustannuksia automaattinen ohjeet koskevat käyttäjän valmistelu kautta. Voit parantaa suojausta automatisoimalla suojauksen käytännön käyttäminen- ja nopeuttaa ja keskittää käyttäjän elinkaaren hallinta ja resurssin valmistelu suuri käyttäjän populaatio.

>[AZURE.NOTE]
Lisätietoja on artikkelissa Azure AD itse sovelluksen access hallinnan määrittäminen

Käyttöoikeus-pohjainen (oikeuden-pohjainen) Azure AD services työn aktivoimalla Azure Active directory-palvelun vuokraajan tilauksen. Kun tilaus on aktiivinen palvelun-ominaisuuksia voi hallita directory-palvelun Järjestelmänvalvojat ja käyttöoikeus käyttäjät käyttävät. Lisätietoja on artikkelissa mistä Azure AD käyttöoikeudet työn?
3 muiden toimittajien integrointi

Azure Active Directory tarjoaa single sign ja parannetun sovelluksen access-suojauksen tuhansia SaaS sovellukset ja paikallisen web-sovellusten avulla. Yksityiskohtainen luettelo tuetuista SaaS sovellusten Azure Active Directory sovelluksen valikoima artikkelissa Azure Active Directory federation yhteensopivuuden luettelon: kolmannen osapuolen palvelut, jotka voidaan ottaa käyttöön kertakirjautumisen

## <a name="define-synchronization-management"></a>Määritä synkronoinnin hallinta
Paikallisen kansioiden integraation Azure AD mahdollistaa käyttäjien tuottavuutta tarjoaa yleisiä tunnistetietojen käyttämiseen cloud ja paikallisen resurssit. Tämä integrointi käyttäjät ja organisaatiot voivat hyödyntää seuraavasti:

- Organisaatioiden tarjota käyttäjät, joilla on yhteiset hybrid tunnistetietojen paikallisen tai pilvipohjaisia palveluja hyödyntäminen Windows Server Active Directory ja Azure Active Directory yhdistäminen yli.
- Järjestelmänvalvojat voivat antaa ehdollinen access-sovelluksen resurssi, laite ja käyttäjätiedot, verkkosijainti ja monimenetelmäisen todentamisen perusteella.
- Käyttäjät voidaan hyödyntää yleisiä jäsenyys tilien kautta Azure AD Office 365: een, Intune-SaaS sovellukset ja kolmansien osapuolien sovellukset.
- Kehittäjät voit luoda sovelluksia, jotka hyödyntää yleisiä identity-mallin, sovellusten integroimisen Active Directory paikallisen tai Azure pilvipohjainen sovellusten

Seuraavassa kuvassa on esimerkki tunnistetietojen synkronointiprosessia ylätason näkymän.

![](./media/hybrid-id-design-considerations/identitysync.png)

Käyttäjätietojen synkronointi

Tarkista vertaileminen synkronointiasetusten seuraavassa taulukossa:

| Synkronoinnin hallinta-vaihtoehto          | Hyvät puolet                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Huonot puolet                                                                                                                                                                                                                                                                                  |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Synkronoi-pohjainen (kautta Dirsync-työkalun tai AADConnect) | Käyttäjät ja ryhmät, jotka synkronoidaan paikallisen ja pilveen <br>  **Käytännön ohjausobjektin**: tilin käytännöt voidaan määrittää kautta Active Directoryssa, jotka antavat järjestelmänvalvoja voi hallita salasanakäytännöt, työasema, rajoitukset, lukitus ohjausobjektit, sekä muita tietoja tarvitsematta lisätoiminnot pilveen.  <br>  **Käyttöoikeuksien hallinta**: Voit rajoittaa käyttöä pilvipalvelussa, niin, että palveluja voi käyttää yritysverkkoa, online-palvelimet tai molemmat. <br>  Rajoitetun tuotetukea koskevia puheluita: Jos käyttäjiä on vähemmän salasanoja muistaa, ne on epätodennäköistä unohtaa niitä. <br>  Suojaus: Käyttäjätietojen ja tiedot on suojattu, koska kaikki palvelimet ja käyttää single Sign-palveluiden selvittänyt ja hallita paikallisen. <br>  Vahvan todentamisen tuki: Voit käyttää vahva todennus (kutsutaan myös kaksiosainen todentamismenetelmä) pilvipalvelussa. Jos käytät vahva todennus, voit kuitenkin on käytettävä kertakirjautumisen. |                                                                                                                                                                                                                                                                                                |
| Liitetty viestintä-pohjainen (kautta AD FS)           | Ottaa käyttöön suojauksen suojaustunnuksen Service (STS). Kun määrität STS antamaan Sign access Microsoftin pilvipalvelussa, luot liitetyt luota paikallisen STS ja olet määrittänyt Azure AD-vuokraajan liitetyssä toimialueessa välillä. <br> Avulla käyttäjät voivat käyttää samoja tunnistetietoja useita resurssien käyttöoikeuksien hankkiminen <br>Loppukäyttäjät ei tarvitse säilyttää useita ehtojoukkoja ja tunnistetiedot. Vielä, käyttäjien on annettava tunnistetietoja yksitellen osallistuvien resurssien., tuettu B2B ja B2C skenaarioita.                                                                                                                                                                                                                                                                                                                                                                                                             | Edellyttää erityistä henkilökunnan käyttöönoton ja oma paikallinen säilyttäminen AD FS-palvelimiin. Ei rajoituksia vahva todennus käyttö Jos aiot käyttää AD FS oman STS. Lisätietoja on artikkelissa [Määrittäminen AD FS 2.0 Lisäasetukset](http://go.microsoft.com/fwlink/?linkid=235649). |

>[AZURE.NOTE]
Lisätietoja on artikkelissa- [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).


## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
