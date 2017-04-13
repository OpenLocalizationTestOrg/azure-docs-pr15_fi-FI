<properties
    pageTitle="Azure tallennustilan opas | Microsoft Azure"
    description="Tietojen suojaaminen Azure-tallennustilan, mukaan lukien rajoituksetta RBAC, tallennustilan palvelun salausta, asiakaspuolen salausta, SMB 3.0 ja Azure salauksen useita maksutapoja."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="robinsh"/>

#<a name="azure-storage-security-guide"></a>Azure tallennustilan opas

##<a name="overview"></a>Yleiskatsaus

Azuren tallennustila on kattavaa tietoturvaominaisuudet, jotka mahdollistavat yhdessä kehittäjät voivat luoda suojatun sovelluksia. Tallennustilan tilin itse voidaan suojata Roolipohjainen käyttöoikeuksien valvonta ja Azure Active Directoryn. Tietoja voi suojata siirron sovelluksen ja Azure välillä käyttämällä [Asiakaspuolen salausta](storage-client-side-encryption.md), HTTPS tai SMB 3.0. Tietoja voidaan määrittää salata automaattisesti, kun kirjoitettu Azuren tallennustilaan [Tallennustilan palvelun salaus (SSE)](storage-service-encryption.md)avulla. OS ja tietojen levyjen näennäiskoneiden käyttämä voi määrittää salata [Azure salauksen](../security/azure-security-disk-encryption.md). Tieto-objektit Azuren tallennustilaan valtuutetun pääsy voi myöntää [Jaettu Access allekirjoitusten](storage-dotnet-shared-access-signature-part-1.md)käyttäminen.

Tässä artikkelissa yleiskatsauksen kaikkien näiden suojausominaisuuksia, jotka voidaan käyttää Azure-tallennustilan. Linkit toimitetaan artikkeleihin, joissa tietoja siitä, että kullekin ominaisuudelle niin, voit helposti tehdä edelleen tutkimuksen aihe.

Seuraavassa on kattaa tämän artikkelin ohjeita:

