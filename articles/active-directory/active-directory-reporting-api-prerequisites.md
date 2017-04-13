<properties
    pageTitle="Voit käyttää reporting API Azure AD edellytykset. | Microsoft Azure"
    description="Lisätietoja käyttämään raportoinnin API Azure AD edellytykset"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Voit käyttää reporting API Azure AD edellytykset 

[Azure AD raportoinnin ohjelmointirajapinnan](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) antaa sinulle ohjelmallisesti REST-pohjainen API suoran tietoihin. Voit soittaa nämä API useista eri kielillä ja työkalut.

Raportoinnin Ohjelmointirajapinnan käyttää [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) Hyväksy Internet-ohjelmointirajapinnan käyttö. 

Valmisteleminen raportointia Ohjelmointirajapinnan käyttöoikeus, toimi seuraavasti:

1. Azure AD-vuokraajan-sovelluksen luominen 

2. Anna sovelluksen tarvittavat käyttöoikeudet, Azure AD-tietojen käyttämistä varten

3. Kokoonpanoasetusten kerääminen hakemistossa

Kysymyksiä ongelmista ja palautetta, ota yhteyttä [AAD raportointi auttaa](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Azure AD-sovelluksen luominen

Voit määrittää hakemiston käyttämään raportoinnin API Azure AD-on kirjautumalla sisään Azure perinteinen portaalin Azure tilauksen järjestelmänvalvojan tilille, jolla on myös Azure AD-vuokraajan Yleinen järjestelmänvalvoja hakemisto-roolin jäsen.

> [AZURE.IMPORTANT] Tunnistetietojen alaisuudessa "järjestelmänvalvojan" oikeuksilla tältä sovellukset voivat olla hyvin tehokkaita, joten muista suojata sovelluksen tunnus/salaisuus tunnistetiedot.


1. Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com), valitse vasemmassa siirtymisruudussa.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Active directory** -luettelosta hakemistossa.

3. Valitse ylä-valikossa **sovellukset**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Alapalkissa Valitse **Lisää**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Valitse **Mitä haluat tehdä?** -valintaikkunassa Lisää **organisaation kehittää sovellus**. 

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/04.png) 


6. **Kerro sovelluksesi tietoja** -valintaikkunassa seuraavasti: 

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/05.png) 

    a. Kirjoita **nimi** -tekstiruutuun nimi (esimerkiksi: raportoinnin API-sovellus).

    b. Valitse **Web-sovelluksen ja/tai verkko-Ohjelmointirajapinnan**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.


7. Suorita **sovellus ominaisuudet** -valintaikkunassa seuraavasti: 

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/06.png) 

    a. Kirjoita **Sign URL** -tekstiruutuun `https://localhost`.

    b. Kirjoita **Sovelluksen tunnus URI** -tekstiruutuun ```https://localhost/ReportingApiApp```.

    c-näppäinyhdistelmää. Valitse **Valmis**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Myönnä käyttöoikeudet sovelluksen Ohjelmointirajapinnan käyttäminen

1. Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com/), valitse vasemmassa siirtymisruudussa.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Active directory** -luettelosta hakemistossa.

3. Valitse ylä-valikossa **sovellukset**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/02.png)


3. Valitse sovellukset-luettelosta juuri luomasi sovelluksen.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/07.png)

4. Valitse ylä-valikossa valitsemalla **Määritä**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/08.png)


5. Valitse **muiden sovellusten käyttöoikeudet** -kohdassa **Azure Active Directory** -resurssien **Sovelluksen käyttöoikeudet** avattavasta luettelosta ja valitse sitten **kansion tietojen**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/09.png)


5. Alapalkissa Valitse **Tallenna**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Kokoonpanoasetusten kerääminen hakemistossa

Tässä osassa näytetään, miten voit saada hakemistossa seuraavat asetukset:

- Toimialuenimi
- Asiakastunnus
- Asiakkaan salaisuus

Tarvitset nämä arvot puhelut raportoinnin ohjelmointirajapinnan määritettäessä. 


### <a name="get-your-domain-name"></a>Toimialuenimen hankkiminen

1. Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com), valitse vasemmassa siirtymisruudussa.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Active directory** -luettelosta hakemistossa.

3. Valitse ylä-valikossa Valitse **Toimialueet**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/11.png) 

4. Kopioi toimialuenimen **Toimialuenimi** -sarakkeessa.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Hae asiakkaan Sovellustunnus

1. Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com), valitse vasemmassa siirtymisruudussa.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Active directory** -luettelosta hakemistossa.

3. Valitse ylä-valikossa **sovellukset**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Valitse sovellukset-luettelosta juuri luomasi sovelluksen.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/07.png)

4. Valitse ylä-valikossa valitsemalla **Määritä**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/08.png)

4. Kopioi omaan **Asiakastunnus**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Hae sovelluksen asiakkaan salaisuus

Pääset sovelluksen asiakkaan salaisuus haluat luoda uuden tunnuksen ja tallentaa arvonsa yhteydessä tallentaminen uusi avain, koska se ei voi hakea myöhemmin enää tätä arvoa.

1. Valitse **Active Directory** [Azure perinteinen portal](https://manage.windowsazure.com), valitse vasemmassa siirtymisruudussa.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/01.png) 

2. **Active directory** -luettelosta hakemistossa.

3. Valitse ylä-valikossa **sovellukset**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Valitse sovellukset-luettelosta juuri luomasi sovelluksen.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/07.png)

4. Valitse ylä-valikossa valitsemalla **Määritä**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/08.png)

5. **Näppäimet** -osassa seuraavasti: 

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/14.png)

    a. Valitse kesto Kesto-luettelosta

    b. Alapalkissa Valitse **Tallenna**.

    ![Sovelluksen rekisteröinti](./media/active-directory-reporting-api-prerequisites/10.png)

    c-näppäinyhdistelmää. Kopioi avainarvon.

## <a name="next-steps"></a>Seuraavat vaiheet

- Haluatko käyttää tietoja Azure AD raportoinnin API ohjelmallisesti tavalla? Tutustu [Azure Active Directory raportoinnin Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md).

- Jos haluat lisätietoja Azure Active Directory raportointi on [Azure Active Directory raportoinnin Guide](active-directory-reporting-guide.md).  
