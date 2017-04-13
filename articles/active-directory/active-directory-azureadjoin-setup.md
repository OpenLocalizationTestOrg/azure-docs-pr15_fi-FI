<properties
    pageTitle="Azure AD liittyä määrittäminen käyttäjien | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten järjestelmänvalvojat voivat määrittää määrittäminen Azure AD liittyä paikallista hakemistoa ja laitteen rekisteröinti."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Organisaation Azure AD liittyä määrittäminen

Ennen kuin määrität Azure Active Directory Join (Azure AD-liittyä), sinun täytyy synkronoida määrittäminen paikallista hakemistoa käyttäjien pilveen tai hallinnoitujen tilien luominen Azure AD manuaalisesti.

Yksityiskohtaiset ohjeet synkronointi paikallisen käyttäjille Azure AD käsitellään [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).


Manuaalisesti luominen ja käyttäjien hallinta Azure AD-viitata [Azure AD-käyttäjien hallinta](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Laitteen rekisteröinnin määrittäminen
1. Kirjaudu Azure-portaaliin järjestelmänvalvojana.
2. Valitse vasemmanpuoleisessa ruudussa **Active Directorysta**.
3. Valitse **kansio** -välilehdessä hakemistossa.
4. Valitse **määritys** -välilehti.
5. Siirry **laitteet** -osassa.
6. Määritä **laitteet** -välilehdessä seuraavasti:  
   * **Suurin numero ja laitteiden käyttäjää KOHDEN**: Valitse laitteet, jotka käyttäjä voi olla Azure AD enimmäismäärä.  Jos käyttäjä saavuttaa tämän kiintiön, hän voi lisätä muita laitteita, ennen kuin vähintään yksi aiemmin laitteensa poistetaan.
   * **Vaadi multi-FACTOR AUTH, liity laitteet**: Määritä, voivatko käyttäjät edellyttää todennusta toisen kerroin niiden laitteen liittäminen Azure AD. Lisätietoja Azure Monimenetelmäisen todentamisen on artikkelissa [Azure Monimenetelmäisen todentamisen pilveen käytön aloittaminen](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **Käyttäjät voivat AZURE AD-liity-laitteet**: Valitse käyttäjät ja ryhmät, jotka voivat liittyä laitteiden Azure AD.
   * **Lisää JÄRJESTELMÄNVALVOJAT edelleen AZURE AD LIITTYNEET laitteet**: Azure AD-Premium tai yrityksen Mobility Suite (EMS), voit valita, käyttäjät, jotka myönnetään laitteen paikallisen järjestelmänvalvojan oikeudet. Yleiset Järjestelmänvalvojat ja laitteen omistajat myönnetään paikallisen järjestelmänvalvojaoikeudet oletusarvoisesti.

<center>![Laitteen regisration määrittäminen](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Kun määrität Azure AD liity käyttäjien, he voivat muodostaa Azure AD yrityksen tai henkilökohtaiset laitteensa kautta.

Seuraavassa on kolme tilanteita, joissa voit ottaa käyttöön Azure AD liittyä määrittäminen käyttäjille:

- Käyttäjien liittää yrityksen omistamien laite suoraan Azure AD.
- Käyttäjät, toimialueen liity yrityksen omistamien laitteen paikallisen Active Directory ja laajentaa sitten laitteen Azure AD.
- Käyttäjät lisäävät työpaikan tai oppilaitoksen tilit Windows Omat laitteessa

## <a name="additional-information"></a>Lisätietoja
* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
