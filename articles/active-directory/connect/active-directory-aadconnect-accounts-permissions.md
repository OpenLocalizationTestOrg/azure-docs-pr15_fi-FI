<properties
   pageTitle="Azure AD Connect: Tilit ja oikeudet | Microsoft Azure"
   description="Tässä ohjeaiheessa kerrotaan käytetään ja luonut tilit ja tarvittavista käyttöoikeuksista."
   services="active-directory"
   documentationCenter=""
   authors="billmath"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/04/2016"
   ms.author="billmath"/>


# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Asiakkaat ja käyttöoikeudet
Azure AD Connect ohjattu asennus on kaksi eri poluista:

- Pika-asetuksia ohjattu toiminto edellyttää enemmän oikeuksia, niin, että sen asennuksen kokoonpanosi helposti tarvitsematta käyttäjien luominen tai määritä käyttöoikeudet erikseen.

- Mukautetut asetukset ohjattu toiminto on käytettävissä Lisää vaihtoehtoja ja asetuksia, mutta joissakin tapauksissa, joissa sinun on varmistettava, sinulla on oikeat käyttöoikeudet itse.

## <a name="related-documentation"></a>Aiheeseen liittyvät ohjeet
Jos lue ohjeet [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](../active-directory-aadconnect.md), seuraavassa taulukossa on linkkejä aiheeseen liittyviä ohjeita.

