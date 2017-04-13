<properties
    pageTitle="Azure AD Connect synkronointi: tietoja ja mukauttaa synkronoinnin | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten Azure AD Connect synkronointi toimii sekä kuinka voit mukauttaa."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi
Azure Active Directory-yhteyden-synkronointipalvelut (Azure AD Connect sync) on Azure AD Connect tärkeimmät osa. Vie huolellisesti, kaikki toiminnot, jotka liittyvät Synkronoi käyttäjätiedot paikallisen ympäristön ja Azure AD välillä. Azure AD Connect synkronointi on seuraaja DirSync Azure AD-synkronointi ja Forefront käyttäjätietojen hallinta ja Azure Active Directory Connectorin määritetty.

Tässä aiheessa on **Azure AD Connect synkronointi** (kutsutaan myös **sync engine**) aloitus ja linkit muiden siihen liittyviä ohjeita. Saat linkkejä Azure AD Connect [integrointi paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).

Synkronoi-palvelu sisältää kaksi komponenttia, paikallisen **Azure AD Connect synkronointi** -osa ja palveluntarjoaja kutsutaan **Azure AD Connect synkronointipalvelun**Azure AD. Palvelu on yleisiä Dirsync-työkalun, Azure AD-synkronointi ja Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect synkronointi aiheita

Aihe | Se peittää ja milloin lukeminen
----- | -----
**Azure AD Connect synkronoinnin perusteet** |
[Tietoja arkkitehtuuri](active-directory-aadconnectsync-understanding-architecture.md) | Käyttäjille, jotka ole aiemmin käyttänyt sync engine ja haluat lisätietoja arkkitehtuuri ja sanaston olette.
[Tekniset käsitteitä](active-directory-aadconnectsync-technical-concepts.md) | Arkkitehtuuri aiheen ja lyhyesti tiivistettynä kerrotaan sanaston.
[Azure AD topologioissa yhdistäminen](active-directory-aadconnect-topologies.md) | Tässä artikkelissa kuvataan eri topologioissa ja synkronoi-ohjelma tukee skenaarioita.
**Mukautettu määritys** |
[Ohjattu asennus uudelleen](active-directory-aadconnectsync-installation-wizard.md) | Tässä artikkelissa kerrotaan, mitä vaihtoehdot ovat käytettävissä, kun suoritat Azure AD Connect ohjattu asennus uudelleen.
[Tietoja määritettäviä valmistelu](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| Tässä artikkelissa kuvataan kutsutaan määritettäviä valmistelu määritysmalli.
[Tietoja määritettäviä valmistelu lausekkeet](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | Kuvaa käytetään määritettäviä valmistelun lausekielellä syntaksi.
[Tietoja oletusarvon määrittäminen](active-directory-aadconnectsync-understanding-default-configuration.md)| Tässä artikkelissa kuvataan ruutuun säännöt ja oletusarvo-määritys. Myös kuvataan, kuinka sääntöjä käytetään yhdessä ruudussa skenaariot toimimaan.
[Tietoja käyttäjien ja yhteystiedot](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Jatkuu edelliseen aiheeseen ja kuvataan, miten käyttäjät ja yhteystiedot siten, että toimii yhdessä, erityisesti usean metsää ympäristössä.
[Voit muuttaa oletusarvoiset määritykset](active-directory-aadconnectsync-change-the-configuration.md) | Esitellään muuttaminen määrite kulkee yleisiä määrityksen poistamisesta.
[Parhaat käytännöt muuttaminen oletusarvon määrittäminen](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Tue rajoitukset ja tehdä muutoksia työnkulkuun ruutuun.
[Määritä suodatus](active-directory-aadconnectsync-configure-filtering.md) | Tässä artikkelissa kuvataan, miten haluat rajata erilaisia vaihtoehtoja, mitkä objektit on parhaillaan synkronoidaan Azure AD- ja vaiheittaiset ohjeet näiden asetusten määrittämisestä.
**Ominaisuudet ja skenaariot** |
[Estä vahingossa poistot](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | Tässä artikkelissa kuvataan *estää vahingossa poistaa* -ominaisuus ja sen määrittäminen.
[Ajoitus](active-directory-aadconnectsync-feature-scheduler.md) | Tässä artikkelissa kuvataan valmiin aikataulutus, joka on tuominen, synkronointi ja tietojen vieminen.
[Ota käyttöön salasanan synkronointi](active-directory-aadconnectsync-implement-password-synchronization.md) | Tässä artikkelissa kuvataan salasanojen synkronoinnin toiminta, ottamisesta käyttöön ja toimia ja vianmäärityksestä.
[Laitteen takaisinkirjoituksen](active-directory-aadconnect-feature-device-writeback.md) | Tässä artikkelissa kuvataan laitteen takaisinkirjoituksen toiminta Azure AD Connect.
[Hakemisto-laajennukset](active-directory-aadconnectsync-feature-directory-extensions.md) | Tässä artikkelissa käsitellään laajentaa omia mukautettuja määritteitä Azure AD rakenne.
**Synkronoi-palvelu** |
[Azure AD Connect synkronoinnin ominaisuudet](active-directory-aadconnectsyncservice-features.md) | Tässä artikkelissa kuvataan synkronointi-palvelun reunassa ja Azure AD-synkronointiasetusten muuttamisesta.
[Kaksoiskappaleiden määrite vikasietoisuudelle](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | Tässä artikkelissa käsitellään ja **userPrincipalName** ja **proxyAddresses** päällekkäinen määritteen arvot vikasietoisuudelle käyttämisestä.
**Toimintojen ja Käyttöliittymä** |
[Synkronoinnin palvelun hallinta](active-directory-aadconnectsync-service-manager-ui.md) | Tässä artikkelissa kuvataan synkronoinnin palvelun hallinta-Käyttöliittymä, mukaan lukien [Toiminnot](active-directory-aadconnectsync-service-manager-ui-operations.md), [yhdistimiä](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse suunnittelu](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)ja [Metaverse haku](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) -välilehdet.
[Toiminnallisia tehtäviä ja huomioon otettavia seikkoja](active-directory-aadconnectsync-operations.md) | Tässä artikkelissa kuvataan toiminnallisia huolenaiheita, kuten palauttaminen.
**Miten tehdään...** |
[Palauttaa Azure AD-tili](active-directory-aadconnectsync-howto-azureadaccount.md) | Voit palauttaa Azure AD-Azure AD Connect synkronointi muodostamisessa käytettävä palvelutilin tunnistetiedot.
**Lisätietoja ja viittaukset** |
[Portit](active-directory-aadconnect-ports.md) | Näyttää portit sinun täytyy avata Synkronoi-ohjelma ja paikallisen kansioiden ja Azure AD välillä.
[Azure Active Directory-synkronoitavien määritteet](active-directory-aadconnectsync-attributes-synchronized.md) | Näyttää kaikki määritteitä, jotka on synkronoitu paikallisen välillä AD ja Azure AD.
[Funktiot viittaus](active-directory-aadconnectsync-functions-reference.md) | Näyttää kaikki Funktiot, jotka ovat käytettävissä määritettäviä valmistelu.

## <a name="additional-resources"></a>Lisäresursseja

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
