<properties
   pageTitle="Azure Active Directory-B2B yhteistyön esikatselu CSV-tiedostomuoto | Microsoft Azure"
   description="Azure Active Directory-B2B tukee yrityksen yhteydet ottamalla liikekumppanien käyttämään yrityksen sovellustesi valikoivasti"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Azure AD-B2B yhteistyön esikatselu: CSV-tiedostomuoto

Azure AD B2B yhteistyö preview-versiosta edellyttää käyttäjän kumppanitiedot ladattava palvelun Azure AD-portaalissa, joka määrittää CSV-tiedostoon. CSV-tiedostossa on oltava tarvittavat otsikoiden alla ja valinnaisia kenttiä. Muokkaa muuttamatta ensimmäisen rivin otsikot oikeinkirjoituksen CSV‑mallitiedostoa (alla).

>[AZURE.NOTE] CSV-tiedoston voi jäsentää onnistuneesti tarvitaan ensimmäisen rivin otsikot (kuten sähköpostin, näyttönimi ja niin edelleen). Oikeinkirjoituksen on vastattava alla määritetyt kentät. Nämä otsikot tunnistaa alle sisältöä. Valinnaisia kenttiä, jotka eivät ole tarpeen niiden otsikot voi poistaa CSV-tiedostosta. Koko sarake voidaan jättää tyhjäksi.

## <a name="required-fields-br"></a>Pakolliset kentät: <br/>
**Sähköpostin:** Kutsuttujen käyttäjän sähköpostiosoite. <br/>
**Näyttönimi:** Kutsuttu käyttäjä (yleensä ensimmäinen ja viimeinen nimi) näyttönimi.<br/>


## <a name="optional-fields-br"></a>Valinnaiset kentät: <br/>

**InvitationText:** Kutsu sähköpostin tekstin mukauttaminen jälkeen sovelluksen mukauttaminen ja ennen lunastushinta-linkkiä.<br/>
**InvitedToApplications:** Voit määrittää käyttäjät yrityksen sovellusten AppIDs. AppIDs ovat noudatettavan powershellissä kutsumalla`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** ObjectIDs ryhmien lisäämään käyttäjälle. ObjectIDs ovat noudatettavan powershellissä kutsumalla`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** URL-osoite ohjaamaan kutsuttu käyttäjä kutsun hyväksymisen jälkeen. Tämä on oltava yrityksen URL-osoite (esimerkiksi [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Jos valinnaiset kentässä ei ole määritetty, kutsuttujen käyttäjä ohjataan kohtaa, johon ne voivat siirtyä valitun yrityksen sovellukset App Access-paneeli. Sovelluksen Accessin Ohjauspaneelin URL-osoite on muotoa `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: kopioi lähetetty kutsu sähköpostiosoite. Jos CcEmailAddress-kenttää käytetään-kutsuun ei voi käyttää sähköpostin vahvistettu käyttäjän tai Palveltavien kohteiden luominen.<br/>
**Kieli:** Kutsu sähköpostin ja lunastushinta mobiilikäyttöä "En" (englanniksi) oletusarvoksi, kun sitä ei ole kieli. Muut 10 tuetut koodeilla kieli:<br/>
1. de: Saksa<br/>
2. ES: Espanja<br/>
3. f: ranska<br/>
4. se: Italia<br/>
5. ja: japani<br/>
6. KO: korea<br/>
7. pt BR: Portugali (Brasilia)<br/>
8. RU: Venäjä<br/>
9. ZH HANS: yksinkertaistettu kiina<br/>
10. ZH HANT: perinteinen kiina<br/>

## <a name="sample-csv-file"></a>CSV-esimerkkitiedosto
Tässä on esimerkki CSV muokkaamista.

>[AZURE.NOTE] Kopioi ja Liitä tämä Muistioon ja tallenna se siihen '.csv-tiedoston avaaminen. Valitse tämä Muokkaa Excelissä. Sitä järjestetty otsikoita, niistä ensimmäisen rivin taulukkoon.

> Lisää valinnaisia kenttiä Excelin määrittämällä otsikko ja täyttää sarakkeen sen alle.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
Hae Microsoftin Azure AD B2B yhteiskäytön artikkeleita

- [Mikä on Azure AD B2B yhteistyön?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Toiminta](active-directory-b2b-how-it-works.md)
- [Laskun](active-directory-b2b-detailed-walkthrough.md)
- [Ulkoisen käyttäjän suojaustunnuksen muoto](active-directory-b2b-references-external-user-token-format.md)
- [Ulkoisen käyttäjän objektin määritemuutokset](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Nykyinen esikatselu-rajoitukset](active-directory-b2b-current-preview-limitations.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
