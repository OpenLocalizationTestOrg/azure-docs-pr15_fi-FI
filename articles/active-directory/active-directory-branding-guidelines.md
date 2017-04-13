<properties
   pageTitle="Ohjeet mukauttaminen sovellusten | Microsoft Azure"
   description="Kehittäjän aloittaminen resurssien Azure Active Directory täydellinen opas"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/23/2016"
   ms.author="mbaldwin"/>


# <a name="branding-guidelines-for-applications"></a>Sovellusten liittyviä ohjeita


Tässä ohjeaiheessa kerrotaan liittyviä kannattaa käyttää kehityssovellusten Azure Active Directory (Azure AD) ohjeita. Näiden ohjeiden avulla suoraan asiakkaille, kun he haluavat käyttää niiden työpaikan tai oppilaitoksen tiliä, hallita Azure AD- tai niiden Oma tili kirjautumista ja kirjaudu sisään sovelluksen.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Henkilökohtaiset tilit ja työ- tai oppilaitostileiltä Microsoftilta

Microsoft hallitsee käyttäjätilit kahdenlaisia:

- **Henkilökohtaiset tilit** (aikaisemmin Windows Live ID). Näissä tileissä esittävät *yksittäisten* käyttäjien ja Microsoft välinen suhde ja avulla voidaan käyttää kuluttaja laitteet ja palvelut Microsoftilta. Näissä tileissä on tarkoitettu käyttöäsi varten.

- **Työpaikan tai oppilaitoksen tilit.** Näissä tileissä hallitaan Microsoft Azure Active Directory käyttävissä organisaatioissa puolesta. Näissä tileissä käytetään Kirjaudu sisään Microsoft Office 365: ssä ja muut business-palvelut.

Microsoft työpaikan tai oppilaitoksen tilit on yleensä määritetty loppukäyttäjät (työntekijät, opiskelijoille, federal työntekijöiden) niiden organisaatioiden (yrityksen, koulun hallitus). Näissä tileissä on joko suoraan pilvipalvelussa, Azure AD-alustalle selvittänyt tai synkronoida Azure AD paikallisen, esimerkiksi Windows Server Active Directory-hakemistosta. Microsoft on työpaikan tai oppilaitoksen tilit *custodian* , mutta tilit omistaa ja hallinnoida organisaatiota.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Viittaaminen sovelluksen Azure AD-tilit

Microsoft ei Näytä loppukäyttäjien Azure Active Directory-brändiä nimet tai ei kumpaakaan, sinun kannattaa.

- Kun käyttäjä on kirjautunut sisään, voit käyttää organisaation nimi ja logo mahdollisimman paljon. Tämä on parempi kuin käyttämällä yleinen ehtoja, kuten "organisaation".

- Kun käyttäjät ole kirjautunut sisään, katso niiden asiakkaat "työpaikan tai oppilaitoksen tilit" ja esittää näissä tileissä hallitsee Microsoft käyttämällä Microsoft-logo. Älä käytä ehtoja, kuten "yrityksen tiliä", "Yritystili" tai "Yritystili" joka käyttäjän sekaannusta.

## <a name="user-account-pictogram"></a>Käyttäjän tilin merkitsevä-kuvake
Nämä ohjeet aiemmassa versiossa on suositeltavaa, "siniset merkin" niiden avulla. Käyttäjä- ja developer palautteen perusteella nyt suosittelemme Microsoft-logo käyttö sijaan. Tämä auttaa käyttäjiä ymmärtämään, että ne uudelleen tiliä ne Office 365: ssä tai muuta Microsoft business services kirjautumaan sovelluksen.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Kirjautuminen ja Azure AD kirjautuminen

Sovelluksen voi esittää erillisessä polut kirjautumista ja kirjaudu sisään ja seuraavissa osissa on visuaaliset ohjeet molempia.

**, Jos sovellus tukee peruskäyttäjän Kirjaudu ylöspäin (kuten ilmainen kokeiluversio tai freemium malliin)**: Voit näyttää **Kirjaudu sisään** -painike, jonka avulla käyttäjät voivat käyttää sovelluksen niiden työpaikan tai henkilökohtainen-tili. Azure AD näkyy suostumusta kehote hän käyttää sovelluksen ensimmäisen kerran.

**Jos sovellus edellyttää oikeuksia, jotka vain järjestelmänvalvojat voivat suostuu, tai jos sovellus edellyttää, että organisaation käyttöoikeudet**: olisi erota järjestelmänvalvojan hankinta-käyttäjä. **"Hanki sovellus-painike** ohjaa järjestelmänvalvojat voivat kirjautua sisään ja valitse pyydä heitä suostumuksen organisaation käyttäjien puolesta. Tämä on lisätty etuna piilottaa sovelluksen loppukäyttäjät suostumusta näyttöön.

