<properties
    pageTitle="Ota käyttöön yrityksen tilan Roaming Azure Active Directoryn | Microsoft Azure"
    description="Usein kysyttyjä kysymyksiä Windows laitteet yrityksen tilan Roaming asetukset. Yrityksen tilan Roaming tarjoaa käyttäjille kaikissa laitteissa, Windows yhtenäinen käytettävyyttä ja vähentää määrittäminen uuden laitteen tarvittava aika."
    services="active-directory"
    keywords="yrityksen tilan roaming, Windowsin cloud yrityksen tilan roaming ottaminen käyttöön"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>



# <a name="enable-enterprise-state-roaming-in-azure-active-directory"></a>Ota käyttöön yrityksen tilan Azure Active Directory-palvelimesta

Yrityksen tilan Roaming on käytettävissä minkä tahansa organisaation Premium Azure Active Directory (Azure AD)-tilaus. Lisätietoja saaminen Azure AD-tilaus on artikkelissa [Azure AD-tuotesivuun](https://azure.microsoft.com/services/active-directory).

Kun otat yrityksen tilan Roaming, organisaation automaattisesti myönnetään käyttöoikeuksien lisääminen tilaukseen maksuton, rajoitettu käyttö voi Azure Rights Management. Tämä ilmainen tilaus on rajoitettu ja salauksen yrityksen asetukset ja tiedot synkronoitu yrityksen tilan Roaming-palvelun; tarvitset maksetun tilauksen Azure Rights Management koko ominaisuuksia.

Saatuaan Premium Azure AD-tilaus, voit ottaa käyttöön yrityksen tilan Roaming seuraavasti:

1. Kirjaudu sisään Azure perinteinen-portaaliin.
2. Vasemmassa reunassa Valitse **ACTIVE DIRECTORY**ja valitse sitten kansio, jonka haluat ottaa käyttöön yrityksen tilan Roaming.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming.png)
3. Siirry alkuun **määritys** -välilehti.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-configure.png)
4.  Vieritä sivua alaspäin ja valitse **käyttäjät voivat SYNKRONOIDA asetukset ja yrityksen Sovelluksen tiedot**ja valitse sitten **Tallenna**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-select-all-sync-settings.png)

Windows 10-laitteen siirtymisen yrityksen tilan Roaming-palvelun asetukset laite on todentaa käyttämällä Azure AD-tunnusta. Laitteille, jotka ovat liittyneet Azure AD-käyttäjän ensisijaisen sisäänkirjautuminen on Azure AD-tunnistetiedot, joten tarvitaan lisämäärityksiä. IT-järjestelmänvalvoja on laitteita, jotka käyttävät perinteinen paikallisen Active Directory- [yhteyden Azure AD for Windows 10: ssä kokemukset toimialueeseen liittyneet laitteet](active-directory-azureadjoin-devices-group-policy.md).