-   [Hallinnan tasossa suojaus](#management-plane-security) – tallennustilan tilin suojaaminen

    Hallinnan tasossa koostuu resursseja tallennustilan tilin hallinta. Tässä osassa käsittelemme Azure resurssien hallinnan käyttöönottomalli ja Roolipohjainen käyttöoikeuksien valvonta (RBAC) avulla voit hallita tallennustilan tilit. Käsittelemme myös tallennustilan tilin avaimien ja miten ne uudelleen.

-   [Tietoja vertailutaso suojaus](#data-plane-security) – käytön tietojen suojaamisesta

    Tässä osiossa tarkastellaan salliminen todellisten tietojen-objektien tallennustilaan tiliisi, kuten BLOB-objektit, tiedostot tai olevien taulukoiden jaettu Access allekirjoitukset ja tallennettu käytäntöjen avulla. Käsiteltävät aiheet palvelun tason Suojaussidosten ja tilin tason Suojaussidokset. Olemme näkyy myös poistamisesta rajoittaa IP-osoite (tai IP-osoitteiden alue), rajoittamisesta HTTPS protokolla ja kumota jaettu Access allekirjoituksen odottamatta se päättyy.

-   [Siirron salaus](#encryption-in-transit)

    Tässä osassa käsitellään tietojen suojaamisesta, kun siirrät siihen sisään tai ulos Azure-tallennustilan. Käsittelemme HTTPS ja käyttää SMB 3.0 Azure tiedostoresurssit salaus suositellut käytön. On myös siirtää katsaus asiakaspuolen salausta, joiden avulla voit salata tiedot, ennen kuin se on siirretty asiakassovellus varastoon ja salauksen tiedot, kun se siirretään loppunut.

-   [Salauksen Rest-palvelussa](#encryption-at-rest)

    Käsittelemme tallennustilan palvelun salaus (SSE) ja miten voit ottaa sen käyttöön tallennustilan tilin tuloksena on estä BLOB-objektit, sivu-BLOB- ja liitä BLOB parhaillaan salattuja automaattisesti, kun kirjoitettu Azure-tallennustilan. On myös näyttää, miten voit käyttää Azure salauksen ja tutustu basic erot ja SSE ja asiakkaan salaus ja salauksen tapauksissa. Olemme näyttää lyhyesti osoitteessa FIPS yhteensopivuuden US Government-tietokoneissa.

-   [Tallennustilan analyysin](#storage-analytics) avulla voit valvoa Azure-tallennustilan

    Tässä osassa kerrotaan, miten voit etsiä tietoja tallennustilan analyysitiedot lokit-pyynnössä. Syy Katso reaali tallennustilan analytics lokitiedot ja katso, miten voit discern onko pyyntö tehdään tallennustilan tilin avaimella, ja jakaa Access-allekirjoitus tai nimettömänä ja onko sen onnistui tai epäonnistui.

-   [Selainpohjainen asiakkaiden käyttämällä CORS ottaminen käyttöön](#Cross-Origin-Resource-Sharing-CORS)

    Tässä osassa puheen siitä, miten voit sallia usean origin resurssien jakaminen (CORS). Käsittelemme-toimialueiden käyttäminen ja käsittelemisestä Azuren tallennustilaan sisäänrakennettu CORS ominaisuuksia.

##<a name="management-plane-security"></a>Hallinnan tasossa suojaus

Hallinnan tasossa koostuu toimintoja, jotka vaikuttavat itse tallennustilan tiliä. Voit esimerkiksi luoda tai poistaa tallennustilan tilin, hankkia tallennustilan tililuettelon tilauksen, noutaa tallennustilan tilin näppäimet tai tallennustilan tilin näppäimet uudelleen.

Kun luot uuden tallennustilan tilin, voit valita käyttöönoton mallin perinteinen tai Resurssienhallinta. Perinteinen-mallin luominen resurssien Azure sallii vain all-or-nothing access tilaus ja valitse Poista-tallennustilan tilin.

Tässä oppaassa keskitytään suositellut keinot tallennustilan-tilien luominen eli Resurssienhallinta-malli. -Resurssienhallinta tallennustilan tilit, joka antaa Accessin sijaan koko tilaukseen, jossa voidaan hallita Lisää rajallinen tasolla käyttämällä Roolipohjainen käyttöoikeuksien valvonta (RBAC) hallinta suuntaisesti.

###<a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Voit suojata tilisi tallennustilan Roolipohjainen käyttöoikeuksien valvonta (RBAC)

Oletetaan, että keskustella RBAC on ja miten voit käyttää sitä. Kunkin Azure tilauksella on Azure Active Directory. Käyttäjät, ryhmät ja kansion sovelluksista voidaan myöntää pääsyn hallinta Azure tilauksen resurssit, jotka käyttävät resurssien hallinnan käyttöönottomalli. Tätä kutsutaan nimellä Roolipohjainen käyttöoikeuksien valvonta (RBAC). Voit hallita tätä käyttöoikeuksia, voit käyttää [Azure portal](https://portal.azure.com/), [Azure CLI Työkalut](../xplat-cli-install.md), [PowerShell](../powershell-install-configure.md)tai [Azure tallennustilan resurssin tarjoajan REST API](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Resurssienhallinta-mallin kanssa olet sijoittanut tallennustilan tilin resurssin ryhmän ja hallita sitä, access Azure Active Directoryn avulla tiettyjä tallennustilan tilin hallinta suuntaisesti. Voit esimerkiksi tarkastella tietyille käyttäjille voi käyttää tallennustilan tilin-näppäimiä, kun muut käyttäjät voivat tarkastella tallennustilan tilin tiedot, mutta ei voi käyttää tallennustilan tilin näppäimet.

####<a name="granting-access"></a>Käyttöoikeuksien myöntäminen

Käyttöoikeus määrittämällä RBAC asianmukainen rooli käyttäjät, ryhmät ja sovelluksia, oikea alueessa. Koko tilauksen käyttöoikeus, määrittää roolin tilauksen tasolla. Voit antaa kaikki resursseja resurssiryhmän mukaan myöntää oikeuksia resurssiryhmä itse. Voit määrittää tietyn roolit myös tiettyjen resurssit, kuten tallennustilan tilit.

Seuraavassa on lisätietoja Azure-tallennustilan tilin hallinta-toimintojen käyttäminen RBAC tarvitsevat pääkohdat:

-   Kun määrität access, Määritä lähinnä rooli tilille, jota haluat käyttää. Voidaan hallita tallennustilan tilin hallinta toimintojen, mutta et tilin tieto-objektit. Esimerkiksi myöntää hakemiseen ominaisuudet-tallennustilan tilin (kuten redundancy), mutta ei säilö tai tietojen säilön sisällä Blob-objektien tallennustilaan.

-   Toisen henkilön on oikeus käyttää tallennustilan tilin tieto-objektit voit antaa heille oikeuden lukea tallennustilan tilin ja käyttäjän voit käyttää näppäimiä voit käyttää BLOB-objektit, olevien, taulukoita ja tiedostoja.

-   Roolien voi määrittää tiettyä käyttäjätiliä, käyttäjäryhmille tai tietylle sovellukselle.

-   Kullakin roolilla on luettelo toiminnoista ja ei-toiminnot. Virtuaalikoneen osallistujan rooli on esimerkiksi "listKeys" toiminnon, joka sallii tallennustilan tilin näppäimet voi lukea. Osallistuja on "Ei toiminnot", kuten Active Directoryn käyttäjät käyttöoikeuden päivittäminen.

-   Tallennustilan roolit ovat (mutta ei rajoitettu) seuraavasti:

    -   Omistajan – ne voivat hallita kohteiden kaikki, mukaan lukien access.

    -   Osallistuja – käyttäjä voi tehdä mitään omistaja voi Määritä lukuun ottamatta. Henkilö, jolla tämän roolin voit tarkastella ja tallennustilan tilin näppäimet uudelleen. Tallennustilan tilin avaimet he voivat käyttää tieto-objektit.

    -   Lukija – he voivat tarkastella tietoja tallennustilan tilin tietoja lukuun ottamatta. Esimerkiksi jos liität reader oikeuksilla tallennustilan tilin roolin muille, he voivat tarkastella tallennustilan tilin ominaisuudet, mutta he eivät voi tehdä muutoksia ominaisuuksia tai tarkastella tallennustilan tilin näppäimet.

    -   Tallennustilan tilin avustaja – ne voivat hallita tallennustilan tili – tilauksen resurssiryhmien ja resurssien, voivat lukea ja luoda ja hallita ylläpitosopimus resurssin ryhmän ominaisuuksissa. Hän voi myös käyttää tallennustilan tilin näppäimet, mikä tarkoittaa puolestaan hän voi käyttää tietoja tasossa.

    -   Käyttäjän käyttöoikeus järjestelmänvalvoja – ne voivat hallita tallennustilan tilin käyttöoikeuden. Ne esimerkiksi myöntää lukuoikeudet tietylle käyttäjälle.

    -   Virtuaalikoneen avustaja – ne hallita näennäiskoneiden, mutta ei tallennustilan-tili, johon ne on kytketty. Tämä rooli lisätä tallennustilan tilin avaimet, mikä tarkoittaa, että käyttäjä, jolle voit määrittää tämän roolin päivittää tiedot tasossa.

        Jotta käyttäjä voi luoda virtual machine niillä voi luoda vastaavan Näennäiskiintolevytiedosto tallennustilan tilin. Saadakseen ne tarvitsemansa voi hakea tallennustilan tilin-näppäintä ja välittää sen Ohjelmointirajapinnan luomisen AM. Tämän vuoksi niiden on oltava nämä oikeudet, jotta ne luettelon tallennustilan tilin näppäimet.

- Mahdollisuus määrittää mukautettuja rooleja on toiminto, jonka avulla voit laatia toimintojoukon luettelosta, käytettävissä olevat toiminnot, jotka voidaan suorittaa Azure resurssit.

- Käyttäjän on määritettävä Azure Active Directoryssa ennen kuin voit määrittää ne rooli.

- Voit luoda raportin, joka myöntää/kumottu mitä oikeuksia / jolta ja mitä laajuus PowerShell tai Azure-CLI.

####<a name="resources"></a>Resurssit

-   [Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)

    Tässä artikkelissa kerrotaan Azure Active Directory Roolipohjainen käyttöoikeuksien valvonta ja miten se toimii.

-   [RBAC: Roolien hallinta](../active-directory/role-based-access-built-in-roles.md)

    Tässä artikkelissa kuvataan kaikki käytettävissä olevat RBAC valmiit roolit.

-   [Tietoja resurssien hallinnan käyttöönotto- ja perinteinen käyttöönotto](../resource-manager-deployment-model.md)

    Tässä artikkelissa kerrotaan Resurssienhallinta käyttöönotto- ja perinteinen käyttöönoton mallit ja kerrotaan käyttämisestä henkilöstöpäälliköt- ja -ryhmät

-   [Azure suorittaminen, verkon ja tallennustilaa tarjoajat Azure Resurssienhallinta-kohdassa](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

    Tässä artikkelissa kerrotaan, miten Azure Laske, verkon ja tallennustilaa tarjoajat toimi Resurssienhallinta-malli.

-   [Roolipohjainen käyttöoikeuksien valvonta REST-ohjelmointirajapinnalla hallinta](../active-directory/role-based-access-control-manage-access-rest.md)

    Tässä artikkelissa kerrotaan, miten REST-Ohjelmointirajapinnalla avulla voit hallita RBAC.

-   [Azure-tallennustilan resurssin tarjoajan REST API-viittaus](https://msdn.microsoft.com/library/azure/mt163683.aspx)

    Näin voit hallita tiliäsi tallennustilan ohjelmallisesti ohjelmointirajapinnan viittauksen.

-   [Kehittäjän ohje auth Azure Resurssienhallinta-ohjelmointirajapinnalla](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

    Tässä artikkelissa kerrotaan, miten todennuksen Resurssienhallinta-ohjelmointirajapinnan.

-   [Microsoft Azure-Ignite Roolipohjainen käyttöoikeuksien valvonta](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Tämä on kanavan 9 videon linkin 2015 MS Ignite neuvottelusta. Tässä istunnossa ne keskustella hallinta ja raportointiominaisuudet Azure-tietokannassa ja tutkiminen parhaita käytäntöjä ympärille Azure Azure Active Directory-tilauksista käytön suojaamisesta.

###<a name="managing-your-storage-account-keys"></a>Avaimien tallennustilan tilin hallinta

Tallennustilan tilin avaimet 512-bittinen merkkijonot luoma Azure, sekä tallennustilan tilin nimi, voidaan käyttää tallennustilan tilin tallennetuista tieto-objektit, kuten BLOB-taulukossa, sanomien ja Azure tiedostot Jaa tiedostoja kohteita. Käyttöoikeuden-tallennustilan tilin näppäimet ohjaa tietojen tasossa tallennustilan tilin hallinta.

Tallennustilan jokaisella tilillä on kaksi näppäimet kutsutaan "Avaimen 1" ja "Avaimen 2" [Azure portal](http://portal.azure.com/) ja PowerShellin cmdlet-komennot. Nämä voit luotava manuaalisesti käyttämällä useilla tavoilla, mukaan lukien muun muassa käyttämällä [Azure portal](https://portal.azure.com/), PowerShellin Azure-CLI tai ohjelmallisesti käyttämällä .NET tallennustilan asiakas-kirjaston tai Azure tallennustilan Services REST-Ohjelmointirajapinnalla.

On jokin muu luku syistä avaimien tallennustilan tilin uudelleen.

-   Ehkä luotava tietoturvasyistä säännöllisesti.

-   Tallennustilan tilin avaimien uudelleen, jos joku hallitut yhteyden Bluetooth sovellukseen ja noutaa englanninkielisissä versiossa tai tallennettu määritystiedostossa, hänelle täydet tallennustilan-tiliisi.

-   Toisen avaimen uudistaminen tapaus on, jos työryhmä käyttää tallennustilan Explorer-sovellus, joka säilyttää tallennustilan tilin-näppäintä ja jonkin ryhmän jäsenten jättää. Sovelluksen jatkaa työskentelyä, hänelle access tallennustilan tiliisi sen jälkeen, kun ne ovat kadonneet. Tämä on todella luomiaan tilin tason jaettu Access allekirjoitukset ensisijainen syy – voit käyttää tilin tason SAS sijaan tallentaminen pikanäppäimet määritystiedostoa.

####<a name="key-regeneration-plan"></a>Avaimen uudistaminen suunnitelma

Et halua uudelleen vain ilman suunnitella hyvin käytät avain. Jos teet näin, kaikki access saattaa tulostu tallennustilan tiliin, jotka voivat aiheuttaa merkittäviä häiriöt. Tämän vuoksi on kaksi avaimet. On luotava yksi näppäin kerrallaan.

Että avaimien uudelleen, että sinulla on kaikki sovellukset, jotka ovat riippuvaisia tallennustilan tilin sekä muut palvelut, joita käytät Azure luettelo. Esimerkiksi jos käytät Azure Media-palvelut, jotka ovat riippuvaisia tallennustilan tilin, on uudelleen synkronoit pikanäppäimet media-palvelun kanssa sen jälkeen, kun näppäintä uudelleen. Jos käytät sovelluksia, kuten tallennustilan explorer, sinun on antamaan näiden sovellusten uusi näppäimet. Huomaa, että jos VMs, jonka näennäiskiintolevytiedostoja tallennetaan tallennustilan-tiliin, ne ei vaikuta tallennustilan tilin näppäimet uudelleen.

Voit luoda uudelleen avaimien Azure-portaalissa. Kun avaimet on luotava uudelleen ne voivat käyttää synkronoitavaksi tallennustilan palveluita yli 10 minuutin kuluttua.

Kun olet valmis, on tässä näkyy yksityiskohtaisesti, kuinka muutettava key-tuotetunnuksen Yleiset prosessi. Tässä tapauksessa olettaen on, että käytät parhaillaan avain 1 ja aiot muuttaa kaikki käyttämään avaimen 2.

1.  Luo avain 2 varmistaa, että se on suojattu. Voit tehdä tämän Azure-portaalissa.

2.  Kaikkien sovellusten tallennustilan avain tallennuspaikan muuttaminen käyttämään avaimen 2 uusi arvo tallennustilan-näppäintä. Testaa ja julkaise sovellus.

3.  Kun kaikki sovelluksia ja palveluja on ylös ja suorittaminen onnistui, Luo avain 1. Näin varmistat, että kuka tahansa käyttäjä, jolle et ole nimenomaisesti antanut uusi avain ei enää ole tallennustilan tilin käyttöoikeus.

Jos käytät parhaillaan avaimen 2, voit noudattamalla samoja ohjeita, mutta kumota avaimen nimet.

Voit siirtää muutaman päivän päälle muuttamalla kunkin sovelluksen uusi avain ja julkaisemisen. Kun kaikki ne on valmis, olisi sitten palaa ja Vanha avain uudelleen, jotta se ei enää toimi.

Toinen vaihtoehto on lisätä tallennustilan tilin avain [Azure avaimen säilö](https://azure.microsoft.com/services/key-vault/) kuin salainen, jolloin sovellustesi noutaa sieltä. Valitse Kun avain uudelleen ja Päivitä Azure avaimen säilö, sovellukset ei on otettava, koska ne Nosta uusi avain Azure avain säilöstä automaattisesti. Huomaa, että sinulla on luku näppäintä aina, kun tarvitset niitä sovelluksen tai voit välimuistiin muistiin ja jos se epäonnistui, kun käytät sitä, hakea avain uudelleen Azure avain säilöstä.

Toisen suojaustason tallennustilan avaimien Lisää myös käyttämällä Azure avaimen säilö. Jos käytät tätä menetelmää, on aina tallennustilan avaimen englanninkielisissä määritystiedostossa, joka poistaa kyseisen avenue, jonkun käytön näppäimet lupaa käyttää.

Voit myös määrittää access avaimien Azure Active Directoryn avulla on toinen etu Azure avaimen säilö. Tämä tarkoittaa sitä, voit myöntää oikeuden sovelluksia, jotka on noutaa näppäimet Azure avain säilöstä ja tiedät, että muut sovellukset eivät pysty käyttämään näppäimet ilman oikeuden erityisesti joitakin.

Huomautus: kannattaa käyttää vain yhtä näppäimet kaikkien sovellustesi samanaikaisesti. Jos käytät vaikkapa avain 1 ja muiden avaimen 2, voit voi kiertää avaimien ilman menetetä sovelluksen.

####<a name="resources"></a>Resurssit

-   [Tietoja Azure-tallennustilan](storage-create-storage-account.md#regenerate-storage-access-keys)

    Tässä artikkelissa on yleiskatsaus tallennustilan tilien ja käsitellään tarkastelu, kopioiminen ja tallennustilaa pikanäppäinten uudelleen.

-   [Azure-tallennustilan resurssin tarjoajan REST API-viittaus](https://msdn.microsoft.com/library/mt163683.aspx)

    Tässä artikkelissa on linkkejä artikkeleita hakeminen tallennustilan tilin näppäimet ja uudelleen tallennustilan tilin näppäimet avulla REST-Ohjelmointirajapinnalla Azure-tilille. Huomautus: Tämä on Resurssienhallinta tallennustilan tilit.

-   [Tallennustilan tilit toimenpiteet](https://msdn.microsoft.com/library/ee460790.aspx)

    Tallennustilan palvelun hallinta REST API-viittaus tässä artikkelissa on linkkejä tiettyjä artikkeleihin hakeminen ja tallennustilaa tilin näppäimiä REST-Ohjelmointirajapinnalla uudelleen. Huomautus: Tämä on perinteinen tallennustilan tilit.

-   [Sano uusia avaimen hallinta – voit hallita käyttämällä Azure AD Azuren tallennustilaan tietojen avulla](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

    Tämän artikkelin avulla opit käyttämään Active Directory käyttöoikeuksien Azure avain säilöön avaimien Azure-tallennustilan hallinta. Se näyttää myös käyttämisestä Azure automaatio-työ uudelleen näppäimet tunnissa.

##<a name="data-plane-security"></a>Tasossa tietoturva

Tasossa tietosuojan viittaa suojaamiseen Azuren tallennustilaan – BLOB-objektit, olevien, taulukoiden ja tiedostojen tallennettuja tieto-objektit menetelmät. Olemme katsomista menetelmiä salaamaan tiedot ja suojauksen tietojen siirron aikana, mutta kuinka käymään objektien käytön sallimisesta?

Kaksi tapaa tieto-objektit itse käytön hallinnasta on lähinnä. Tallennustilan tilin näppäimet käytön hallinnasta on ensimmäinen ja toinen on jaettu Access allekirjoitusten käyttäminen tiettyä tieto-objektit käyttöoikeus tietyn ajanjakson aikana.

Ainoa poikkeus huomata on, että voit sallia yleisön, että BLOB määrittämällä käyttöoikeustaso, säilö, joka sisältää BLOB-objektit vastaavasti. Jos määrität access säilön ja Blob-säilö, kyseisen säilön BLOB julkisen lukuoikeus on sallittua. Tämä tarkoittaa sitä, kuka tahansa ja blob-kyseisen säilön osoittamalla URL-osoite voi avata sitä selaimessa jaettu Access-allekirjoitus tai ilman tallennustilan tilin näppäimet.

###<a name="storage-account-keys"></a>Tallennustilan tilin näppäimet

Tallennustilan tilin avaimet 512-bittinen merkkijonot luoma Azure, sekä tallennustilan tilin nimi-avulla voidaan käyttää tallennustilan tilin tallennetuista tieto-objektit.

Voit esimerkiksi lukea BLOB-objektit, kirjoittaa olevien, luoda taulukoita ja muokata tiedostoja. Monet näistä toiminnoista voidaan suorittaa Azure-portaaliin, tai käytä jotakin seuraavista monet tallennustilan Explorer-sovellukset. Voit myös kirjoittaa koodin REST-Ohjelmointirajapinnalla ja tallennustilaa asiakkaan kirjastoihin käytön voit suorittaa nämä toiminnot.

Kuten edellä osassa [Hallinta tasossa suojaus](#management-plane-security)-perinteinen-tallennustilan tilin tallennustilan näppäimet pääsy voi myöntää antamalla täydet Azure-tilaukseen. Tallennustilan näppäimet Azure Resurssienhallinta mallin tallennustilan tilin käyttöoikeus voi säätää Roolipohjainen käyttöoikeuksien valvonta (RBAC) kautta.

###<a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Miten määritettävän tilin käyttäminen jaettu Access allekirjoitukset ja tallennettu käytäntöjen objektien käyttöä

Jaettu Access-allekirjoitus on merkkijono, joka sisältää suojaustunnus, joka voidaan liittää URI, jonka avulla voit tallennustilan objektien edustajakäyttöoikeudet ja määritä kuten käyttöoikeudet ja käyttöoikeustasot päivämäärä ja kellonaika-alueen rajoitukset.

Voit antaa käyttää BLOB-objektit, säilöjen, sanomien, tiedostoja ja tauluja. Taulukoita voit myöntää todella solualueeseen, taulukon kohteiden käyttöoikeudet määrittämällä osio ja rivin tärkeimmät alueet, johon haluat käyttäjä voi käyttää. Esimerkiksi jos osion avaimella maantieteellinen valtion tallennettuja tietoja, voit voi tarkastella joku Kalifornian vain tietoihin pääsy.

Toinen esimerkki ehkä anna verkkosovelluksen SAS tunnistetta, se voi kirjoittaa tapahtumat jonossa ja antaa työntekijän rooli sovelluksen SAS-tunnuksen hakee viestien jonossa ja käsitellä niitä. Tai yksi asiakas voi antaa SAS-tunnuksen, he voivat ladata kuvat Blob-objektien tallennustilaan säilön ja antaa web-sovelluksen oikeudet lukea kuviin. Kummassakin tapauksessa on koskee erottelua – kunkin sovelluksen voidaan antaa vain ne käyttöoikeudet, jotka he tarvitsevat jotta voit suorittaa tämän tehtävän. Tämä on mahdollista, kun jaettu Access allekirjoitukset.

####<a name="why-you-want-to-use-shared-access-signatures"></a>Miksi haluat jakaa Access allekirjoitusten käyttäminen

Miksi laitteesta kannattaa käyttää SAS sijaan tallennustilan tilin avainta, joka on siten helpompi vain muille käyttäjille? Tallennustilan tiliavain, jossa on jakaa tallennustilan kuningaskunta avaimet. Se antaa täydet käyttöoikeudet. Joku käyttää avaimien ja niiden koko Musiikki-kirjaston lataaminen tallennustilan-tilillesi. Ne voi myös korvata tiedostojen virus tartunnan versioilla tai varastaa tietoja. Sivukomentojen tallennustilan tilin poissa rajoittamaton käyttö on jotakin, mitä ei olisi otettava sipaise.

Jaettu Access allekirjoituksia voit antaa asiakkaan vain rajoitetun ajan tarvittavista käyttöoikeuksista. Esimerkiksi jos joku ladataan blob tiliä, voit myöntää ne vain riittävästi aikaa, voit ladata (sen mukaan, Blob-objektien koon tietenkin) Blob-objektien kirjoitusoikeudet. Ja jos muutat mielesi, voit kumota että.

Lisäksi voit määrittää pyyntöihin käyttämällä Suojaussidokset on rajoitettu IP-osoite tai IP-osoitealueita Azure ulkopuolelta. Voit myös edellyttää, että pyynnöt tehdään tietyn protokollan (HTTPS tai HTTP/HTTPS) avulla. Tämä tarkoittaa sitä, jos haluat sallia HTTPS-liikenne ja voit määrittää tarvittavat protokolla HTTPS vain HTTP-liikenne estetään.

####<a name="definition-of-a-shared-access-signature"></a>Jaettu käyttö allekirjoituksen määritys

Jaettu Access-allekirjoitus on joukko kyselyparametrit liitetään osoittamalla resurssin URL-osoite

joka sisältää tietoja sallittu käyttö- ja aika, jonka käyttöä on sallittu. Esimerkki; Tämä URI on lukuoikeudet blob viiteen minuuttiin. Huomautus SAS kyselyparametrit on oltava koodattu URL-osoite, kuten kaksoispiste (:) tai välilyönnillä 20 % 3 % a.

    http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
    ?sv=2015-04-05 (storage service version)
    &st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
    &se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
    &sr=b (resource is a blob)
    &sp=r (read access)
    &sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
    &spr=https (only allow HTTPS requests)
    &sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)

####<a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Miten jaettu Access allekirjoitus todentaa Azure Tallennuspalvelu

Tallennustilan-palvelu on vastaanottanut pyynnön, kun se syötteen kyselyn parametrit ja luo käyttämällä samalla tavalla kuin puheluja ohjelma allekirjoituksen. Se sitten vertaa kahden allekirjoitukset. Jos ne sopivat tallennuspalvelu voit tarkistaa tallennustilan service-versio, varmista, että se on voimassa, varmista, että nykyisen päivämäärän ja kellonajan ovat määritetyn keskusteluikkunasta käsin, varmista, että access pyytää vastaa pyynnön jne.

Esimerkiksi ja tutustu URL-osoite edellä, jos URL-osoite on osoittamalla tiedoston sen sijaan, että blob-pyyntö epäonnistuu, koska se määrittää, että jaetut Access allekirjoitus on blob. Jos REST-komentoa, kutsutaan on päivitettävä blob-se virhetilanne, koska jaetun Access-allekirjoituksen määrittää, että vain luku-käyttö on sallittu.

####<a name="types-of-shared-access-signatures"></a>Jaettu käyttö allekirjoitukset tyypit

-   Palvelun tason SAS voidaan käyttää tiettyä resursseja tallennustilan tilillä. Joitakin esimerkkejä haetko BLOB säilössä lataaminen blob kohteen taulukon päivittäminen viestien lisääminen jonossa tai tiedoston lataaminen jaetun tiedoston luettelo.

-   Tilin tason SAS voidaan käyttää mitään, palvelun tason SAS voi käyttää. Lisäksi se voi antaa resurssit, jotka eivät ole sallittuja ja palvelun tason Suojaussidokset, kuten mahdollisuus luoda säilöjen, taulukoita tai olevien jaettujen tiedostojen asetukset. Voit myös määrittää useita palveluiden käyttäminen kerralla. Esimerkiksi joku saattaa antaa BLOB-objektit ja tiedostojen tallennustilan tilin käyttö.

####<a name="creating-an-sas-uri"></a>SAS URI luominen

1.  Voit luoda luonnoslehtiössä URI pyydettäessä, määrittämällä kaikki kyselyparametrit aina.

    Tämä on todella joustavia, mutta jos looginen joukko parametreja, jotka muistuttavat aina, kun on tallennettu Access-käytännön avulla on paremman käsityksen.

2.  Voit luoda tallennettu-käyttöoikeuskäytäntö, koko säilö, jaettua tiedostoresurssia, taulukko tai jonossa. Sitten voit käyttää tätä pohjana, voit luoda SAS URI. Tallennettu käytäntöjen perustuvat käyttöoikeudet voi kumota helposti. Käytössä voi olla enintään 5 käytäntöjä säilö, jonossa, taulukko tai jaettua tiedostoresurssia.

    Esimerkiksi jos aiot on usean käyttäjän lukea tietyn säilössä BLOB-objektit, voit luoda tallennettu Access käytännön, joka lukee "Anna lukuoikeudet" ja muita asetuksia, jotka ovat samat aina. Voit luoda sitten SAS-URI käyttämällä tallennettu käyttöoikeuskäytäntö asetukset ja määrittämällä vanhentumisen päivämäärä ja kellonaika. Tämä etuna on se, että sinun ei tarvitse määrittää kaikki kyselyparametrit aina.

####<a name="revocation"></a>Peruuttaminen

Oletetaan, että Suojaussidokset on ongelma tai haluat muuttaa yrityksen suojaus tai säädösten noudattamisen vaatimukset. Miten peruutat käyttämällä kyseisen SAS resurssin käyttöoikeutta? Odotusaika riippuu siitä, miten olet luonut Suojaussidosten URI.

Jos käytät ad hoc URI, on kolme vaihtoehtoa. Voit myöntää SAS tunnusten lyhyt vanhenemiskäytännöistä ja odota yksinkertaisesti Suojaussidokset voimassa. Voit nimetä uudelleen tai Poista resurssi (olettaen että tunnus on rajattu yhtenä objektina). Voit muuttaa tallennustilan tilin näppäimet. Tämä vaihtoehto voi olla suuri vaikutus, sen mukaan, kuinka monta palveluita käyttävät tallennustilan tiliin ja ei jotain ilman suunnitella hyvin ei todennäköisesti ole.

Jos käytössäsi on SAS, joka on tallennettu käyttöoikeuskäytäntö johdettu, voit poistaa access peruutetaan tallennettu käyttöoikeuskäytäntö – voit vain muuttaa sitä, jotta se on vanhentunut tai voit myös poistaa kokonaan. Tämä tulee voimaan heti ja peruuttaa jokaisen luotujen tallennettu Access käytäntöä käyttäviä Suojaussidosten. Päivittäminen tai poistamalla tallennettu käyttöoikeuskäytäntö voi käyttää kyseisen tietyn säilön jaettua tiedostoresurssia vaikutus henkilöiden taulukon tai jonon kautta SAS, jos asiakkaiden kirjoitetaan, jotta ne pyytää uuden SAS, kun vanhan on virheellinen, mutta se toimii moitteettomasti.

Käyttämällä SAS, joka on tallennettu käyttöoikeuskäytäntö johdettu antaa voi kumota, SAS heti, se on suositellut parhaiden käytäntöjen mukaisesti kannattaa käyttää aina tallennettu käytön käytännöt kun se on mahdollista.

####<a name="resources"></a>Resurssit

Lue lisätietoja käyttämisestä jaettu Access allekirjoitukset ja tallennettu käytäntöjen, ja lisää siihen esimerkkejä, Lisätietoja on seuraavissa artikkeleissa:

-   Nämä ovat viittaus on artikkeleissa.

    -   [Palvelun Suojaussidokset](https://msdn.microsoft.com/library/dn140256.aspx)

        Tässä artikkelissa on esimerkkejä palvelun tason SAS käyttämisestä BLOB-objektit, sanomien, taulukon alueet ja tiedostoja.

    -   [Palvelun SAS luominen](https://msdn.microsoft.com/library/dn140255.aspx)

    -   [Tilin SAS luominen](https://msdn.microsoft.com/library/mt584140.aspx)

-   Nämä ovat opetusohjelmat jaettu Access allekirjoitukset ja tallennettu käytäntöjen luominen käyttämällä .NET asiakas-kirjasto.

    -   [Jaettu käyttö allekirjoitusten (SAS) käyttäminen](storage-dotnet-shared-access-signature-part-1.md)

    -   [Jaettu Access allekirjoituksia, osa 2: Luominen ja käyttäminen SAS Blob-palvelussa](storage-dotnet-shared-access-signature-part-2.md)

        Tässä artikkelissa on selitys SAS-mallin esimerkkejä jaettu Access allekirjoitukset ja parhaiden käytäntöjen suosituksia käyttää suojaussidokset. Myös aiheena on myönnetty käyttöoikeus kumoamisen.

-   IP-osoite (IP-käyttöoikeusluettelot) käytön rajoittaminen

    -   [Mikä on päätepisteen käyttöoikeusluettelo (käyttöoikeusluettelot)?](../virtual-network/virtual-networks-acl.md)

    -   [Palvelun SAS luominen](https://msdn.microsoft.com/library/azure/dn140255.aspx)

        Tämä on artikkeli palvelun tason suojaussidosten; Esimerkki IP ACLing sisältää.

    -   [Tilin SAS luominen](https://msdn.microsoft.com/library/azure/mt584140.aspx)

        Tämä on viittaus on artikkelissa tilin tason suojaussidosten; Esimerkki IP ACLing sisältää.

-   Todennus

    -    [Azure-tallennustilan-palveluiden todennus](https://msdn.microsoft.com/library/azure/dd179428.aspx)

-   Jaettu Access allekirjoitukset opetusohjelman aloittaminen

    -   [Opetusohjelman aloittaminen Suojaussidokset](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

##<a name="encryption-in-transit"></a>Siirron salaus

###<a name="transport-level-encryption--using-https"></a>Transport-tason salaus – HTTPS-yhteyden avulla

Toinen vaihe tekee saapuville Azuren tallennustilaan tietojen suojauksesta on salaamaan tiedot asiakkaan ja Azure-tallennustilan välillä. Ensimmäinen suositus on käytetään aina [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -protokollan, joka takaa tietosuojaa julkisen Internetin välityksellä.

Käytä HTTPS aina, kun kutsumista REST API tai käyttäminen-objektien tallennustilaan. **Jaettu Access allekirjoitukset**, joiden avulla voidaan edustajakäyttöoikeudet Azuren tallennustilaan objektit, myös vaihtoehto, joka määrittää HTTPS-protokollan avulla voidaan jakaa Access allekirjoitukset varmistetaan, että kuka tahansa käyttäjä, lähettää linkin SAS tunnusten käyttää ERISNIMI protokolla käytettäessä.

####<a name="resources"></a>Resurssit

-   [Ota käyttöön HTTPS sovelluksen Azure sovelluksen-palvelussa](../app-service-web/web-sites-configure-ssl-certificate.md)

    Tässä artikkelissa kerrotaan ottamisesta käyttöön HTTPS Azuren verkkosovelluksessa.

###<a name="using-encryption-during-transit-with-azure-file-shares"></a>Siirron aikana salauksen käyttäminen Azure tiedostoresurssit

Azure tiedostosäilön tukee HTTPS käytettäessä REST-Ohjelmointirajapinta, mutta Lisää yleisesti käytetty SMB-tiedostoresurssin liitetään AM. SMB 2.1 ei tue salausta, jotta yhteydet sallitaan vain samalla alueella Azure-tietokannassa. Kuitenkin SMB 3.0 tukee salausta ja Windows Server 2012 R2: n kanssa voidaan käyttää, Windows 8, Windows 8.1 tai Windows 10-salliminen rajat-alueen käyttäminen ja jopa access työpöydälle.

Huomaa, että kun Azure tiedostoresurssit voidaan käyttää Unix-Linux SMB-asiakas ei vielä tue salausta, jotta käyttö on sallittu vain Azure alueella. Salauksen Linux tuki on Linux kehittäjät vastuussa SMB toiminnon ohjeita. Kun henkilö Lisää salausta, on sama voi käyttää tapaan Windows Azure-tiedostoresurssin Linux.

####<a name="resources"></a>Resurssit

-   [Voit käyttää Azure tiedostosäilön Linux](storage-how-to-use-files-linux.md)

    Tämän artikkelin avulla voit ottaa Azure-tiedostoresurssin Linux-järjestelmässä ja lataa ja lataa tiedostot.

-   [Windows Azure-tiedostosäilön käytön aloittaminen](storage-dotnet-how-to-use-files.md)

    Tässä artikkelissa on yleiskatsaus Azure tiedostoresurssit ja niiden käyttöön ja käyttää niitä käyttämällä PowerShell- ja .NET.

-   [Keltaiset Azure tiedostojen tallennus](https://azure.microsoft.com/blog/inside-azure-file-storage/)

    Tässä artikkelissa ilmoittaa Azure tiedostosäilön yleiseen käyttöön ja teknistä tietoa SMB 3.0-salausta.

###<a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Suojaa tiedot, joita voi lähettää tallennustilan asiakkaan salauksen avulla

Jokin muu vaihtoehto, jonka avulla voit varmistaa, että tiedot on suojattu, kun asiakassovellus ja tallennustilaa välillä siirrettävien on asiakaspuolen salausta. Tiedot salataan ennen siirrettävän Azure varastoon. Kun tietojen noutaminen Azure-tallennustilan, tiedot puretaan, kun se vastaanotetaan asiakaspuolen. Vaikka tiedot salataan siirtymällä koneisiin välillä, on suositeltavaa, että voit käyttää myös HTTPS-kuin tietojen eheys tarkistukset joka minimoida verkon virheiden vaikuttavia tietojen eheys hallinta.

Asiakkaan salaus on myös tavan tietojen etsiminen rest-, tiedot on tallennettu salattuja muodossaan. Käsittelemme tämän osan tarkemmin salauksen [Rest-palvelussa](#encryption-at-rest).

##<a name="encryption-at-rest"></a>Salauksen Rest-palvelussa

On kolme Azure ominaisuuksia, jotka tarjoavat salaus rest-palvelussa. Azure salauksen käytetään salaa IaaS näennäiskoneiden levyjen OS ja tiedot. Muut kahden – asiakkaan salaus ja SSE – ovat molemmat Azuren tallennustilaan tiedot salataan. Oletetaan, että kaikkien näiden, tarkista Tee vertailu ja milloin kutakin voi käyttää.

Kun asiakkaan salauksen avulla voit salata (joka on tallennettu myös salattujen muodossaan tallennustilaan) salataanko siirrettävät tiedot, voit halutessasi yksinkertaisesti https-PROTOKOLLAN avulla siirron aikana ja on jollakin tavalla tietojen salata automaattisesti, kun se on tallennettu. Kahdella toiminto--Azure salauksen ja SSE. Käytetään suoraan VMs käyttämä Käyttöjärjestelmä ja tietojen levyille tiedot salataan ja toinen käytetään salaa kirjoittaa Azure-Blob-säiliö tiedot.

###<a name="storage-service-encryption-sse"></a>Tallennustilan palvelun salaus (SSE)

SSE voit pyytää, että tallennuspalvelu salata tiedot automaattisesti, kun kirjoitat sen Azure-tallennustilan. Kun tietojen lukeminen Azure-tallennustilan, se puretaan tallennustilan-palvelun ennen palautetaan. Voit suojata tietoja tarvitsematta Muokkaa koodia tai sovelluksia koodin lisääminen.

Tämä asetus, joka koskee koko tallennustilan tilin on. Voit ottaa käyttöön ja poistaa tämän toiminnon käytöstä muuttamalla tämän asetuksen arvosta. Toiminto, voit käyttää Azure portaalin, PowerShell, Azure-CLI, tallennustilan resurssin tarjoajan REST-Ohjelmointirajapinnalla tai .NET tallennustilan asiakas-kirjasto. SSE on oletusarvoisesti poistettu käytöstä.

Microsoft hallitsee salaus käytettävät näppäimet tällä hetkellä. Olemme Luo näppäimet alun perin ja hallita suojattuun säilöön näppäimet sekä säännöllisesti kiertoa sisäinen Microsoft käytännön määrittämä. Jatkossa Hae voi hallita omia salausavaimet ja anna siirtymispolku Microsoftin hallitsemaan Key asiakkaan hallitun avaimet.

Tämä ominaisuus on käytettävissä Standard- ja tallennustilaa Premium-tileissä luotu resurssien hallinnan käyttöönottomalli. SSE koskee vain estää BLOB-sivun BLOB-objektit, ja liitä BLOB-objektit. Muun tyyppisiä tietoja, kuten taulukoita, olevien ja tiedostoista ei salata.

Tiedot salataan vain, kun SSE on otettu käyttöön ja tietoja kirjoitetaan Blob-objektien tallennustilaan. Käyttöönoton tai käytöstäpoiston SSE ei vaikuta aiemmin luotuihin tietoihin. Toisin sanoen, kun otat tämän salausta, se siirtyvät ei takaisin ja salata tiedot, jotka on jo olemassa. eikä se näkyy salauksen tiedot, jotka on jo olemassa, kun poistat SSE.

Jos haluat käyttää tätä toimintoa perinteinen tallennustilan tilillä, voit luoda uuden Resurssienhallinta-tallennustilan tilin ja AzCopy avulla voit kopioida tiedot uuteen tiliin. 

###<a name="client-side-encryption"></a>Asiakkaan salaus

Olemme edellä asiakaspuolen salausta, kun keskustella salataanko siirrettävät tiedot salausta. Tämän ominaisuuden avulla voit salata ohjelmallisesti tietojen asiakassovellus ennen sen lähettämistä yli koneisiin kirjoitetaan Azuren tallennustilaan ja salauksen ohjelmallisesti tietojen noutaminen Azuren tallennustilaan jälkeen.

Tämä on salaus siirron, mutta se tarjoaa myös salauksen osoitteessa Rest-toiminnon. Huomaa, että tiedot salataan siirron, vaikka edelleen Suosittelemme, että käytät HTTPS hyödyntää valmiin tietojen eheyden tarkistaa, joka auttaa pienentämään verkon virheiden vaikuttavia tietojen eheys.

Esimerkki, jossa voit käyttää tätä on, jos sinulla on web-sovelluksen, joka tallentaa BLOB-objektit ja noutaa BLOB-objektit ja haluamasi sovellus ja tiedot, jotka on suojattu mahdollisimman hyvin. Tässä tapauksessa käytetään asiakkaan salausta. Asiakkaan ja Azure-Blob-palvelun välillä liikenne sisältää salatun resurssin ja ei kukaan tulkita salataanko siirrettävät tiedot ja jakomenetelmä kyselyjä yksityinen BLOB-objektit.

Asiakkaan salaus on valmiina Java ja .NET tallennustilan asiakas-kirjastot, jotka käyttävät puolestaan Azure avain säilöön-API-tekeminen on helppoa Voit toteuttaa. Prosessi, ja salauksen tiedot käyttää kirjekuori-tekniikka ja kunkin tallennuspaikka salaus käyttämä metatiedot. Esimerkiksi varten BLOB-objektit, se tallentaa sen blob-metatiedot varten olevien, lisää se jonon jokaisen viestin.

Salauksen itse voit luoda ja hallita omia salausavaimet. Voit myös käyttää Azure-tallennustilan asiakkaan kirjaston luomat avaimet tai voi olla Luo näppäimet Azure avaimen säilö. Voit tallentaa salausavaimet paikallisen avaimen tallennustilaan tai voit tallentaa ne Azure-avain säilöön. Azure avaimen säilö voit myöntää oikeuden Azure avain säilöön tietoja Azure Active Directoryn avulla tietyille käyttäjille. Tämä tarkoittaa, että et pelkästään kuka tahansa käyttäjä, lue Azure avaimen säilö ja noutaa asiakkaan salauksen käytät näppäimet.

####<a name="resources"></a>Resurssit

-   [Salaa ja salauksen BLOB Microsoft Azuren tallennustilaan käyttämällä Azure avaimen säilö](storage-encrypt-decrypt-blobs-key-vault.md)

    Tässä artikkelissa kerrotaan asiakkaan salauksen käyttäminen Azure avaimen säilö, sekä luominen KEK ja tallentaa sen säilöön PowerShellin avulla.

-   [Asiakkaan salaus ja Azure avaimen säilö for Microsoft Azure-tallennustilan](storage-client-side-encryption.md)

    Tässä artikkelissa antaa selvitys asiakkaan salaus- ja esimerkkejä salaaminen ja salauksen resurssien neljä tallennustilan palvelut-tallennustilan asiakas-kirjaston avulla. Se myös puheen tietoja Azure avaimen säilö.

###<a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Salaa levyjen oman näennäiskoneiden käyttämä Azure salauksen avulla

Azure salauksen on uusi ominaisuus, joka on tällä hetkellä esikatselu. Tämän ominaisuuden avulla voit salata OS levyjen ja tietojen levyjä käytetään mukaan IaaS Virtual Machine. Windows-asemat salataan käyttämällä yleisesti BitLocker salaustekniikkaa. Linux-levyjen salataan DM Crypt-tekniikan avulla. Tämä on integroitu Azure avaimen säilö, jotta voit määrittää ja hallita levyn salausavaimet.

Azure salauksen-ratkaisun tukee kolme asiakkaan salaus seuraavissa tilanteissa:

-   Ottaa käyttöön uuden IaaS VMs, jotka on luotu asiakkaan salattu näennäiskiintolevytiedostoja ja asiakkaan salausavaimet, jotka on tallennettu Azure avain säilöön salauksen.

-   Ottaa käyttöön uuden IaaS VMs, jotka on luotu Azure Marketplace-salausta.

-   Ota käyttöön aiemmin IaaS VMs käynnissä Azure salausta.

>[AZURE.NOTE] Linux-virtuaalilaitteiksi jo Azure-tietokannassa tai uusi Linux VMs luotu kuvat Azure Marketplace-salausta OS levyn ei tue tällä hetkellä. Salauksen Linux VMs OS äänenvoimakkuus on tuettu vain VMs, jotka on salattu paikallisen ja Azure ladataan. Tämä rajoitus koskee vain OS levyä. Linux AM tietomääriä salaamista tuetaan.

Ratkaisu tukee seuraavia IaaS VMs public preview-versio, kun käytössä Microsoft Azure varten:

-   Azure avaimen säilö integrointi

-   Vakio- [A, D ja G sarjan IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)

-   Ota käyttöön salauksen IaaS VMs luotu [Azure resurssien hallinnan](../azure-resource-manager/resource-group-overview.md) malli

-   Kaikki Azure julkisen [alueet](https://azure.microsoft.com/regions/)

Tämä ominaisuus varmistaa, että kaikki tiedot virtuaalikoneen levyille salataan muiden Azuren tallennustilaan.

####<a name="resources"></a>Resurssit

-   [Windows-ja Linux IaaS näennäiskoneiden Azure salauksen](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

    Tässä artikkelissa käsitellään Azure salauksen preview-versio ja tekninen raportti latauslinkkiä.

###<a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Azure salauksen, SSE ja asiakkaan salaus vertailu

####<a name="iaas-vms-and-their-vhd-files"></a>IaaS VMs ja niiden näennäiskiintolevytiedostoja

Levyjen IaaS VMs käyttämä on suositeltavaa käyttää Azure salauksen. Voit ottaa käyttöön SSE salata, joiden avulla takaisin näiden levyjen Azuren tallennustilaan näennäiskiintolevytiedostoja, mutta se vain salaa äskettäin kirjoitettujen tiedot. Tämä tarkoittaa sitä, jos luot AM ja ottaa SSE tallennustilan tilin, joka sisältää Näennäiskiintolevytiedosto, vain muutokset salataan, ei alkuperäinen Näennäiskiintolevytiedosto.

Jos luot AM käyttämällä kuvan Azure Marketplacen Azure suorittaa [Suppea kopioi](https://en.wikipedia.org/wiki/Object_copying) tallennustilan tilin kuvan Azuren tallennustilaan ja ei ole salattu, vaikka sinulla olisi SSE käytössä. Kun se luo AM ja alkaa kuvan päivittämistä, SSE alkavat tiedot. Tästä syystä on helpointa luoda Azure Marketplacesta kuvia, jos haluat heidän täysin salattu VMs Azure salauksen käyttäminen.

Jos valmiiksi salattuja AM tuo Azure-paikallisen, osaat lähettämistä salausavaimet Azure avaimen säilö ja jatkaa salauksen käytön, jota käytit paikallisen AM. Tässä skenaariossa käsittelemään Azure salauksen on käytössä.

Jos sinulla ei salattu Näennäiskiintolevyn-paikallisen, voit ladata mukautetun kuvana valikoimaan ja valmistella AM siitä. Jos olet tehnyt tämän Resurssienhallinta-mallien avulla, voit pyytää Azure salauksen käyttöön, kun se käynnistetään AM ylöspäin.

Kun lisäät tietoja DVD-levyllä ja ota se käyttöön AM, voit käyttöön Azure salauksen tietojen kyseisellä levyllä. Se salaa tiedot levyn paikallisesti ensin ja sitten service management kerros hyötyä kuitata kirjoittaminen vastaan tallennustilan, jotta tallennustilan sisältö on salattu.

####<a name="client-side-encryption"></a>Asiakkaan salaus####

Asiakkaan salaus on turvallisin tapa salaamaan tiedot, koska se salaa sitä ennen salataanko siirrettävät ja salaa muiden tiedoissa. Kuitenkin edellyttävät koodin lisääminen käyttämällä tallennustilaa, jotka ehkä halua tehdä sovellusten. Tällöin voit HTTPs salataanko siirrettävät ja SSE tietojen, loput tiedot salataan.

Voit salata taulukon kohteiden ja sanomien BLOB asiakkaan salauksella. SSE voit salata vain BLOB-objektit. Jos tarvitset taulukon ja jonon tiedot salataan, sinun on käytettävä asiakkaan salaus.

Asiakkaan salaus hallitsee kokonaan sovellus. Tämä on turvallisin tapa, mutta edellyttää, että sovelluksesi ohjelmallisesti muutosten ja asettaa hallintaa prosessit. Käytä tätä, kun haluat ylimääräisiä suojaus siirron aikana, ja haluat salata tallennetut tiedot.

Asiakkaan salaus on enemmän kuormituksen asiakkaan ja sinulla on asiakas tämän skaalattavuus-palvelupakettien, erityisesti, jos ole salaaminen ja siirtäminen paljon tietoja.

####<a name="storage-service-encryption-sse"></a>Tallennustilan palvelun salaus (SSE)

SSE hallitaan Azure-tallennustilan. Käyttämällä SSE ei tarjoa salataanko siirrettävät tiedot suojaus, mutta salaa tietoja, koska se on kirjoitettu Azure-tallennustilan. Ei ole vaikutusta suorituskykyyn käyttäessäsi tätä ominaisuutta.

Voi vain salaa estä BLOB-objektit, Liitä BLOB-objektit ja sivun käyttämällä SSE BLOB. Jos haluat salata taulukkotiedot tai jonossa, kannattaa käyttää asiakkaan salausta.

Jos sinulla on arkistosta tai valikoiman näennäiskiintolevytiedostoja, voit käyttää pohjana uusien näennäiskoneiden luomiseen, voit luoda uuden tallennustilan tilin, SSE ottaminen käyttöön ja lataa näennäiskiintolevytiedostoja tilin. Näiden näennäiskiintolevytiedostoja salataan Azure-tallennustilan.

Jos sinulla on käytössä AM ja pidät näennäiskiintolevytiedostoja tallennustilan tilin käytössä SSE levyjen Azure salauksen, se toimii moitteettomasti; johtaa äskettäin kirjoitettu tiedot salataan kahdesti.

##<a name="storage-analytics"></a>Tallennustilan Analytics

###<a name="using-storage-analytics-to-monitor-authorization-type"></a>Tallennustilan analyysin avulla voit valvoa luvan tyyppi

Tallennustilan jokaiselle tilille voit ottaa Azure tallennustilan analyysin suorittamiseen kirjaaminen ja tallentaa arvot. Tämä on erinomainen työkalu, jota käytetään, kun haluat tarkistaa suorituskyvyn mittarit tallennustilan tilin tai vianmääritys tallennustilan tilin, koska suorituskyky on ongelmia.

Toisen tallennustilan analyysitiedot lokit näet tieto on käytetty joku, kun he käyttävät tallennustilan todentamismenetelmä. Esimerkiksi Blob-objektien tallennustilaan avulla saat käytetään jaetun Access-allekirjoituksen tai tallennustilan tilin näppäimet tai käyttää Blob-objektien on julkinen.

Tämä voi olla erittäin hyödyllinen, jos ovat tiiviisti suojaamalla voi käyttää tallennustilaasi. Esimerkiksi Blob-objektien tallennustilaan voit määrittää kaikki säilöt yksityiseksi ja toteuttaa SAS-palvelun käyttöä sovelluksia kaikkialla. Sitten voit tarkistaa säännöllisesti, jos haluat nähdä, jos oman BLOB käytetään-tallennustilan tilin pikanäppäimet, joka ilmoittaa turvallisuus, tai jos BLOB-objektit ovat julkisia, mutta ei kannata voi lokitiedot.

####<a name="what-do-the-logs-look-like"></a>Lokit millaiseksi?

Kun otat tallennustilan tilin arvot ja kirjaaminen palvelun Azure-portaalissa analytics-tiedot alkaa keräämistä nopeasti. Jokaisen palvelun mittaukset ja kirjaaminen on erillinen; lokikirjaus kirjoitetaan vain, kun kyseessä on tehtävä tallennustilan kyseisessä tilissä samalla, kun kirjataan minuutin välein, tunnissa tai päivittäin sen mukaan, miten voit määrittää arvot.

Lokit tallennetaan estä BLOB-säilö $logs-tallennustilan tilin. Tämä säilö luodaan automaattisesti, kun tallennustilan Analytics on otettu käyttöön. Kun tämä säilö on luotu, et voi poistaa, vaikka olet poistanut sen sisältöä.

$Logs säilössä on kumpaankin palveluun on oma kansio ja sitten ei alikansiot, vuosi tai kuukausi/päivä/tunti. Valitse tunti lokit numeroidaan yksinkertaisesti. Miltä kansiorakenne näyttää seuraavat asiat:

![Näytä lokitiedostojen](./media/storage-security-guide/image1.png)

Azuren tallennustilaan jokaisen pyyntö on kirjautunut. Seuraavassa on tilannevedoksen lokitiedostoon, muutaman ensimmäisen kenttien näyttäminen.

![Lokitiedoston tilannevedoksen](./media/storage-security-guide/image2.png)

Näet, että lokit avulla voit seurata kaikenlaista tallennustilan tilin puhelut.

####<a name="what-are-all-of-those-fields-for"></a>Mitkä ovat kaikki kyseiset kentät?

Ei artikkelin resurssit, jotka on useita kenttiä lokit ja miten niitä käytetään luettelo. Seuraavassa on luettelo kentät järjestyksessä:

![Tilannevedoksen lokitiedostoon-kentät](./media/storage-security-guide/image3.png)

Olemme kiinnostunut GetBlob ja kuinka ne todennetut-merkintöjä, jotta annettava Etsi ne merkinnät, joiden toiminnon tyyppi "Get-Blob- ja tarkista pyynnön-tila (4<sup>ja</sup> sarakkeen) ja todennus-tyypin (8<sup>ja</sup> sarakkeen).

Esimerkiksi yllä luettelon ensimmäinen muutamia riveillä pyynnön-tila on "Onnistunut" ja todennus-tyypin "todennetaan". Tämä tarkoittaa pyyntö vahvistettu tallennustilan tilin avaimen avulla.

####<a name="how-are-my-blobs-being-authenticated"></a>Miten oma BLOB parhaillaan todennetut?

On kolme tapaukset, emme kiinnostunut.

1.  Blob on julkiseen ja niitä käytetään URL-Osoitetta ei ole jaettu Accessin allekirjoitettu käyttämällä. Tässä tapauksessa pyynnön-tila on "AnonymousSuccess" ja todennus-tietotyyppi on "anonyymi".

    1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37; **Anonyymi**; mystorage...

2.  Blob on yksityinen ja käytettiin jaettu Access allekirjoituksen kanssa. Tässä tapauksessa pyynnön-tila on "SASSuccess" ja todennus-tietotyyppi on "sas".

    1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200; 416; 64; **sas**; mystorage...

3.  Blob on yksityinen ja käyttää sitä on käytetty tallennustila-näppäintä. Tässä tapauksessa pyynnön-tila on "**onnistunut**" ja todennus-tietotyyppi on "**todennettu**".

    1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Success**; 206; 59; 22; **todennettu**; mystorage...

Voit tarkastella ja analysoida lokitiedostot Microsoft viestin Analyzer. Se sisältää hakemisesta ja suodattamisesta ominaisuuksia. Esimerkiksi haluta etsiä GetBlob onko käyttö vastaa odotuksiasi, eli varmistaaksesi, että joku ei käytä tallennustilan tilin asiattomasti esiintymät.

####<a name="resources"></a>Resurssit

-   [Tallennustilan Analytics](storage-analytics.md)

    Tässä artikkelissa on yleiskatsaus tallennustilan analyysin ja miten voit ottaa ne käyttöön.

-   [Tallennustilan Analytics lokin muoto](https://msdn.microsoft.com/library/azure/hh343259.aspx)

    Tässä artikkelissa kuvataan Analytics Tallennusmuoto ja tiedot käytettävissä olevat kentät, mukaan lukien todennus-tyyppi, joka ilmaisee pyynnön todentamistapa.

-   [Näytön tallennustilan-tilin Azure-portaalissa](storage-monitor-storage-account.md)

    Tässä artikkelissa kerrotaan, miten voit määrittää arvot seuranta ja tallennustilaa tilin kirjaamisen.

-   [Azure-tallennustilan arvot ja kirjaaminen, AzCopy ja viestin Analyzer lopusta loppuun-vianmääritys](storage-e2e-troubleshooting.md)

    Tässä artikkelissa vianmäärityksestä käyttämällä tallennustilan Analytics puheen ja kerrotaan, miten voit käyttää Microsoft viestin Analyzer.

-   [Microsoft Message Analyzer toimivien opas](https://technet.microsoft.com/library/jj649776.aspx)

    Tässä artikkelissa on Microsoft viestin Analyzer viittauksen sekä linkkejä opetusohjelma, pika-aloitusopas ja ominaisuuden yhteenveto.

##<a name="cross-origin-resource-sharing-cors"></a>Usean Origin resurssien jakaminen (CORS)

###<a name="cross-domain-access-of-resources"></a>Toimialueiden välinen resurssien käyttö

Kun käynnissä yhtä toimialuetta selaimen tekee HTTP-pyyntö resurssin toinen toimialue, eli rajat origin HTTP-pyynnön. Esimerkiksi contoso.com-palvelimella HTML-sivu on isännöimät fabrikam.blob.core.windows.net jpeg pyynnön. Tietoturvasyistä selaimet rajoittaa rajat origin pyyntöjen sisällä komentosarjoja, kuten JavaScript käynnistetty. Tämä tarkoittaa, että kun joitakin JavaScript-koodia verkkosivulla contoso.com pyytää, jpeg-fabrikam.blob.core.windows.net, selaimen ei salli pyynnön.

Mitä tämä on Azuren tallennustilaan tehdään? Jos tallennat staattinen kohteita, kuten JSON tai XML-datatiedostojen Blob-objektien tallennustilaan käyttämällä tallennustilan tilin nimi Fabrikam varat-toimialue on fabrikam.blob.core.windows.net ja contoso.com web-sovelluksen ei voi käyttää niitä käyttämällä JavaScript, koska toimialueet ovat eri. Tämä on myös TOSI, jos yrität kutsua jokin Azure-tallennustilan palvelujen – kuten taulukkotallennus –, jotka palauttavat JSON-tietoja käsitellään JavaScript-asiakas.

####<a name="possible-solutions"></a>Ratkaisuja

Voit ratkaista ongelman yksi tapa on määrittää mukautetun toimialueen, kuten "storage.contoso.com" fabrikam.blob.core.windows.net. Ongelma ilmenee, että voit määrittää, että mukautettu toimialue vain yhden tallennustilan tilin. Entä jos varat tallennetaan usean tallennustilan tilin?

Voit ratkaista ongelman myös on web-sovelluksen välityspalvelimen tallennustilan puheluihin edustajana. Tämä tarkoittaa sitä, jos lataat tiedoston Blob-objektien tallennustilaan, web-sovelluksen tapauksessa kirjoita se paikallisesti ja kopioimalla sen sitten Blob-objektien tallennustilaan tai se lukea ne muistiin ja kirjoittamaan Blob-objektien tallennustilaan. Voit vaihtoehtoisesti kirjoittaa oma web-sovellus (esimerkiksi verkko-Ohjelmointirajapinnan), lataa tiedostot paikallisesti ja kirjoittaa ne Blob-objektien tallennustilaan. Kummassakin tapauksessa sinun on tili, jotka toimivat, kun määritettäessä skaalattavuus tarvitsee.

####<a name="how-can-cors-help"></a>Miten CORS voi auttaa?

Azure-tallennustilan voit ottaa käyttöön CORS – toimintojen Origin resurssien jakaminen. Tallennustilan jokaiselle tilille voit määrittää toimialueet, joita voi käyttää tallennustilan tilin resursseista. Esimerkiksi tässä tapauksessa edellä kuvattujen voimme CORS käyttöön fabrikam.blob.core.windows.net-tallennustilan tilin ja määritä se sallimaan contoso.com. Web-sovelluksen contoso.com voidaan sitten käyttää suoraan fabrikam.blob.core.windows.net resursseja.

Huomautus on CORS pääsee, mutta se ei tarjoa todennusta, joita tarvitaan, kaikki muissa kuin julkisen access tallennustilan resursseja. Tämä tarkoittaa sitä, voit käyttää BLOB vain, jos ne ovat julkisia tai voit lisätä jaettu Access allekirjoituksen tarvittavat oikeudet. Taulukoiden ja olevien tiedostojen julkisen eivät pääse ja edellyttävät Suojaussidokset.

Oletusarvon mukaan CORS on poistettu käytöstä kaikki palvelut. Voit ottaa CORS käyttämällä REST-Ohjelmointirajapinnalla tai tallennustilan asiakkaan kirjaston Soita palvelun käytännöt tavoilla. Kun teet näin, voit lisätä CORS säännön, joka on XML. Tässä on esimerkki CORS säännön, joka on määritetty käyttämällä Blob-palvelun palvelun ominaisuuksien määrittäminen toiminto tallennustilan tilin. Voit suorittaa toiminnon tallennustilan asiakas-kirjaston tai REST-ohjelmointirajapinnan käyttäminen Azure-tallennustilan.

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Mitä tarkoittaa kullekin riville:

-   **AllowedOrigins** Tämä kertoo, mitä ei ole vastaavaa toimialueiden voit pyytää ja vastaanottaa tietoja tallennustilan-palvelusta. Tämä kertoo, että contoso.com ja fabrikam.com voit pyytää tietoja Blob-objektien tallennustilaan tietyn tallennustilan tilin. Voit myös määrittää tämän yleismerkkiä (\*), jotta kaikki toimialueet, kun haluat käyttää pyynnöt.

-   **AllowedMethods** Tässä luetellaan menetelmiä (HTTP pyynnön verbien), jotka voidaan käyttää, kun pyynnön. Tässä esimerkissä sallitaan vain HYLLYTETTY ja HAE. Voit tehdä tämän yleismerkkiä (\*), jotta kaikki menetelmät.

-   **AllowedHeaders** Tämä on pyynnön otsikot, jotka origin toimialueen voit määrittää, kun pyynnön. Tässä esimerkissä kaikki metatietojen otsikot x-ms-metatiedon, aloittaen x-ms-metatiedot-kohde ja x-ms-metatiedot-abc valitseminen. Yleismerkki (\*) ilmaisee, että kaikki otsikon alkaa sanalla määritettyä etuliitettä on sallittua.

-   **ExposedHeaders** Tämä kertoo, mitä vastausten otsikot näyttämiä selaimen pyynnön myöntäjä. Tässä esimerkissä mitä tahansa alkaen otsikkoa "x-ms - metatiedot-" voi luovuttaa.

-   **MaxAgeInSeconds** Tämä on aika, selaimen välimuisti kuvatiedostomuodot asetukset pyynnön enimmäismäärän. (Saat lisätietoja kuvatiedostomuodot pyynnön Tarkista ensimmäisen artikkelin ohjeiden mukaisesti.)

####<a name="resources"></a>Resurssit

Katso lisätietoja CORS ja kuinka se otetaan käyttöön, katso nämä resurssit.

-   [Usean Origin resurssien jakaminen Azure.com Azure tallennustilan palvelut-tuki (CORS)](storage-cors-support.md)

    Tässä artikkelissa on yleiskatsaus CORS ja niiden sääntöjen eri tallennustilan-palvelujen määrittäminen.

-   [Usean Origin resurssien jakaminen MSDN Azure tallennustilan palvelut-tuki (CORS)](https://msdn.microsoft.com/library/azure/dn535601.aspx)

    Tämä on CORS tuki Azure-tallennustilan Services ohjeessa. Tämä on linkkejä artikkeleihin kunkin tallennuspalvelu soveltaminen ja Esimerkki ja muut elementit CORS-tiedostossa.

-   [Microsoft Azuren tallennustila: Esittely CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

    Tämä on ensimmäinen blogin artikkelin julkaisu CORS ja miten sitä käytetään näyttäminen linkki.

##<a name="frequently-asked-questions-about-azure-storage-security"></a>Usein kysyttyjä kysymyksiä Azuren tallennustilaan suojaus

1.  **Miten voin 'M siirtäminen sisään tai ulos Azuren tallennustilaan Jos ei voi käyttää HTTPS-protokollan BLOB eheyden tarkistaa?**

    Jos jostakin syystä, sinun on käytettävä HTTP sijaan HTTPS ja olet käsittelemässä ja estä BLOB-objektit, voit MD5 tarkistuksen tarkistamisessa siirrettävän BLOB eheys. Tämä auttaa verkon/transport layer virheet suojaus, mutta ei välttämättä välittävät kalastelu.

    Jos käytät HTTPS, joka sisältää suojauksen käyttämällä MD5 tarkistus on tarpeettomat ja tarpeettomat.
    
    Lisätietoja on Katso Ota [Azure Blob MD5 yleiskatsaus](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).

2.  **Mitä tietoja FIPS-vaatimusten noudattaminen US Government?**

    Yhdysvallat tietojen käsittely Standard (FIPS Federal) määrittää salauksen algoritmit käytettäväksi hyväksymä Federal US government tietokonejärjestelmien suojaa luottamukselliset tiedot. FIPS ottaminen käyttöön Windows server tai työpöydän tilassa kertoo käyttöjärjestelmän, että vain FIPS vahvistettu salauksen algoritmit käytetään. Jos sovellus käyttää-yhteensopiva algoritmit-sovellukset lakkaavat toimimasta. With.NET Framework-versiot 4.5.2 tai uudempi versio, sovellus siirtyy automaattisesti salaus algoritmit käyttämään FIPS-yhteensopivaa algoritmit tietokoneen ollessa FIPS-tilassa.

    Microsoft ei kunkin asiakkaan valitseminen FIPS-tila käyttöön ylöspäin. Microsoft uskoo ei ole pakottavaa asiakkaille, jotka eivät ole government määräysten FIPS-tila käyttöön oletusarvoisesti.

    **Resurssit**

-   [Miksi emme ole ei suositellaan "FIPS-tilassa" enää](http://blogs.technet.com/b/secguide/archive/2014/04/07/why-we-re-not-recommending-fips-mode-anymore.aspx)

    Blogin tässä artikkelissa on yleiskatsaus FIPS ja kerrotaan, miksi hän ei FIPS tila käyttöön oletusarvoisesti.

-   [FIPS 140 vahvistus](https://technet.microsoft.com/library/cc750357.aspx)

    Tässä artikkelissa on tietoja siitä, miten Microsoft-tuotteiden ja salauksen moduulit mukaisia Federal US government FIPS-standardin.

-   ["Järjestelmän salaus: Käytä FIPS yhteensopivien algoritmien salauksen hajautuksessa ja allekirjoituksessa" suojauksen asetusten vaikutukset Windows XP: ssä ja uudemmissa versioissa Windows](https://support.microsoft.com/kb/811833)

    Tässä artikkelissa on lyhyt FIPS-tilassa vanhempia Windows-tietokoneissa käyttö.
