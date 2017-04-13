<properties
   pageTitle="Azure Active Directory-raportointi: Käytön aloittaminen | Microsoft Azure"
   description="Näyttää luettelon eri raporttien Azure Active Directory-raporteissa"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>Azure Active Directory-raportointi käytön aloittaminen

## <a name="what-it-is"></a>Mikä se on

Azure Active Directory (Azure AD) sisältää suojaus, tehtävän ja valvonta raporttien hakemistossa. Ohessa on luettelo sisällyttää raportteja:

### <a name="security-reports"></a>Suojaus-raportit

- Kirjaudu apuohjelmien tuntemattomista lähteistä
- Kirjaudu apuohjelmien useita virheitä jälkeen
- Kirjaudu apuohjelmien useita paikkojen kohteesta
- Kirjaudu apuohjelmien IP-osoitteista epäilyttävistä aktiviteettiin
- Epäsäännöllinen kirjautumisen tehtävä
- Kirjaudu apuohjelmien mahdollisesti tartunnan laitteilla
- Käyttäjät, joilla on erheellisiin kirjautumisen tehtävä

### <a name="activity-reports"></a>Käyttöraportit

- Sovellusten käyttö: Yhteenveto
- Sovellusten käyttö: yksityiskohtaiset
- Sovelluksen koontinäyttö
- Tilin valmisteluvirheitä
- Yksittäisen Käyttäjälaitteet
- Yksittäisen käyttäjän tehtävä
- Ryhmien toimintojen raportti
- Salasanan palauttaminen rekisteröinti toimintojen raportti
- Salasanan tehtävä

### <a name="audit-reports"></a>Valvonta-raportit

- Hakemiston valvontaraportin

> [AZURE.TIP] Lisää ohjeita Azure AD-raportointi Tutustu [kopioimasta raportteja](active-directory-view-access-usage-reports.md).



## <a name="how-it-works"></a>Toiminta


### <a name="reporting-pipeline"></a>Myyntijakso raportointi

Raportoinnin myyntijakso koostuu kolme päävaihetta. Aina, kun käyttäjä kirjautuu tai todennus on tehty, tapahtuu seuraavasti:

- Ensin käyttäjä todennetaan (onnistuneesti tai mutta epäonnistuu), ja tulos on tallennettu Azure Active Directory-palvelun tietokannat.
- Kaikki viimeisimmät Kirjaudu ins käsitellään säännöllisin väliajoin. Tässä vaiheessa Microsoftin security ja erheellisiin tehtävän algoritmit etsitään kaikki viimeisimmät Kirjaudu ins epäilyttävistä tehtävälle.
- Käsittelyn jälkeen raportit on kirjoitettu, välimuistin ja served Azure perinteinen-portaalissa.

### <a name="report-generation-times"></a>Raportin luonti ajat

Erittäin paljon välityspalvelimia vuoksi ja kirjaudu ins käsitteleminen Azure AD-ympäristössä, merkki-laajennukset viimeisimmän käsitteleminen on, keskimäärin noin tunnin vanha. Vain harvoin voi kestää jopa kahdeksan tuntia käsittelemään viimeisimmän merkki-laajennukset.

Löydät viimeisimmän käsitellyt-kirjautuminen tarkastelemalla ohjeteksti jokaisen raportin yläreunassa.

![Ohje-teksti kunkin raportin yläreunassa](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Lisää ohjeita Azure AD-raportointi Tutustu [kopioimasta raportteja](active-directory-view-access-usage-reports.md).



## <a name="getting-started"></a>Käytön aloittaminen


### <a name="sign-into-the-azure-classic-portal"></a>Kirjautua Azure perinteinen-portaaliin

Sinun on ensin kirjautumalla [Azure perinteinen portal](https://manage.windowsazure.com) yleisenä tai yhteensopivuuden järjestelmänvalvoja. On myös oltava Azure tilauksen palvelun järjestelmänvalvoja tai muiden järjestelmänvalvojan tai käyttää "Pääsy Azure AD" Azure tilauksen.

### <a name="navigate-to-reports"></a>Siirry raportteihin

Voit tarkastella raportteja Siirry hakemistossa yläreunassa raportit-välilehti.

Jos katselet raporttien ensimmäistä kertaa, tarvitset Hyväksy-valintaikkuna, ennen kuin voit tarkastella raportteja. Näin voit varmistaa, että se on hyväksyttävä järjestelmänvalvojille organisaation tarkastella näitä tietoja, joita voidaan pitää henkilötietojen joissakin maissa.

![Valintaikkuna](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Raporttien tarkasteleminen

Siirry kunkin raportin tiedot kerätään ja käsitellä merkki-laajennukset. Voit tarkastella [kaikkien raporttien luettelo tähän](active-directory-reporting-guide.md).

![Kaikkien raporttien](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Lataa raportit CSV-muodossa

Jokaisen raportin voi ladata CSV (CSV-arvo)-tiedostona. Voit käyttää Excelissä, PowerBI tai kolmannen osapuolen analyysi ohjelmat voivat edelleen tietojen analysointi nämä tiedostot.

Voit ladata kaikki valmiiksi CSV-raportin Siirry ja valitse alareunassa "Lataa".

![Lataa-painike](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Lisää ohjeita Azure AD-raportointi Tutustu [kopioimasta raportteja](active-directory-view-access-usage-reports.md).





## <a name="next-steps"></a>Seuraavat vaiheet

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Mukauta ilmoitukset erheellisiin kirjauduttaessa tehtävän

Siirry hakemistoon "Määrittäminen"-välilehti.

Siirry "ilmoitukset-osaan.

Ota käyttöön tai poistaa käytöstä "Sähköpostin erheellisiin Kirjaudu apuohjelmien ilmoitukset"-osassa.

![Ilmoitukset-osiossa](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Raportoinnin API Azure AD integrointi

Katso [raportointi-Ohjelmointirajapinnan käytön aloittaminen](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Osallistuminen Monimenetelmäisen todentamisen käyttäjille

Valitse käyttäjän raportin.

Napsauta näytön alareunassa "Käyttöön MFA"-painiketta.

![Näytön alalaidan multi-factor Authentication-painike](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Lisää ohjeita Azure AD-raportointi Tutustu [kopioimasta raportteja](active-directory-view-access-usage-reports.md).




## <a name="learn-more"></a>Opi lisää


### <a name="audit-events"></a>Valvottavat tapahtumat

Tietoja siitä, mitä seurattujen [Azure Active Directory raportoinnin valvontalokin tapahtumien](active-directory-reporting-audit-events.md)hakemistossa.

### <a name="api-integration"></a>API-integrointi

On [raportointi-Ohjelmointirajapinnan aloittaminen](active-directory-reporting-api-getting-started.md) ja [API oppaat](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Yhteyden

Sähköpostin [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) palautetta, Ohje tai kysyttävää, voit joutua.

> [AZURE.TIP] Lisää ohjeita Azure AD-raportointi Tutustu [kopioimasta raportteja](active-directory-view-access-usage-reports.md).
