<properties
    pageTitle="Julkaise-sovelluksia ja Azure AD-sovelluksen välityspalvelimen | Microsoft Azure"
    description="Julkaise paikallisen sovellusten pilveen Azure AD-sovelluksen välityspalvelinta."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-using-azure-ad-application-proxy"></a>Julkaise sovellusten välityspalvelimen Azure AD-sovelluksen avulla

Välityspalvelimen Azure AD-sovelluksen avulla voit tukea remote työntekijöiden julkaisemalla paikallisen sovellusten Internetin kautta käytettäviä. Tämä pisteellä olisi sinulla on jo [käytössä sovelluksen välityspalvelimen Azure perinteinen-portaalissa](active-directory-application-proxy-enable.md). Tässä artikkelissa käydään läpi vaiheet ja julkaista sovelluksia, jotka on paikallisessa verkossa ja etäkäyttöä-verkon ulkopuolella. Kun olet suorittanut tämän artikkelin, voidaan ryhtyä määrittää sovelluksen Omat tiedot tai suojausvaatimukset.

> [AZURE.NOTE] Sovelluksen välityspalvelimen toimintoa, joka on käytettävissä vain, jos olet päivittänyt Premium- tai Basic Azure Active Directory-versiota. Lisätietoja on artikkelissa [Azure Active Directory-versiot](active-directory-editions.md).

## <a name="publish-an-app-using-the-wizard"></a>Julkaise sovelluksen käyttämällä ohjattua