## <a name="sync-data-storage"></a>Synkronoi tietojen tallentaminen
Yrityksen tilan Roaming tietojen nykyisessä vähintään yksi [Azure alueet](https://azure.microsoft.com/regions/ ) , jotka parhaiten Tasaa määrittäminen Azure Active Directory-esiintymä maa/alue-arvolla. Yrityksen tilan Roaming tiedot osioidaan perusteella kolme tärkeintä maantieteellisillä alueilla: Pohjois-Amerikka, Afrikan ja APAC. Yrityksen tilan Roaming tietojen vuokraajan sijaitsee paikallisesti maantieteellinen alue ja ei replikoida eri alueilla.  Esimerkiksi asiakkaat, joilla on niiden maa/alue-kohdan arvoksi jokin Afrikan maissa, kuten "Ranska" tai "Sambia" on yhdessä tai Europe Azure alueiden tiedot.  Asiakkaat, joiden määrittäminen Azure AD johonkin Pohjois-Amerikka maiden niiden maa/alue-arvo, kuten "Yhdysvallat" tai "Kanada" on tietonsa pitäminen yhdessä tai useammassa USA Azure alueiden.  Asiakkaat, joiden määrittäminen Azure AD johonkin APAC maat niiden maa/alue-arvo, kuten "Australia" tai "Uusi-Seelanti" on tietonsa pitäminen yhdessä tai useammassa Aasian Azure alueiden.  Etelä-Amerikan maiden ja Antarktis tietojen isännöityjen vähintään yksi USA Azure alueita.  Maa/alue-arvo on määritetty Azure Active directory luomisen yhteydessä ja ei voi muokata myöhemmin. 

Jos tarvitset lisätietoja tietojen tallennuspaikka, tiedoston lippu kanssa [Azure tukevat](https://azure.microsoft.com/support/options/).

## <a name="manage-enterprise-state-roaming"></a>Yrityksen tilan Roaming hallinta
Azure AD Yleiset järjestelmänvalvojat voivat ottaminen käyttöön ja käytöstä yrityksen tilan Roaming Azure perinteinen-portaalissa.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-manage.png)

Yleiset järjestelmänvalvojat rajoittaa asetuksia Synkronoi tietyn käyttöoikeusryhmät.

Yleiset järjestelmänvalvojat voit myös tarkastella käyttäjäkohtainen laitteen synkronointi tilaraportti valitsemalla tietyn käyttäjän **Active Directory-esiintymä käyttäjäluettelo** ja valitsemalla **laitteet** -välilehti ja valitsemalla Näytä **laitetta käytettäessä asetukset ja yrityksen sovelluksen tiedot**.
![](./media/active-directory-enterprise-state-roaming/active-directory-enterprise-state-roaming-device-sync-settings.png)

##<a name="data-retention"></a>Tietojen säilytys
Tiedot synkronoituvat Azure kautta yrityksen tilan Roaming säilytetään jatkuvasti, ellei manuaalinen poistotoiminto suoritetaan tai kyseiset tiedot määritetään vanhentuneiden. 

**Eksplisiittinen poistaminen:** Tiedot poistetaan, kun Azure järjestelmänvalvoja poistaa käyttäjän tai hakemiston tai järjestelmänvalvoja pyytää erikseen, että tiedot ovat poistetaan.

- **Käyttäjän poistaminen**: kun käyttäjä poistetaan Azure AD-käyttäjätilin tiedot roaming poistettavaksi merkityt ja poistetaan 90 180 päivää. 
- **Kansion poistaminen**: poistaminen koko hakemistosta Azure AD on heti toiminto. Kaikki asetukset tiedot liitetyn, kansion poistettavaksi merkityt ja poistetaan 90 180 päivää. 
- **Pyyntö poistetut**: Jos Azure AD-järjestelmänvalvojan haluaa tietyn käyttäjän tiedot tai asetukset tietojen poistaminen manuaalisesti, järjestelmänvalvojan tiedoston lippu [tukevat Azure](https://azure.microsoft.com/support/)kanssa. 

**Vanhentuneiden tietojen poisto**: tiedot, joita on käytetty ei vuosi ("säilytysaika") käsitellään ajan tasalla olevia ja azuren poistetaan. Säilytysaika sitä saatetaan muuttaa, mutta eivät ole alle 90 päivää. Vanhentuneita tietoja voi olla Windows-sovelluksen asetukset- tai kaikki asetukset käyttäjän tietyt. Esimerkki:
 
- Jos laitteita käyttää tiettyjä asetuksia sivustokokoelman (esimerkiksi jokin sovellus poistetaan laitteesta tai asetukset-ryhmässä, kuten "Teema" on poistettu käytöstä kaikkien käyttäjän laitteet), ja valitse kyseisen sivustokokoelman tulee vanhentuneiden säilytysaika jälkeen ja poistetaan. 
- Jos käyttäjä on poistettu käytöstä asetusten synkronointi kyseistä laitteissa, sitten asetukset-tiedot voi käyttää, ja kaikki asetukset kyseisen käyttäjän tiedot tulee vanhentuneita ja voi poistaa säilytysaika jälkeen. 
- Jos Azure Active directory-järjestelmänvalvoja poistaa käytöstä yrityksen tilan Roaming koko kansion, valitse kaikki käyttäjät directory ne eivät synkronoidu enää asetukset ja kaikki asetukset tiedot kaikille käyttäjille tulee vanhentuneita ja voi poistaa säilytysaika jälkeen. 

**Poistetut tietojen palauttaminen**: tietojen arkistointikäytäntöjen ei voi muuttaa. Kun tiedot on poistettu pysyvästi, se ei palautettavissa. Kuitenkin on tärkeää muistaa, että asetukset tiedot poistetaan ainoastaan Azure, ei käyttäjän laite. Jos laitteeseen myöhemmin muodostaa uudelleen yhteyden yrityksen tilan Roaming-palvelun-asetukset uudelleen synkronoidut ja Azure tallennetaan.


## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita
- [Yrityksen tilan Roaming yleiskatsaus](active-directory-windows-enterprise-state-roaming-overview.md)
- [Asetukset ja tiedot roaming usein kysytyt kysymykset](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Oletuskäytäntö- ja Mobiililaitteiden ryhmäasetusten asetukset-synkronointia varten](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
- [Windows 10: ssä sijaitsevan asetukset viittaus](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
