<properties
   pageTitle="Azure Active Directory-B2B yhteistyön | Microsoft Azure"
   description="Azure Active Directory-B2B yhteistyö mahdollistaa liikekumppanien yrityksen-sovelluksia, niiden käyttäjien vastaavan yhden Azure AD kanssa tili"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory-B2B yhteistyö

Azure Active Directory (Azure AD) B2B yhteiskäyttö voit käyttöönottaminen yrityksen sovellusten-kumppani hallitut jäsenyydet. Voit luoda yrityksen yhteyksiä kutsutaan ja sallimisesta kumppaniyritykset, voit käyttää resurssien käyttäjiltä. Monimutkaisuus pienenee, koska jokaisen yrityksen federates kerran Azure Active Directory-hakemistosta ja kunkin käyttäjän edustaa yhden Azure AD-tili. Suojauksen kasvaa, kun yhteyttä liikekumppanien hallita niiden tilien Azure AD, koska oikeudet poistetaan, kun kumppanin käyttäjien lopetetaan niiden organisaation ja tahattoman käytön kautta sisäisiä kansioita jäsenyys on estetty. Liiketoiminta-käytössä, joilla ei ole vielä Azure AD-B2B yhteistyö on virtaviivaistettu kirjautumisen kokemus antamaan business kumppaneille Azure AD-tilit.

-   Liiketoiminta-kumppanien käyttää omia kirjautumisen tunnistetiedot, jotka voit vapauttaa hallinta ulkoisen kumppanin-kansio ja käyttämistavat, kun käyttäjä jättää kumppaniorganisaation ei tarvitse.

-   Käyttöoikeuksien hallinta ulkopuolisista business kumppanin tilin elinkaari sovelluksia. Tämä tarkoittaa esimerkiksi, että voit kumota pääsyn eikä sinun tarvitse tehdä jotakin business kumppanin IT-osaston Kysy.

## <a name="capabilities"></a>Ominaisuudet

B2B yhteistyön yksinkertaistaa hallinta ja parantaa suojausta yrityksen resurssit, kuten SaaS sovellukset, kuten Office 365: ssä, Salesforce, Azure-palvelujen ja jokaisen mobile cloud ja paikallisen saatavat tukeva sovelluksen kumppanien käyttöoikeus. B2B yhteiskäyttö ottaa kumppaneiden hallita omia ja yritysten korostuksesta suojauskäytäntöjä kumppanien käyttöoikeus.

Azure Active Directory B2B yhteistyö on helppoa määrittää yksinkertaistettu kirjautuminen kaiken kokoisille kumppaneille, vaikka hänellä ei ole omia Azure Active Directory sähköpostin vahvistettu prosessin kautta. On myös helppo ylläpitää ei ulkoisen kansioita tai kohti kumppanin federation määrityksiä.

## <a name="b2b-collaboration-process"></a>B2B yhteistyö prosessi

1. Azure AD-B2B yhteistyön avulla yrityksen järjestelmänvalvoja, voit kutsua ja sallia ulkoisten käyttäjien joukon mukaan enintään 2 000 riviä pilkuilla erotetut arvot (CSV)-tiedoston lataaminen B2B yhteistyö-portaaliin.

  ![CSV-tiedosto lataaminen-valintaikkuna](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. Portaalin lähettää sähköpostikutsun Ulkoiset käyttäjät.

3. Kutsuttu käyttäjä joko aiemmin työpaikan kirjautuminen Microsoft (hallitaan Azure AD), tai pyydä uusi työpaikan Azure AD.

4. Kun olet kirjautunut sisään, käyttäjä ohjataan sovellusta, joka on jakanut heidän kanssaan.

Kutsujen kuluttaja sähköpostiosoitteet (esimerkiksi Gmail tai [*comcast.net*](http://comcast.net/)) eivät ole tuettuja.

Saat lisätietoja B2B yhteistyön toiminta Katso [Tämä video](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Seuraavat vaiheet
Selaa, tutustu artikkeliin Azure AD B2B yhteiskäytön.

- [Mikä on Azure AD B2B yhteistyö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Toiminta](active-directory-b2b-how-it-works.md)
- [Laskun](active-directory-b2b-detailed-walkthrough.md)
- [CSV-tiedoston muodon Ohje](active-directory-b2b-references-csv-file-format.md)
- [Ulkoisen käyttäjän suojaustunnuksen muoto](active-directory-b2b-references-external-user-token-format.md)
- [Ulkoisen käyttäjän objektin määritemuutokset](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Nykyinen esikatselu-rajoitukset](active-directory-b2b-current-preview-limitations.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
