<properties
    pageTitle="Käyttäjän valmistelu Azure Active Directory-esikatselussa sovellusten hallinnan | Microsoft Azure"
    description="Opi hallitsemaan käyttäjän tilin valmistelu sovellusten Azure Active Directory-esikatselun käyttäminen"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Esikatselu: Hallinta käyttäjätilin valmistelu sovellusten uusi Azure-portaalissa

Tässä artikkelissa käsitellään [Azure portal](https://portal.azure.com) avulla voit hallita automaattinen käyttäjätilin valmistelu ja poista valmistelu sovelluksia, jotka tukevat sitä erityisen lähimpään kokonaislukuun, joka on lisätty "esitellyt" luokasta [Azure Active Directory-sovelluksen valikoima](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Tämä hallintatoimintojen uusi Azure-portaalissa on parhaillaan julkinen esikatselu ja tässä artikkelissa kuvataan uusista ominaisuuksista sekä seuraavat tilapäinen rajoitukset, jotka ovat paikallaan aikana esikatselu. [Mikä on esikatselussa?](active-directory-preview-explainer.md)

Lisätietoja automaattisen käyttäjän tilin valmistelu ja sen toiminnasta on artikkelissa [automatisoida käyttäjän valmistelu ja Deprovisioning SaaS sovellusten Azure Active Directory-hakemistosta](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Uuden portaalin sovelluksesi etsiminen

Saavuttaman syyskuu 2016 kaikissa sovelluksissa, jotka on määritetty single Sign-kansioon, [Azure Active Directory-sovelluksen gallery](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) sisällä [Azure perinteinen portal](https://manage.windowsazure.com)directory järjestelmänvalvojan, nyt voi tarkastella ja hallitun uusi Azure-portaalissa.

Nämä sovellukset löytyy **Yrityksen** osassa uuden Azure portaalin, jota voi käyttää vasemmanpuoleisessa siirtymisalueessa **Enemmän palvelut** -valikon kautta. Sovellusten ovat sovellukset, jotka on otettu käyttöön ja organisaatiosi käyttäjät ovat käytössä.

![Yrityksen sivu][0]

Kaikki sovellukset, jotka on määritetty, mukaan lukien sovellukset, jotka on lisätty valikoimasta luettelo valitsemalla **kaikki sovellukset** -linkki vasemmalla puolella näkyy. Resurssi-sivu, jossa raportteja voi tarkastella, sovelluksen ja erilaisia asetuksia voi hallita sovelluksen sovelluksen valitsemalla Lataa.

Käyttäjätilin valmisteluasetukset voidaan hallita valitsemalla **Provisioning** vasemmalla.

![Sovelluksen resurssi-sivu][1]


##<a name="provisioning-modes"></a>Tilojen valmisteleminen

**Provisioning** sivu alkaa **tila** -valikossa, joka näkyy, mitä valmistelu tilat ovat tuettuja enterprise-sovelluksen, ja sallii ne on määritettävä. Käytettävissä olevat vaihtoehdot ovat:

* **Automaattinen** - vaihtoehto näkyy tukeeko Azure AD automaattinen API-pohjaisen valmisteleminen ja/tai poistaa valmistelu sovellukselle-käyttäjätilien. Tässä tilassa valitseminen näyttää liittymää, joka ohjaa järjestelmänvalvojien määrittäminen Azure AD muodostaa sovelluksen Käyttäjähallinta API, tilin yhdistämismääritykset ja työnkulkuja, jotka määrittävät, kuinka käyttäjätilin tiedot flow välillä Azure AD luominen ja sovelluksen ja Azure AD valmistelu palvelun hallinta.

* **Manuaalinen** - vaihtoehto näkyy Jos Azure AD ei tue tämän sovelluksen käyttäjätilit Automaattinen valmisteleminen. Tämä asetus tarkoittaa, että käyttäjän asiakas-tietueiden tallennetaan sovelluksen vain Skype ulkoinen prosessi-käyttäjien hallinta ja valmistelu ominaisuuksia, myöntämä sovelluksen (joka voi sisältää SAML oikeaan aikaan tapahtuva valmistelu) perusteella.


##<a name="configuring-automatic-user-account-provisioning"></a>Määritä automaattinen käyttäjän tilin valmistelu

**Automaattinen** -vaihtoehdon valitseminen tuo näyttöön näyttö, jossa on jaettu seuraaviin osiin:

###<a name="admin-credentials"></a>Järjestelmänvalvojan tunnistetiedot
Tämä on missä tunnistetiedot vaaditaan Azure AD muodostaa sovelluksen Käyttäjähallinta API lisätään. Syöte pakollinen vaihtelee sen mukaan, sovellus. Tietoja tunnistetiedon tyypit ja sovelluksia vaatimukset-kohdassa [tietyn sovelluksen kokoonpanon opetusohjelma](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Valitsemalla **Testaa yhteys** -painikkeen avulla voit testata tunnistetiedot että Azure AD muodostaminen sovellus on valmistelu sovellusta annettuja tunnistetietoja.

###<a name="mappings"></a>Yhdistäminen
Tämä on, jossa järjestelmänvalvojat voivat tarkastella ja muokata tietoja käyttäjän välinen Azure AD-määritteitä tiedonkulku ja kohdesovelluksen käyttäjätilit valmistelun yhteydessä tai päivitetään.

Tällä Azure AD käyttäjän ja kunkin SaaS-sovelluksen käyttäjän objektien välisten yhdistämismääritysten esimääritettyjä joukko. Jotkin sovellukset hallita muita objekteja, kuten ryhmien tai yhteystietojen. Valitsemalla jonkin yhdistämismääritykset taulukko näyttää oikealta, voivat voi tarkastella ja mukauttaa yhdistäminen-editorin.

![Sovelluksen resurssi-sivu][2]

Tuetut mukautukset ensimmäisen esikatselun aikana ovat seuraavat:

* Ottaminen käyttöön ja poistaminen käytöstä tietyn objektit, kuten Azure AD-käyttäjäobjektin SaaS sovelluksen käyttäjän objektiin yhdistämismääritykset.

* Muokkaa, mitkä attribuutit flow Azure AD-käyttäjän objektista sovelluksen käyttäjäobjektin. Lisätietoja määritettä yhdistäminen on artikkelissa [tietoja määritettä yhdistäminen tiedostotyypit](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Voit suodattaa tietoja valmistelu toimintoja Azure AD kannattaa suorittaa kohdesovellus, joka on uusi ominaisuus Azure-portaalissa. Sen sijaan, että Azure AD synkronoida kaikkia objekteja, voit rajoittaa toimintoja. Esimerkiksi mukaan vain valitseminen **päivitys**-ja Azure AD vain olemassa olevaa käyttäjää sovelluksen tilit ja päivityksiä ei voi luoda uusia. Jos valitset vain **luominen**ja Azure vain Luo uusia käyttäjätilejä, mutta ei päivitä aiemmin luotuja. Tämän ominaisuuden avulla järjestelmänvalvojat voivat luoda eri yhdistämismääritykset tilin luominen ja päivittäminen työnkulut. Koko voi luoda useita yhdistämismäärityksiä app kohden on suunniteltu myöhemmin kaudella esikatselu.

###<a name="settings"></a>Asetukset
Tässä osassa avulla järjestelmänvalvojat voivat aloittaa ja Pysäytä palvelu valitun sovelluksen valmistelu Azure AD sekä voit myös poistaa valmistelu ja -palvelun käynnistäminen uudelleen.

Jos valmistelu on käytössä sovelluksen ensimmäisen kerran, ota käyttöön palvelun muuttamalla **Valmistelu tila** **käyttöön**. Tämä aiheuttaa Azure AD valmistelu palvelun suorittamiseen alkusynkronoinnin, jossa lukee määritetyt **käyttäjät ja ryhmät** -kohdassa käyttäjät, niiden kohdesovelluksen kyselyt ja suorittaa sitten valmistelu Azure AD- **yhdistämismääritykset** -osiossa määritetyt toiminnot. Tämän prosessin aikana valmistelu palvelu tallentaa välimuistiin tallennetut tiedot siitä, mitä Käyttäjätilien hallinta, niin hallinnalla suojatun tilit, jotka olivat koskaan laajuus varauksen kohdesovellusten sisällä ei koske valmistelu varaustiedoista toiminnot. Alkusynkronoinnin jälkeen valmistelu palvelun synkronoi käyttäjän ja Ryhmitä objektit-10 minuutin väli.

Valmistelun palvelun keskeyttää yksinkertaisesti muuttaminen **Valmistelu tilan** **käytöstä** . Tässä tilassa Azure ei luoda, Päivitä tai Poista käyttäjä tai Ryhmitä objektit-sovelluksessa. Kohtaan käytössä tilan muuttaminen aiheuttaa palvelu, johon se viimeksi jäit.

Valitsemalla **Poista nykyisen tilan ja käynnistää synkronoinnin uudelleen** -valintaruutu ja tallentaminen lopettaa valmistelu palvelun Vedostaa välimuistiin tallennetut tiedot siitä, mitä tilejä Azure AD hallinnoidaan, käynnistää palvelut uudelleen ja suorittaa ensimmäisen synkronoinnin uudelleen. Tämän asetuksen avulla järjestelmänvalvojat voivat aloittaa valmistelun käyttöönoton uudelleen.

###<a name="synchronization-details"></a>Synkronoinnin tiedot
Tässä osassa on lisäksi tietoja valmistelu palvelua, mukaan lukien valmistelu palvelun suoritettiin sovelluksen ja kuinka monta käyttäjää ja Ryhmitä objektit on hallittavan ensimmäisen ja viimeisen ajat toiminnan.

Linkit ovat **Provisioning käyttöraportti**mitkä tarjoaa loki kaikki käyttäjät ja ryhmät luodaan, päivitetyt ja poistetut välillä Azure AD ja sovelluksesta, ja **Provisioning virheraportin** , joka sisältää useamman kohteen yksityiskohtaiset virheilmoituksia käyttäjän ja ryhmän objekteja, jotka epäonnistui voi lukea, luoda, päivittää tai poistaa. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
