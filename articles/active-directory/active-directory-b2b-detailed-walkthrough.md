<properties
   pageTitle="Version Azure Active Directory-B2B yhteistyön preview'lla | Microsoft Azure"
   description="Azure Active Directory-B2B yhteistyö tukee yrityksen yhteydet ottamalla liikekumppanien käyttämään yrityksen sovellustesi valikoivasti"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Azure AD-B2B yhteistyön esikatselu: laskun

Tätä vaiheittaista jäsentää Azure AD B2B yhteistyön käyttämisestä. Contoson IT-järjestelmänvalvoja haluamme sovellusten jakaminen kolme kumppanin yritysten työntekijöiden kanssa. Ei mitään kumppaniyritykset tarvitse Azure AD.

- Yksinkertainen kumppanin Org-Anneli
- Teemu Normaali kumppanin Org-sen Määritä sovellusten käyttöoikeudet
- Carolin monimutkaisia kumppanin Org-tarvitseville sovellukset ja ryhmien Contoson jäsenyys

Sen jälkeen, kun kutsut lähetetään partner käyttäjille, että voit määrittää myöntää sovelluksia ja palvelun Azure-portaalissa ryhmien jäsenyyden Azure AD. Aloitetaan lisäämällä Anneli.

## <a name="adding-alice-to-the-contoso-directory"></a>Anneli lisääminen Contoso-kansio
1. Luoda .csv-tiedoston kanssa otsikot, täyttää vain Anneli 's **sähköpostin** **Näyttönimi**ja **InviteContactUsUrl**. **Näyttönimi** on nimi, joka näkyy kutsun ja nimi, joka näkyy Contoso Azure AD-kansiossa. **InviteContactUsUrl** on tapa Anneli yhteystiedot Contoso. Seuraavassa esimerkissä InviteContactUsUrl määrittää Contoso LinkedIn-profiiliin. On tärkeää oikeinkirjoituksen täsmälleen samalla tavalla kuin [CSV-tiedoston muoto viittaus](active-directory-b2b-references-csv-file-format.md)määritettyyn .csv-tiedoston ensimmäisen rivin otsikot.  
![Anneli Esimerkki CSV-tiedostosta](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Azure-portaalissa Lisää käyttäjä Contoso-kansio (Active Directory > Contoso > käyttäjät > Lisää käyttäjä). Valitse "käyttäjä tyyppi-avattavaa valikkoa,"Käyttäjät kumppaniyritykset". .Csv-tiedoston lataaminen Varmista, että .csv-tiedosto suljetaan ennen lataamista.  
![CSV-tiedoston lataaminen Anneli varten](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Anneli esitetään nyt ulkoisena käyttäjänä Contoso Azure AD-kansiossa.  
![Anneli näkyy Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Anneli vastaanottaa seuraavat sähköpostin.  
![Kutsu sähköpostin Anneli](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Anneli napsauttaa linkkiä, ja hän pyydetään Hyväksy kutsu ja kirjautua sisään hänen työ-tunnistetiedoilla. Jos Anneli ei ole Azure Active directory-Anneli pyydetään rekisteröintiä.  
![Kun kutsu Anneli rekisteröityminen](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Anneli ohjataan-sovelluksen Access-paneelin tyhjä, ennen kuin hän on myönnetty sovelluksia.  
![Access-paneelin Anneli varten](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Tämän toiminnon avulla B2B yhteistyön helpoin lomake. Contoso Azure Active directory-käyttäjäksi Anneli voit myöntää sovellukset ja ryhmien palvelun Azure-portaalissa käyttöoikeudet. Nyt lisääminen Teemu, kuka on Moodle ja Salesforce-sovellukset.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Teemu lisääminen Contoso-kansio ja sovellusten käyttöoikeuksien myöntäminen
1. Windows PowerShellin Azure AD-moduulin ja Etsi sovellus Moodle tunnukset ja Salesforce asennettuna. Tunnukset voi hakea cmdlet-komennolla: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` Tämä näyttää kaikki käytettävissä olevat sovellukset Contoso ja niiden AppPrincialIds luettelo.  
![Teemu tunnukset noutaminen](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Luo .csv-tiedosto, joka sisältää Teemun sähköpostin ja näyttönimi, **InviteAppID**, **InviteAppResources**ja InviteContactUsUrl. Täytä **InviteAppResources** AppPrincipalIds Moodle kanssa ja Salesforce löytyy PowerShell-erottaa ne toisistaan välilyönnillä. Täytä **InviteAppId** sama AppPrincipalId, Moodle brand sähköposti ja kirjaudu sivujen Moodle logolla kanssa.  
![Teemu Esimerkki CSV-tiedostosta](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Lataa palvelun Azure-portaalissa .csv-tiedoston samalla tavalla kuin se oli valmis Anneli. Teemu on nyt ulkoisen käyttäjän Contoso Azure AD-kansiossa.

4. Teemu vastaanottaa seuraavat sähköpostin.  
![Teemu sähköpostin kutsu](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Teemu olevaa linkkiä ja pyydetään Hyväksy kutsu. Kun hän on kirjautunut, hän ohjataan Access-ruutu ja jo käyttää Moodle ja Salesforce.  
![Access-paneelin Teemu varten](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Lisäämme Carolin seuraavaksi access-sovellukset ja Contoso-hakemisto-ryhmien jäsenyyden tarvitsevalle.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Carolin lisääminen Contoso-kansio ja sovellusten käyttöoikeuksien myöntäminen antamalla Ryhmäjäsenyys

1. Windows PowerShellin Azure AD-moduulin kanssa asennettuna sovellusten tunnukset ja ryhmätunnus Contoso kuluessa.
 - Noutaa cmdlet-komennolla AppPrincipalId `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, sama kuin Teemu
 - Noutaa objektitunnus ryhmien cmdlet-komennolla `Get-MsolGroup | fl DisplayName, ObjectId`. Tämä näyttää kaikkien ryhmien Contoso ja niiden ObjectIds luettelo. Ryhmätunnus voi hakea myös nimellä Objektitunnus Azure-portaalissa ryhmän ominaisuudet-välilehti.  
![Carolin tunnukset ja ryhmien noutaminen](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Luo .csv-tiedoston, täyttää Carolin sähköpostin, näyttönimi, InviteAppID, InviteAppResources, **InviteGroupResources**ja InviteContactUsUrl. **InviteGroupResources** täytetään ObjectIds MyGroup1 ja ulkoisia, erottaa ne toisistaan välilyönnillä ryhmien mukaan.  
![Carolin Esimerkki CSV-tiedostosta](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Palvelun Azure-portaalissa .csv-tiedoston lataaminen

4. Carolin on Contoso-directory-käyttäjän ja myös MyGroup1 ja ulkoisia-ryhmän jäsen tarkastelu Azure-portaalissa.  
![Carolin näkyy Azure AD-ryhmä](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carolin saa ilmoituksen sähköpostitse, joka sisältää linkin Hyväksy kutsu. Kun käyttäjä kirjautuu sisään, hän ohjataan Moodle ja Salesforce käyttää sovelluksen Access-paneeli.  

Tämä on kaikkien käyttäjien lisääminen kumppanin yritysten Azure AD B2B yhteistyössä on. Tätä vaiheittaista ilmeni Anneli, Teemu ja Carolin käyttäjien lisäämisestä Contoso-hakemistoon käyttämällä kolme erillisissä .csv-tiedostoissa. Tämä prosessi voi tehdä helpommin kondenssikattilat erillisissä .csv-tiedostoissa yhteen tiedostoon.  
![Esimerkki CSV-tiedoston Anneli, Teemu ja Carolin](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
Siirry Azure AD B2B yhteistyö sekä muita artikkeleita:

- [Mikä on Azure AD B2B yhteistyö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Toiminta](active-directory-b2b-how-it-works.md)
- [CSV-tiedoston muodon Ohje](active-directory-b2b-references-csv-file-format.md)
- [Ulkoisen käyttäjän suojaustunnuksen muoto](active-directory-b2b-references-external-user-token-format.md)
- [Ulkoisen käyttäjän objektin määritemuutokset](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Nykyinen esikatselu-rajoitukset](active-directory-b2b-current-preview-limitations.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