## <a name="visual-guidance-for-app-acquisition"></a>Sovelluksen hankkiminen visuaaliset ohjeet

"Hanki sovellus-linkki Azure AD-käyttöoikeuksien myöntäminen käyttäjän on luotava uudelleenohjaus (Määritä)-sivulla Salli organisaation järjestelmänvalvoja ja Määritä organisaation tiedot, jotka nykyisessä Microsoft käyttää sovelluksen. [Azure Active Directory-integrointi sovelluksiin](active-directory-integrating-applications.md) on artikkelissa käsitellään käyttöönottamisesta käyttöoikeuksien pyytäminen.

Kun järjestelmänvalvojat suostuu sovelluksen, käyttäjät voivat valita Lisää käyttäjien Office 365: n sovelluksen käynnistintä versiota (käytettävissä ruutukuvakkeen ja [https://portal.office.com/myapps](https://portal.office.com/myapps)). Jos haluat ilmoittaa tätä ominaisuutta, voit käyttää esimerkiksi "Tämän sovelluksen lisääminen organisaation" ja näyttää seuraavanlaiselta painikkeen:

![Sovelluksen tiedostotyypit ja -skenaariot](./media/active-directory-branding-guidelines/add-to-my-org.png)

Suosittelemme kuitenkin, että kirjoitat sen sijaan, että käyttäisit painikkeet selittävä teksti. Esimerkki:
> *Jos käytät jo Office 365- tai muut business-palvelun Microsoftilta, voit myöntää vain organisaation tietojen käytön < your_app_name >. Tämä sallii käyttäjien pääsyn < your_app_name > niiden aiemmin työ-tilien kanssa.*


## <a name="visual-guidance-for-sign-in"></a>Visuaaliset ohjeet kirjautumista varten
Sovelluksen pitäisi näkyä merkki-painiketta, joka ohjaa käyttäjät, joka vastaa protokolla, voit integroida Azure AD-kirjautuminen päätepiste. Seuraavassa osassa on tietoja-painike olisi millaiseksi.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Niiden ja "Kirjaudu sisään Microsoft-
Se on Microsoft-logo ja "Sign in with Microsoft" ehdot, joka edustaa yksilöllisesti Azure AD muun muassa muiden tunnistetietojen toimittajat sovelluksen voivat tukea. Jos sinulla ei ole tarpeeksi tilaa "Sign in with Microsoft", on lyhentää "Kirjaudu" ok.

![Sovelluksen tiedostotyypit ja -skenaariot](./media/active-directory-branding-guidelines/sign-in-with-microsoft-light.png)

![Sovelluksen tiedostotyypit ja -skenaariot](./media/active-directory-branding-guidelines/sign-in-light.png)

Voit käyttää myös Tumma värimallin painikkeiden.

![Sovelluksen tiedostotyypit ja -skenaariot](./media/active-directory-branding-guidelines/sign-in-with-microsoft-dark.png)

![Sovelluksen tiedostotyypit ja -skenaariot](./media/active-directory-branding-guidelines/sign-in-dark.png)

## <a name="branding-dos-and-donts"></a>Tehtävät ja vältettävät asiat mukauttaminen

**Tee** käyttää "työpaikan tai oppilaitoksen tiliä" "Sign in with Microsoft"-painiketta yhdessä antamaan muita selitys avulla loppukäyttäjien tunnista niitä, voit käyttää sitä. **Älä** Käytä muita ehtoja, kuten "yrityksen tiliä", "Yritystili" tai "yritystili."

**Älä** Käytä "Office 365: n" tai "Azure tunnus". Office 365: n myös Microsoftin, joka ei käytä Azure AD-todennuksessa kuluttaja nimi.

**Älä** muuta Microsoft-logo.

**Älä** Näytä loppukäyttäjien Azure tai Active Directory-merkkisiä. Se on kunnossa kuitenkin käyttää näitä ehtoja kehittäjät, IT-ammattilaisille ja järjestelmänvalvojat.

## <a name="navigation-dos-and-donts"></a>Siirtymisruudun tehtävät ja vältettävät asiat

**Tee** avulla käyttäjät voivat kirjautua ulos ja vaihtaa toiseen käyttäjätiliin. Vaikka useimmat käyttäjät on henkilökohtainen tilistä Microsoft/Facebook/Google tai Twitter-, henkilöt ovat usein liittyvät useampaan kuin yhteen organisaatioon. Tuki useille kirjautunut sisään käyttäjille on tulossa.
