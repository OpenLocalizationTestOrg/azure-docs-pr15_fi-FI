<properties
    pageTitle="Azure AD Connect synkronointi: Parhaat käytännöt muuttaminen oletusarvon määrittäminen | Microsoft Azure"
    description="Tarjoaa parhaita käytäntöjä muuttaminen Azure AD Connect synkronointi oletusarvon määrittäminen."
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
    ms.date="08/22/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-best-practices-for-changing-the-default-configuration"></a>Azure AD Connect synkronointi: Parhaat käytännöt muuttaminen oletusarvon määrittäminen
Tässä ohjeaiheessa tarkoituksena on kuvaus tuetut ja joita ei tueta Azure AD Connect Synkronoi muutokset.

Azure AD Connect luoma määritys toimii "sellaisenaan" useimmat ympäristössä, jotka synkronoidaan paikallisen Active Directory Azure AD kanssa. Joissakin tapauksissa on kuitenkin tarvittavat muutosten joitakin erityisesti on tai vaatimus määritys.

## <a name="changes-to-the-service-account"></a>Palvelutilin muutokset
Azure AD Connect synkronointi suoritetaan ohjatun asennuksen luoma palvelutili. Tämä palvelutilin Pidot salausavaimet synkronointi käytetyn tietokannan. Se on luotu 127 merkkiä pitkä salasanalla ja salasana on määritetty ei voimassa.

- Se ei **tueta** muuttaminen tai palvelun tilin salasanan. Tällöin poistaa salauksen näppäimet ja palvelu ei ole voivat käyttää tietokantaa ja ei voi käynnistää.

## <a name="changes-to-the-scheduler"></a>Muutokset ajoitus
Muodosta 1.1-versioiden alkaen (helmikuu 2016) voit määrittää [ajoitus](active-directory-aadconnectsync-feature-scheduler.md) on eri synkronointi jakson kuin oletusarvoista 30 minuuttia.

## <a name="changes-to-synchronization-rules"></a>Muutosten synkronointi sääntöihin
Ohjattu asennus on määritys, joka on tarkoitettu sovellu tilanteissa. Siltä varalta, että haluat tehdä muutoksia työnkulkuun, sinun on noudatettava sääntöjen vielä tueta.

- Voit [muuttaa määritettä työnkulut](active-directory-aadconnectsync-change-the-configuration.md#other-common-attribute-flow-changes) , jos oletusarvon suoraan määrite työnkulut eivät sovellu organisaatiollesi.
- Jos haluat [työnkulun määrite](active-directory-aadconnectsync-change-the-configuration.md#do-not-flow-an-attribute) ja poistaa aiemmin määritteen arvoja Azure AD, sinun on tässä skenaariossa säännön.
- [Poista tarpeettomat synkronointi-sääntö käytöstä](#disable-an-unwanted-sync-rule) poistamisen sijaan se. Poistetun säännön luodaan päivityksen aikana.
- [Ruutuun säännön muuttaminen](#change-an-out-of-box-rule)Tee kopio alkuperäisestä säännöstä ja ruutuun säännön poistaminen käytöstä. Synkronoi säännön editorin kehottaa ja auttaa sinua.
- Vie mukautetun synkronoinnin sääntöjen synkronoinnin säännöt-editorin. Editorin avulla voit helposti luo ne uudelleen tietojen palauttaminen tilanne PowerShell-komentosarjaa.

>[AZURE.WARNING] Valinta synkronoinnin sääntöjen on allekirjoitus. Jos teet muutoksia sääntöjen, allekirjoitus ei enää vastaavat. Voit joutua ongelmia myöhemmin, kun yrität käyttää Azure AD Connect uuden version. Vain muuttaa tapaa, jolla on kuvattu tämän artikkelin.

### <a name="disable-an-unwanted-sync-rule"></a>Ei-toivottuja synkronointi-säännön poistaminen käytöstä
Älä poista valinta synkronointi-säännön. Se luodaan seuraavan päivityksen aikana.

Joissakin tapauksissa ohjattu asennus on valmistettu määrityksistä, jotka oman topologian ei toimi. Esimerkiksi jos tilin resurssi-metsää topologian mutta on laajennettu rakenne-tilin metsää Exchange rakenteen kanssa, valitse säännöt Exchange luodaan tilin metsää ja resurssin metsää. Tässä tapauksessa sinun täytyy synkronointi säännön käytöstä Exchange.

![Säännön käytöstä synkronointi](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

Yllä olevassa kuvassa ohjattu asennus löysi vanhan Exchange 2003-mallin tilin metsää. Rakenne-tunniste on lisätty ennen resurssin metsää otettiin Fabrikam's-ympäristössä. Jotta vanha Exchange-ympäristöstä ei attribuutit synkronoidaan synkronointi sääntö olisi käytöstä, esitetyllä.

### <a name="change-an-out-of-box-rule"></a>Ruutu säännön muuttaminen
Jos haluat tehdä muutoksia ruutuun säännön, voit kopioida ruutuun säännön ja alkuperäinen säännön käytöstä. Tee haluamasi muutokset kloonatun säännön. Synkronoi sääntö-editori on parannetaan nämä toimet. Avattaessa ruutuun sääntö on esitetty tässä valintaikkunassa:  
![Varoitus ruutuun säännön ulos](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Valitse **Kyllä** Jos haluat luoda säännön. Valitse kloonatun säännön avataan.  
![Kloonatun sääntö](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

Tämä kloonatun sääntö tehdä tarvittavat muutokset laajuus, Kutsu osallistujat ja muunnoksia.

## <a name="next-steps"></a>Seuraavat vaiheet

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