Aihe |  
--------- | ---------
Asenna Express-asetusten avulla | [Pika-asennus, Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Asenna mukautettu asetusten avulla | [Mukautetun asennuksen Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Dirsync-työkalun päivittäminen | [Päivittää Azure AD-synkronointityökalu (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)


## <a name="express-settings-installation"></a>Pika-asennus asetukset
Express-asetuksissa ohjattu asennus pyytää AD DS yrityksen järjestelmänvalvojan tunnistetietoja niin paikallisen Active Directory voidaan määrittää Azure AD Connect tarvittavia oikeuksia. Jos päivität DirSync-AD DS Yrityksen valvojat-tunnistetietoja käytetään käyttämä DirSync-tilin salasanan. Sinun on myös Azure AD yleisen järjestelmänvalvojan oikeudet.

Ohjatun toiminnon sivulla  | Kerätä tunnistetiedot | Tarvittavat oikeudet| Käytetään
------------- | ------------- |------------- |-------------
PUUTTUU|Käyttäjän ohjattu asennus| Paikallisen palvelimen järjestelmänvalvoja| <li>Luo paikalliseen tili, jota käytetään [sync engine palvelutilin](#azure-ad-connect-sync-service-account).
Azure AD yhdistäminen| Azure Active directory-käyttäjätiedot | Azure AD yleisen järjestelmänvalvojan rooli | <li>Azure Active directory-synkronoinnin käyttöönotto.</li>  <li>[Azure AD-tili](#azure-ad-service-account) , jota käytetään jatkuvaa synkronointi toimille Azure AD-luominen.</li>
Yhteyden muodostaminen AD DS: ÄÄN | Paikallisen Active Directory-tunnistetiedot | Yrityksen järjestelmänvalvojat (EA) Active Directory-ryhmän jäsen| <li>Luo [tili](#active-directory-account) Active Directoryn ja myöntää käyttöoikeuksia siihen. Tätä luonut tilin käytetään lukemiseen ja kirjoittamiseen directory tietojen synkronoinnin yhteydessä.</li>

### <a name="enterprise-admin-credentials"></a>Yrityksen järjestelmänvalvojan tunnistetietoja
Näitä tunnistetietoja käytetään vain asennuksen aikana, ja niitä käytetään, kun asennus on valmis. Yrityksen järjestelmänvalvoja ja ole toimialueen järjestelmänvalvoja, varmista, että kaikki toimialueet voi määrittää Active Directory käyttöoikeuksien on.

### <a name="global-admin-credentials"></a>Yleisen järjestelmänvalvojan tunnistetietoja
Näitä tunnistetietoja käytetään vain asennuksen aikana, eikä niitä käytetä kun asennus on valmis. Sillä voidaan luoda [Azure AD-tili](#azure-ad-service-account) , jota käytetään Azure AD synkronoiminen muutokset. Tilin avulla synkronoi myös Azure AD-ominaisuudeksi.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Luotu AD DS käyttöoikeuksien tili Expressin asetukset
Luotu lukemiseen ja kirjoittamiseen AD DS: ÄÄN [tilin](#active-directory-account) on oltava seuraavat oikeudet luotaessa express asetusten mukaan:

Käyttöoikeudet | Käytetään
---- | ----
<li>Toistaa kansion muutokset</li><li>Rinnakkaiset Directory muuttuu kaikki | Salasanan synkronointi
Luku-/ kirjoitusoikeudet kaikki ominaisuudet käyttäjä | Tuonti- ja Exchange hybrid
Luku-/ kirjoitusoikeudet kaikki ominaisuudet inetOrgHenkilö | Tuonti- ja Exchange hybrid
Luku-/ kirjoitusoikeudet kaikki ominaisuudet ryhmittely | Tuonti- ja Exchange hybrid
Ota kaikki ominaisuudet kirjoitus | Tuonti- ja Exchange hybrid
Salasanan vaihtaminen | Valmistelut salasanan takaisinkirjoituksen ottaminen käyttöön

## <a name="custom-settings-installation"></a>Mukautetut asetukset asennus
Kun käytät mukautetut asetukset, tili, jota käytetään yhteyden muodostaminen Active Directoryyn on luotava ennen asennusta. Tällä tilillä on myönnettävä käyttöoikeudet löytyvät [AD DS-tilin luominen](#create-the-ad-ds-account).

Ohjatun toiminnon sivulla  | Kerätä tunnistetiedot | Tarvittavat oikeudet| Käytetään
------------- | ------------- |------------- |-------------
PUUTTUU | Käyttäjän ohjattu asennus|<li>Paikallisen palvelimen järjestelmänvalvoja</li><li>Jos koko SQL Server-käyttäjän täytyy olla järjestelmänvalvojan (SA) SQL</li>| Luo paikalliseen tili, jota käytetään [sync engine palvelutilin](#azure-ad-connect-sync-service-account)oletusarvon mukaan. Kun järjestelmänvalvoja ei määritetä tietyn tilin luodaan vain tili.
Asenna synkronoinnin services-palvelun tili-vaihtoehto | AD tai paikallisen tilin tunnistetiedot | Käyttäjän käyttöoikeudet myönnetään ohjattu asennus | Jos järjestelmänvalvoja määrittää tilin, tili käytetään palvelutili synkronointi-palveluun.
Azure AD yhdistäminen | Azure Active directory-käyttäjätiedot| Azure AD yleisen järjestelmänvalvojan rooli| <li>Azure Active directory-synkronoinnin käyttöönotto.</li>  <li>[Azure AD-tili](#azure-ad-service-account) , jota käytetään jatkuvaa synkronointi toimille Azure AD-luominen.</li>
Muodostaa yhteyttä hakemistojen selaaminen | Kunkin metsää, joka on liitetty Azure AD paikallisen Active Directory-käyttäjätiedot | Käyttöoikeudet määräytyvät sen mukaan, mitä ominaisuuksia ottaminen käyttöön ja löytyvät [luominen AD DS-tili](#create-the-ad-ds-account) |Tämän tilin käytetään lukemiseen ja kirjoittamiseen directory tietojen synkronoinnin yhteydessä.
AD FS-palvelimet | Luettelon jokaisen palvelin-ohjatun toiminnon kerää tunnistetiedot, kun ohjattu käyttäjän kirjautumistiedot eivät riitä muodostaa | Toimialueen järjestelmänvalvoja | Asennus- ja AD FS palvelimen roolin määrittäminen.
Web-sovelluksen välityspalvelimien |Luettelon jokaisen palvelin-ohjatun toiminnon kerää tunnistetiedot, kun ohjattu käyttäjän kirjautumistiedot eivät riitä muodostaa | Paikallisen järjestelmänvalvojan kohde tietokoneeseen. | Asentaminen ja määrittäminen WAP rooli.
Salli käyttäjätietojen |Federation palvelu luottaa tunnistetiedot (tunnistetiedot välityspalvelimen käyttää Rekisteröi FS luota todistus |Toimialuetili, jolla on paikallinen AD FS-palvelimen järjestelmänvalvoja | Alkuperäinen rekisteröinti FS WAP luota varmenteen.
AD FS-palvelutili-sivulla, "Käytä toimialueen tilin käyttäjäasetus" | AD käyttäjän tilin tunnistetiedot | Toimialuekäyttäjä | Active Directory-käyttäjätiliä, jonka tunnistetietoja anneta, käytetään kirjautumistili AD FS-palvelun.

### <a name="create-the-ad-ds-account"></a>AD DS-tilin luominen
Kun asennat Azure AD Connect, **muodostaa yhteyttä hakemistoja** -sivulla määrittämäsi tilin on sijaittava Active Directoryn ja on tarvittavat käyttöoikeudet. Ohjattu asennus ei tarkista käyttöoikeudet ja mahdolliset ongelmat löytyvät vain synkronoinnin yhteydessä.

Käyttöoikeudet, jotka tarvitset määräytyy valinnaisia ominaisuuksia otetaan käyttöön. Jos sinulla on useita toimialueita, kaikki toimialueiden on myönnettävä käyttöoikeudet. Jos et anna näiden ominaisuuksien oletusarvoisen **Toimialueen** käyttöoikeudet on riittävä.

Toiminto | Käyttöoikeudet
------ | ------
Salasanan synkronointi | <li>Toistaa kansion muutokset</li>  <li>Rinnakkaiset Directory muuttuu kaikki
Exchange-yhdistelmäratkaisu | Kirjoitusoikeudet määritteet kuvattu [Exchange hybrid takaisinkirjoituksen](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) käyttäjiä, ryhmiä ja yhteystiedot.
Salasanan takaisinkirjoituksen | Käyttäjien [salasanojen hallinta aloittaminen](../active-directory-passwords-getting-started.md#step-4-set-up-the-appropriate-active-directory-permissions) kuvattu määritteet kirjoitusoikeudet.
Laitteen takaisinkirjoituksen | Käyttöoikeudet ja PowerShell-komentosarjaa kuvatulla tavalla [laitteen takaisinkirjoituksen](../active-directory-aadconnect-feature-device-writeback.md).
Ryhmän takaisinkirjoituksen | Lue, luominen, päivittäminen ja poistaminen Ryhmitä objektit, joissa jaot ryhmät tulisi OU.

## <a name="upgrade"></a>Päivittäminen
Kun päivität Azure AD Connect yhdestä versiosta uuteen versioon, tarvitset seuraavat oikeudet:

Lainan | Tarvittavat oikeudet | Käytetään
---- | ---- | ----
Käyttäjän ohjattu asennus | Paikallisen palvelimen järjestelmänvalvoja | Päivitä binaaritiedostot.
Käyttäjän ohjattu asennus | ADSyncAdmins jäsen | Muutosten synkronointi säännöt ja muiden asetusten.
Käyttäjän ohjattu asennus | Jos käytössäsi on koko SQL server: DBO (tai vastaava) sync engine tietokannan | Varmista tietokannan tason muutoksia, kuten uusien sarakkeiden taulukot päivitetään.

## <a name="more-about-the-created-accounts"></a>Lisätietoja luodut tilit

### <a name="active-directory-account"></a>Active Directory-tili
Jos käytät Expressin asetukset, tili luodaan Active Directoryssa, jota käytetään synkronointia varten. Luotu tili sijaitsee metsää päätoimialue käyttäjien säilössä ja on nimessä etuliite **MSOL_**. Tili on luotu pitkään monimutkaisia salasanalla, joka ei vanhene. Jos sinulla Salasanakäytäntö toimialueen, varmista, että pitkä ja monimutkainen salasanat voivat tämän tilin.

![AD-tili](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

### <a name="azure-ad-connect-sync-service-accounts"></a>Azure AD Connect synkronointi palvelutilejä
Paikallisen palvelutilin luoma ohjattu asennus (Jos et määritä tili, jota käytetään mukautettuja asetuksia). Tili on etuliite **AAD_** ja niitä käytetään todellinen synkronointi-palvelun suoritetaan. Jos olet asentanut Azure AD Connect toimialueen ohjauskoneen, tili luodaan toimialueen. Jos käytät SQL Serveriä tai jos käytät välityspalvelin, joka edellyttää käyttöoikeuden tarkistusta etäpalvelimeen, toimialue on sijaittava **AAD_** palvelutilin.

![Synkronoi palvelutili](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Tili on luotu pitkään monimutkaisia salasanalla, joka ei vanhene.

Tämän tilin käytetään muiden tilien salasanojen tallentaminen turvallisesti. Näiden tilien salasanojen on tallennettu salataan tietokannan. Salauksen services salaisuus avaimen salaus käyttämällä Windows tietojen suojauksen API (DPAPI) on suojattu salausavaimet yksityiset avaimet. Olisi ei palauta palvelutilin salasanan, koska Windows sitten poistaa kaikki tietoturvasyistä salausavaimet.

Jos käytät koko SQL Server-palvelutili on sync engine luodun tietokannan DBO. Palvelu ei toimi halutulla muiden käyttöoikeuksien avulla. SQL-kirjautuminen luodaan.

Tili on myös myöntää käyttöoikeuksia-tiedostoja, rekisteriavaimia ja muiden objektien liittyvän synkronointi-ohjelma.

### <a name="azure-ad-service-account"></a>Azure AD-palvelutili
Luodaan uusi tili Azure AD-synkronointipalvelun käyttöä varten. Tämä tili voidaan tunnistaa sen näyttönimi.

![AD-tili](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

Toisen osan käyttäjänimi tunnistaa tilin käytetään palvelimen nimi. Kuva-palvelimen nimi on FABRIKAMCON. Jos sinulla on väliaikainen palvelimissa, kunkin palvelimen on oma tili. Azure AD on enintään 10 Synkronoi palvelutilejä.

Palvelutili luodaan pitkään monimutkaisia salasanalla, joka ei vanhene. Se on myönnetty erityisiä roolin **Directory-synkronoinnin tilit** , jotka on vain ne käyttöoikeudet, kansion synkronoinnin tehtävien suorittamiseen. Erityiset valmiin roolilla ei voi myöntää Azure AD Connect ohjatun toiminnon ulkopuolella ja Azure portaalin näyttää roolin **käyttäjän**kanssa tähän tiliin.

![AD-tilin rooli](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccountrole.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](../active-directory-aadconnect.md).
