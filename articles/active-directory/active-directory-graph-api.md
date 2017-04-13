<properties
   pageTitle="Azure Active Directory-kaavion API | Microsoft Azure"
   description="Yleiskatsaus ja pikaopas oppaan Graph Ohjelmointirajapinnan joka mahdollistaa ohjelmallisesti Azure AD REST API päätepisteet kautta."
   services="active-directory"
   documentationCenter=""
   authors="PatAltimore"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-graph-api"></a>Azure Active Directory-kaavion Ohjelmointirajapinta

> [AZURE.IMPORTANT] Azure AD-kaavion API toiminnot ovat käytettävissä myös [Microsoft Graph](https://graph.microsoft.io/)-yhdistetyn API, joka sisältää API muiden Microsoft-palveluiden, kuten Outlook, OneDrive, OneNote, suunnittelusta ja Office Graph-käytettävissä yhden päätepisteen avulla ja yksittäisen käyttöoikeustietue kautta.

Azure Active Directory Graph-Ohjelmointirajapinnan tarjoaa ohjelmallisesti Azure AD REST API päätepisteet kautta. Sovellukset voivat käyttää Graph-Ohjelmointirajapinnan suorittamiseen luominen, lukeminen, päivittäminen ja poistaminen (CRUD) toimenpiteet directory tietoja ja objekteja. Esimerkiksi Graph-Ohjelmointirajapinnan tukee käyttäjän objektin seuraavat yhteiset toiminnot:

- Luo uusi käyttäjä-kansioon

- Hae käyttäjän yksityiskohtaisia ominaisuuksia, kuten niiden ryhmien

- Päivittää käyttäjän ominaisuuksia, kuten niiden sijainti ja puhelinnumero tai vaihtaa oman salasanansa

- Käyttäjän Ryhmäjäsenyyden Roolipohjainen käyttöoikeuksien tarkistaminen

- Käyttäjän tilin käytöstä tai poistaa sen kokonaan

Käyttäjän objektit voit tehdä muita objekteja, kuten ryhmiä ja sovellusten samankaltaisia toimintoja. Kutsu Graph-Ohjelmointirajapinnan hakemisto-sovelluksen rekisteröitävä Azure AD ja on määritetty sallimaan hakemiston. Tämä saavutetaan normaalisti – käyttäjän tai järjestelmänvalvojan hyväksyntä-työnkulku.

Aloita Azure Active Directory Graph-Ohjelmointirajapinnan käyttäminen on [Graph API pika-aloitusopas](active-directory-graph-api-quickstart.md)tai [Vuorovaikutteinen Graph API-oppaat](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).


## <a name="features"></a>Ominaisuudet

Graph-Ohjelmointirajapinnan sisältää seuraavat ominaisuudet:

- **REST API päätepisteet**: kaavion API on päätepisteet, joita voi käyttää käyttämällä vakio pyyntöjen koostuu RESTful palvelu. Graph-Ohjelmointirajapinnan tukee XML- tai Javascript Object Notation (JSON) sisältötyypit pyynnöt ja vastaukset. Lisätietoja on artikkelissa [Azure AD Graph REST API-viittaus](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- **Azure AD todentaminen**: jokaisen Graph-ohjelmointirajapinnan on voi todentaa liittämällä JSON Web suojaustunnuksen (JWT)-pyynnön Authorization-otsikko. Tunnuksessa on hankittu pyynnön tekeminen Azure AD-tunnuksen päätepiste ja tarjoamalla kelvolliset tunnistetiedot. Voit käyttää OAuth 2.0 asiakkaan tunnistetiedot kulun tai luvan koodin myöntää työnkulku hankkia tunnuksen, Soita kaavion. Lisätietoja [Azure AD-OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx).

- **Roolipohjainen käyttöoikeuksien (RBAC)**: käyttöoikeusryhmät käytetään suorittamaan RBAC Graph-Ohjelmointirajapinnan. Esimerkiksi jos haluat selvittää, onko käyttäjän tietyn resurssin käyttöoikeutta, sovelluksen osallistua [Tarkista Ryhmäjäsenyyden (transitiiviverbien)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) -toiminto, joka palauttaa arvon TOSI tai EPÄTOSI.

- **Eroavuus kyselyn**: Jos haluat tarkistaa muutokset hakemistossa kaksi ajanjaksojen eikä sinun tarvitse tehdä usein käytetyt kyselyt Graph-Ohjelmointirajapinnan välillä, voit soittaa eroavuus kysely. Pyyntö tällaista palauttaa vain edellisen eroavuus kyselypyynnön ja nykyisen pyynnön välillä tehdyt muutokset. Lisätietoja [Azure AD Graph API eroavuus kyselyn](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).

- **Hakemiston tunnisteet**: Jos kehität sovellus, joka on lukeminen tai tiedostoon yksilöivä hakemisto objektien ominaisuudet, voit rekisteröidä ja Graph-Ohjelmointirajapinnan käyttäminen tunniste-arvojen avulla. Esimerkiksi jos sovellus edellyttää Skype-tunnuksen ominaisuus kullekin käyttäjälle, voit rekisteröidä uuden ominaisuuden hakemiston, ja se on käytettävissä käyttäjän kaikki objektissa. Lisätietoja [Azure AD Graph API Directory rakenteen tunnisteet](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).

- **Käyttöoikeuksien käyttöalueet mukaan suojattu**: AAD Graph API paljastaa käyttöoikeus-alueet, jotka tukevat AAD tietoja suojatun/sitoutunut käyttöä ja tukee monenlaisia sovelluksen asiakastyyppien, mukaan lukien:
 - käyttöliittymä, joka on määritetty sisältävän delegoida tietoja kautta luvan access kirjautunut sisään käyttäjän (valtuutetun)
  - käyttäjät, jotka käyttävät sovelluksen: Määritä Roolipohjainen käyttöoikeuksien valvonta kuten palvelun/daemon asiakkaille (sovelluksen roolit)

    Sekä valtuutetun ja sovelluksen roolin käyttöoikeuksien käyttöalueet edustavat Graph-Ohjelmointirajapinnan näyttämiä oikeus ja voit pyytää asiakassovelluksissa sovelluksen rekisteröinnin käyttöoikeudet [Azure perinteinen portaalissa ominaisuuksia](https://manage.windowsazure.com). Asiakkaiden tarkistaa käyttöoikeuksien käyttöalueet myönnetty niihin poistamiseksi vastaanotettu käyttöoikeustietue valtuutetun käyttöoikeuksien laajuus ("scp")-ryhmän ja roolien ("roolit") ottaa sovelluksen roolin käyttöoikeudet. Lisätietoja [Azure AD Graph API käyttöoikeuksien käyttöalueet](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).


## <a name="scenarios"></a>Skenaariot

Graph-Ohjelmointirajapinnan ottaa käyttöön sovelluksen skenaarioita. Seuraavassa on yleisimpiä:

- **Yritystietojen rivi (yksi Alihallinta) sovelluksen**: Tässä skenaariossa yrityksen developer soveltuu organisaatiota, jolla on Office 365-tilauksen. Kehittäjän luomisesta web-sovelluksen appsien kanssa Azure AD tehtävien suorittamiseen tällaisten käyttöoikeuden määrittäminen käyttäjälle. Tämän tehtävän edellyttää käyttöoikeutta Graph-Ohjelmointirajapinnan niin developer rekisteri yksi vuokraaja Azure AD-sovelluksen ja määrittää luku- ja kirjoitusoikeudet Graph-Ohjelmointirajapinnan. Valitse sovellus on määritetty käyttämään Omat tunnistetiedot tai niiden kirjautumisvirheiden tällä hetkellä käyttäjän hankkia tunnuksen, Soita Graph-Ohjelmointirajapinnan.

- **Ohjelmiston palvelusovelluksen (usean Alihallinta)**: Tässä skenaariossa riippumattomat-toimittajan (ISV) kehittää isännöityä usean vuokraajan web-sovelluksen, joka tarjoaa käyttäjän hallintaominaisuudet muiden Azure AD käyttävissä organisaatioissa. Näiden ominaisuuksien käyttöön tarvitaan hakemiston objektien ja siten sovellus on Graph-Ohjelmointirajapinnan kutsu. Kehittäjä Rekisteröi sovelluksen Azure AD, määrittää sen edellyttävät luku- ja kirjoitusoikeudet Graph-Ohjelmointirajapinnan ja sitten mahdollistaa ulkoisen käytön niin, että muiden organisaatioiden voit hyväksyminen niiden directory sovelluksen käyttöä varten. Toisen organisaation käyttäjän todentaa sovelluksen ensimmäisen kerran, kun ne ovat haluamassasi suostumuksen-valintaikkuna, jossa sovellus pyytää käyttöoikeudet.  Myöntämistä suostumusta sitten avulla sovelluksen ne pyytänyt Graph ohjelmointirajapinnan käyttäjän kansion käyttöoikeudet. Lisätietoja suostumusta framework on artikkelissa [Yleistä suostumus puitteissa](active-directory-integrating-applications.md).

## <a name="see-also"></a>Katso myös

[Azure AD-kaavion API pika-aloitusopas](active-directory-graph-api-quickstart.md)

[AD Graph muut asiakirjat](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Azure Active Directory-Sovelluskehittäjän opas](active-directory-developers-guide.md)
