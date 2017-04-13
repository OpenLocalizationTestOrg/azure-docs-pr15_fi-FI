<properties
    pageTitle="Resurssien käytön hallinta Azure Active Directory-ryhmiin | Microsoft Azure"
    description="Opit käyttämään ryhmät Azure Active Directoryn hallinta käyttöoikeuden paikalliseen ja cloud sovellukset ja resurssit."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Resurssien käytön hallinta ja Azure Active Directory-ryhmä

Azure Active Directory (Azure AD) on kattava tunnistetietojen ja käytön hallintaratkaisu, joka sisältää lukuisia ominaisuuksia, hallita niiden käyttöä paikallisen ja cloud sovellukset ja resurssit, kuten Microsoft online-palvelut, kuten Office 365: ssä ja Microsoft SaaS sovellusten maailman. Tässä artikkelissa on yleisiä tietoja, mutta jos haluat aloittaa Azure AD-ryhmien käyttäminen juuri nyt, noudata [hallinta käyttöoikeusryhmät Azure AD](active-directory-accessmanagement-manage-groups.md). Jos haluat nähdä, miten voit Azure Active directory-ryhmien hallinta PowerShellin avulla voit lukea lisää tekstimuodossa [ryhmän hallinta Azure Active Directory esikatselu cmdlet-komennot](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Jos haluat käyttää Azure Active Directory, sinun on Azure-tili. Jos sinulla ei ole tiliä, voit [rekisteröityä maksuttoman Azure-tili](https://azure.microsoft.com/pricing/free-trial/).


Azure AD-yksi tärkeimmistä ominaisuuksista on mahdollisuus resurssien käyttöoikeuksien hallinta. Nämä resurssit voivat kuulua hakemiston, kuten käyttöoikeuksia objektit-kansiossa tai hakemiston, kuten SaaS sovellukset ja SharePoint-sivustojen Azure palvelujen tai paikalliseen resurssien ulkopuolisiin resursseihin rooleilla kirjainkokoa. Käyttäjä voi määrittää käyttöoikeuksia resurssille kolmella tavalla:


1. Suora osoittaminen

    Käyttäjä voi määrittää suoraan resurssin resurssin omistaja.

2. Ryhmäjäsenyys

    Ryhmän voi määrittää resurssin resurssin omistaja ja näin, myöntämistä resurssin käyttöoikeutta kyseisen ryhmän jäsenet. Valitse ryhmän omistaja voi hallita ryhmän jäsenyyden. Resurssien omistajan tehokkaasti, Delegoi oikeuden määrittää käyttäjiä niiden resurssin ryhmän omistajalle.

3. Roolipohjainen

    Resurssien omistajan express käyttäjät, jotka määritetään access resurssille säännön avulla. Säännön ulkoasusta määräytyy määritteitä, joita käytetään sääntöön ja niiden arvojen tietyiltä käyttäjiltä ja tekemällä resurssien omistajan Delegoi tehokkaasti oikeuden hallita niiden resurssin tärkeimpien lähde-käyttöoikeuden määritteet, joita käytetään sääntö. Resurssin omistaja hallitsee säännön itse edelleen ja määrittää, mitkä määritteet ja arvot tarjota niiden resurssien käytön.

4. Ulkoinen myöntäjä

    Resurssin käyttöoikeutta johdetaan ulkoiseen tietolähteeseen; esimerkiksi ryhmä, joka on synkronoitu tärkeimpien lähteen, kuten paikallisesta hakemistosta tai SaaS sovellus, kuten työpäivä. Resurssien omistajan määrittää ryhmän resurssin pääsy ja ulkoisen tietolähteen hallitsee ryhmän jäsenet.

  ![Yleiskatsaus access hallinta-kaavio](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>Katso video, jossa kerrotaan käyttöoikeushallinta

Voit katsoa lyhyt video, jossa kerrotaan enemmän aiheesta:

**Azure AD: Dynaaminen ryhmien jäsenyyden esittely**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Miten käyttää Azure Active Directory-työ hallinta?
Access hallintaratkaisu on keskellä olevaa Azure AD käyttöoikeusryhmän. Käyttöoikeusryhmän avulla voit hallita niiden käyttöä resursseja on tunnetun koaksiaalikaapeliin, joka mahdollistaa joustavan ja helposti ymmärrettäviä vielä antaa resurssin käyttöoikeutta tarkoitettu käyttäjäryhmään. Resurssien omistajan (tai järjestelmänvalvoja hakemiston) määrittää ryhmän tiettyjen käyttää oikealta ne ole resursseja. Ryhmän jäsenet antaa käyttöoikeuden ja resurssin omistaja voi delegoida oikeuden hallita toiselle henkilölle, kuten osaston esimies tai tukipalvelu järjestelmänvalvojan ryhmän jäsenten luettelon.

![Azure Active Directory access kaavio](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Ryhmän omistaja voi myös käytettäväksi kyseisen ryhmän Omatoiminen pyynnöt. Tällöin käyttäjä voi etsiä ja Etsi ryhmä ja tee pyyntö liittyä, hakemista tehokkaasti resursseja, joilla hallitaan käyttöoikeus ryhmän kautta. Ryhmän omistaja määrittää ryhmän niin, että liity pyynnöt hyväksytään automaattisesti, tai ryhmän omistaja hyväksyttävä. Kun käyttäjä tekee pyyntö liittyä ryhmään, liity-pyyntö on lähetetty omistajat-ryhmän. Jos jokin omistajat hyväksyy pyynnön, käyttäjällä on ilmoitettu ja käyttäjä on liitetty ryhmään. Jos jokin omistajat estää pyynnön, käyttäjällä on ilmoituksen mutta ei ole liitetty ryhmään.


## <a name="getting-started-with-access-management"></a>Käyttöoikeuksien hallinnan käytön aloittaminen
Oletko valmis aloittamaan? Kokeile, voit tehdä Azure AD-ryhmiin perustoiminnot. Näiden ominaisuuksien avulla voit antaa erityinen eri henkilöryhmille eri resurssien organisaation. Ensimmäiset perusvaiheet luettelo näkyvät jäljempänä.

* [Voit määrittää ryhmän dynaaminen jäsenyydet yksinkertaisen säännön luominen](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Ryhmän avulla voit hallita niiden käyttöä SaaS sovellukset](active-directory-accessmanagement-group-saasapps.md)

* [Saataville ryhmän Lopeta käyttäjän Omatoiminen](active-directory-accessmanagement-self-service-group-management.md)

* [Synkronointi paikallisen-ryhmän Azure Azure AD Connect käyttäminen](active-directory-aadconnect.md)

* [Ryhmän omistajien hallinta](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Seuraavat vaiheet access hallintaa varten
Nyt kun käyttöoikeuksien hallinnan perusteet on ymmärretty, seuraavassa on joitakin muita lisäominaisuuksia Azure Active Directoryn hallintaan sovellukset ja resurssit.

* [Luoda Monitasoisempia sääntöjä määritteet avulla](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Azure AD hallinta käyttöoikeusryhmät](active-directory-accessmanagement-manage-groups.md)

* [Määrittämään oma ryhmät Azure AD](active-directory-accessmanagement-dedicated-groups.md)

* [Ryhmien Graph API-viittaus](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Ryhmän asetusten määrittämiseen Azure Active Directory cmdlet-komennot](active-directory-accessmanagement-groups-settings-cmdlets.md)