1. Kirjaudu sisään järjestelmänvalvojana [Azure perinteinen portal](https://manage.windowsazure.com/).
2. Siirry Active Directory ja valitse kansio, jossa sovelluksen välityspalvelin on käytössä.

    ![Active Directory - kuvake](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Valitse **sovellukset** -välilehti ja valitse sitten näytön alalaidassa olevaa **Lisää** -painiketta

    ![Sovelluksen lisääminen](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Valitse **Julkaise jokin sovellus, joka voi käyttää verkon ulkopuolella**.

    ![Valinnaiseksi, jotka ovat käytettävissä verkon ulkopuolelta](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Anna sovelluksen seuraavat tiedot:

    - **Nimi**: sovelluksen käyttäjäystävällinen nimi. Sen on oltava yksilöivä hakemisto.
    - **Sisäinen URL-osoite**: osoite, joiden sovelluksen välityspalvelimen yhdistimen käyttää sovelluksen yksityinen verkoston sisällä. Voit antaa tietyt polku julkaista palvelimeen loput ollessa julkaisemattomien yhteyttä palvelimeen. Tällä tavalla voit julkaista eri sivustojen samaan palvelimeen ja antaa jokaiselle oma nimi ja access-sääntöjä.

        > [AZURE.TIP] Jos julkaiset polku, varmista, että se sisältää tarvittavat kuvien, komentosarjojen ja sovelluksen tyylisivuja. Esimerkiksi jos sovellus on https://yourapp/app ja käyttää osoitteessa https://yourapp/media kuvia, valitse olisi julkaiset https://yourapp/ polku.

    - **Preauthentication menetelmä**: kuinka sovelluksen välityspalvelimen tarkistaa käyttäjät ennen hänelle access-sovellukseen. Valitse avattavasta valikosta jokin vaihtoehdoista.

        - Azure Active Directory: Sovelluksen välityspalvelin ohjaa käyttäjät voivat kirjautua sisään Azure AD, joka todentaa annettujen käyttöoikeuksien hakemistosta ja sovelluksen.
        - Läpivienti: Käyttäjien ei tarvitse käyttää sovellusta todennusta.

    ![Sovelluksen ominaisuudet](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Viimeistele ohjattu toiminto valitsemalla valintamerkki näytön alareunassa. Sovellus on nyt määritetty Azure AD.


## <a name="assign-users-and-groups-to-the-application"></a>Määritä käyttäjät ja ryhmät-sovellukseen

Tilauksen käyttäjät voivat käyttää julkaistua-sovellusta sinun on joko yksitellen tai ryhmissä heille. (Muista liian määrittää itsellesi Accessin.) Tämä edellyttää, että kullekin käyttäjälle käyttöoikeuden Azure Basic tai uudempi versio. Voit määrittää käyttöoikeudet erikseen tai ryhmille. Saat lisätietoja [sovelluksen käyttäjien määrittäminen](active-directory-applications-guiding-developers-assigning-users.md) . 

Sovellukset, jotka edellyttävät esitarkistusta tämä myöntää käyttöoikeuksia-sovelluksella. Sovellusten, joka ei edellytä esitarkistusta käyttäjien edelleen voi määrittää sovelluksen niin, että se näkyy niiden sovellus-luettelossa, kuten Omat sovellukset.

1. Lisää sovellus ohjatun toiminnon jälkeen, näet sovelluksen Pika-aloitus-sivulla. Voit hallita, kenellä on oikeudet-sovellukseen, valitse **käyttäjät ja ryhmät**.

    ![Sovelluksen välityspalvelimen pikaopas määritettävä käyttäjille - näyttökuva](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Hae tiettyjen ryhmien hakemistossa tai Näytä kaikki käyttäjät. Etsinnän tulokset näkyviin napsauttamalla valintaruudun valinta.

    ![Etsi ryhmille tai käyttäjille - näyttökuva](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Valitse kunkin käyttäjän tai ryhmän, Määritä sovellus ja valitse **Liitä**. Sinua pyydetään vahvistamaan toiminnon.

> [AZURE.NOTE] Windows-todennusta-sovelluksia voit määrittää käyttäjät ja ryhmät, jotka on synkronoitu paikallisen Active Directorysta. Käyttäjät, jotka Kirjaudu sisään Microsoft-tili ja vieraiden ei voi määrittää sovellukset, jotka on julkaistu ja Azure Active Directory välityspalvelinta. Varmista, että käyttäjien Kirjaudu sisään, jotka ovat samassa toimialueessa kuin julkaiset sovelluksen osa.

## <a name="test-your-published-application"></a>Testaa julkaistu sovellus

Kun olet julkaissut sovelluksen, voit testata sen URL-osoite, jonka julkaisit siirtymällä. Varmista, että voit käyttää sitä, että se liiteohjausobjekti oikein ja että kaikki toimii odotetulla tavalla. Jos sinulla on ongelmia tai näkyviin tulee virhesanoma, kokeile [vianmääritysoppaan](active-directory-application-proxy-troubleshoot.md).

## <a name="configure-your-application"></a>Sovelluksen määrittäminen

Voit muokata julkaistua sovellukset tai määritä sivun lisäasetusten määrittäminen. Tällä sivulla voit mukauttaa sovelluksen nimen muuttaminen tai ladata logon. Voit hallita käyttösäännöt, kuten esitarkistusta menetelmää tai monimenetelmäisen todentamisen.

![Määritysten Lisäasetukset](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Kun olet julkaissut sovellusten Azure Active Directory sovelluksen välityspalvelimen, ne näkyvät luettelossa sovellusten Azure AD- ja voit hallita olemassa.

Jos poistat sovelluksen välityspalvelimen services kun olet julkaissut sovelluksia, ne eivät ole enää käytettävissä olevat yksityisen verkon ulkopuolella. Tämä ei poista sovellukset.

Voit tarkastella sovellus ja varmista, että se on käytettävissä, kaksoisnapsauta sovelluksen nimeä. Jos sovellusta ei ole käytettävissä sovelluksen välityspalvelu on poistettu käytöstä, näyttöön tulee varoitussanoma näytön yläreunaan.

Jos haluat poistaa sovelluksen, valitse sovellus luettelosta ja valitse sitten **Poista**.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Valinnaiseksi oman toimialuenimen käyttäminen](active-directory-application-proxy-custom-domains.md)
- [Yksi merkki ottaminen käyttöön](active-directory-application-proxy-sso-using-kcd.md)
- [Ehdollinen käytön käyttöön ottaminen](active-directory-application-proxy-conditional-access.md)
- [Vaateet huomioon sovellusten käyttäminen](active-directory-application-proxy-claims-aware-apps.md)

Uusimmat uutiset ja päivitykset Tutustu [sovelluksen välityspalvelin-blogi](http://blogs.technet.com/b/applicationproxyblog/)
