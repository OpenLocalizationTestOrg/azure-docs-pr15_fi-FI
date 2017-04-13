<properties
    pageTitle="Azure AD Connect kunto-toimintoja."
    description="Tässä artikkelissa kerrotaan Lisää toiminnoista, joita voi käyttää, kun olet ottanut Azure AD yhteyden kunto."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Azure AD Connect kunto-toiminnot

Seuraavassa aiheessa kerrotaan eri toiminnoista, joita voidaan suorittaa Azure AD yhteyden kunto.

## <a name="enable-email-notifications"></a>Sähköposti-ilmoitusten ottaminen käyttöön
Voit määrittää Azure AD yhteyden kunto palvelun lähettämään sähköposti-ilmoitukset, kun Hälytykset luodaan ilmaisee, että käyttäjätietojen infrastruktuurin ei ole kunnossa. Tilanne ilmenee, kun ilmoituksen luodaan sekä kun se on merkitty ratkennut. Noudattamalla seuraavia ohjeita voit määrittää sähköposti-ilmoitukset.

![Azure AD Connect kunto sähköpostin ilmoituksen Tutustu](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] Sähköposti-ilmoitukset ovat oletusarvoisesti poissa käytöstä.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Jos haluat ottaa käyttöön Azure AD yhteyden kunto sähköposti-ilmoitukset

1. Avaa ilmoitukset-sivu, johon haluat vastaanottaa sähköposti-ilmoituksen palvelulle.
2. Napsauta toimintorivin "Ilmoitusasetusten määrittäminen"-painiketta.
3. Poista sähköposti-ilmoituksen valitsin kohtaan käytössä.
4. Valitse Yleiset järjestelmänvalvojat vastaanottaa sähköposti-ilmoitusten määrittäminen-valintaruutu.
5. Jos haluat saada sähköposti-ilmoitukset muut sähköpostiosoitteet, voit määrittää ne muita sähköpostin vastaanottaja-ruutuun. Voit poistaa tämän luettelon sähköpostiosoite, napsauta sitä hiiren kakkospainikkeella ja valitsemalla Poista.
6. Voit viimeistellä muutoksia napsauttamalla "Tallenna". Kaikki muutokset tulevat voimaan tehosteet, vain, kun olet valinnut "Tallenna".

## <a name="delete-a-server-or-service-instance"></a>Palvelimeen tai palveluun poisto

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Palvelimen poistaminen Azure AD yhteyden kunto palvelu
Joissakin tilanteissa haluat ehkä seurataan palvelimen poistaminen. Noudattamalla seuraavia ohjeita palvelimen poistaminen Azure AD yhteyden kunto palvelu.

Palvelimen poistettaessa huomioon seuraavasti:

- Tämä toiminto lopettaa kaikki edelleen tietojen keräämisen tähän palvelimeen. Tämä palvelin enää seuranta-palvelusta. Kun tämä toiminto, et voi olla voivat tarkastella uusia ilmoitukset-seurantaa tai käyttö tälle palvelimelle analytics-tiedot.
- Tämä toiminto ei poista tai kunto-agentti poistaminen palvelimellesi. Jos olet poistanut kunto-agentti ei ennen tämän vaiheen suorittamiseen, näkyviin voi tulla virhe tapahtumien liittyvät kunto-agentti palvelimessa.
- Tämä toiminto ei poista jo keränneet palvelimen tiedot. Tiedot poistetaan Microsoft Azure-tietoihin säilytyskäytäntö mukaan.
- Jos haluat aloittaa seuranta samaan palvelimeen uudelleen, sinun on jälkeen tätä toimintoa varten ja asenna uudelleen kunto-agentti tähän palvelimeen.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Palvelimen poistaminen Azure AD yhteyden kunto palvelu

Azure AD Connect kunto AD FS ja Azure AD-yhteyden (synkronointi):

1. Avaa Server-sivu Server luettelo-sivu valitsemalla poistettava palvelimen nimi.
2. Valitse palvelin-sivu toimintorivin "Poista"-painiketta.
3. Vahvista poistaminen palvelimen kirjoittamalla vahvistus-ruutuun palvelimen nimi.
4. Valitse "Poista"-painiketta.

Azure AD Connect kunto, AD DS:

1. Toimialueen ohjaimet Raporttinäkymät-ikkunan avaaminen
2. Valitse toimialueen ohjauskoneen poistetaan.
3. Napsauta toimintorivin "Poista valitut"-painiketta.
4. Vahvista poistaminen palvelimeen.
5. Valitse "Poista"-painiketta.

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Poista palvelun esiintymän Azure AD yhteyden kunto-palvelusta

Joissakin tilanteissa haluat ehkä poistaa palvelun esiintymän. Noudattamalla seuraavia ohjeita voit poistaa palvelun esiintymän Azure AD yhteyden kunto palvelun.

Palvelun esiintymän poistettaessa huomioon seuraavasti:

- Tämä toiminto poistaa palvelun nykyisen esiintymän seuranta-palvelusta.
- Tämä toiminto ei poistaminen tai asennuksen poistaminen kunto-agentti mistä tahansa palvelimet, joilla on seurattava tämän palvelun esiintymän osana. Jos olet poistanut kunto-agentti ei ennen tämän vaiheen suorittamiseen, virhe tapahtumien saattaa lakata liittyvät kunto-agentti palvelimesta.
- Tämän palvelun esiintymän kaikki tiedot poistetaan Microsoft Azure tietojen Arkistointikäytäntöjen mukaan.
- Jälkeen tätä toimintoa, jos haluat käynnistää palvelun seuranta, poista ja asennettava uudelleen kaikki palvelimet, joilla seurattava kunto-agentti. Jos haluat aloittaa seuranta samaan palvelimeen uudelleen, sinun on jälkeen tätä toimintoa varten ja asenna uudelleen kunto-agentti tähän palvelimeen.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Voit poistaa palvelun esiintymän Azure AD yhteyden kunto palvelu

1. Avaa palvelu sivu palvelun luettelo-sivu valitsemalla palvelun tunnus (klusterin nimi), jonka haluat poistaa.
2. Valitse palvelin-sivu toimintorivin "Poista"-painiketta.
3. Vahvista palvelunimi kirjoittamalla se vahvistusruudussa. (esimerkiksi: sts.contoso.com)
4. Valitse "Poista"-painiketta.
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Hallitse roolista käytön käyttää hallinta
### <a name="overview"></a>Yleiskatsaus
Azure AD yhteyden kunto- [Roolin perusteella käyttöoikeuksien valvonta](role-based-access-control-configure.md) tarjoaa Azure AD yhteyden kunto-palvelun käyttäjien tai ryhmien ulkopuolella Yleiset järjestelmänvalvojat. Tämä on saavuttaa roolien määrittäminen asianmukaiset käyttäjät ja ryhmät ja, jolla voit rajoittaa sisällä hakemistossa Yleiset järjestelmänvalvojat.

#### <a name="roles"></a>Roolit
Azure AD yhteyden kunto tukee seuraavia valmiita rooleja.

| Rooli | Käyttöoikeudet |
| ----------- | ---------- |
| Omistaja | Omistajat voivat ***hallita niiden käyttöä*** (kuten Määritä rooli käyttäjän tai ryhmän), ***näet kaikki tiedot*** (kuten Näytä ilmoitukset)-portaalin ja ***asetusten muuttaminen*** (kuten sähköposti-ilmoitukset) sisällä Azure AD yhteyden kunto. <br>Oletusarvon mukaan Azure AD Yleiset Järjestelmänvalvojat on määritetty tämä rooli ja tämä ei voi muuttaa.  |
|Avustaja|  Osallistujat voivat ***tarkastella kaikkia tietoja*** (kuten Näytä ilmoitukset)-portaalin ja ***muuttaa asetuksia*** (kuten sähköposti-ilmoitukset) sisällä Azure AD yhteyden kunto.|
|Lukija| Lukijat voivat ***tarkastella kaikkia tietoja*** (kuten näkymän ilmoitusten) sisällä Azure AD yhteyden kunto-portaalista.|

Kaikki muut roolit (esimerkiksi 'Käyttäjän Access järjestelmänvalvojat' tai "DevTest harjoituksia käyttäjät") vaikka portaalin kokemus käytettävissä ei ole vaikutusta pääset Azure AD yhteyden kunto.

#### <a name="access-scope"></a>Access-aluetta

Azure AD Connect tukee kahdella tasolla käyttöoikeuksien hallinta:

- ***Kaikki palvelun esiintymät***: Tämä on Useimmat asiakkaat suositellut polku ja ohjaa kaikkien palvelun esiintymien (kuten ADFS-klusterin) kaikki roolin välillä, jotka seurataan Azure AD yhteyden kunto mukaan.

- ***Palveluesiintymä***: Joskus saatat joutua eroteltava käyttöoikeuden roolin tyypit tai palvelun esiintymän. Tässä tapauksessa voidaan hallita palvelun esiintymän tasolla.  

Käyttöoikeus myönnetään, jos käyttäjä on käyttää joko hakemiston tai palvelun esiintymän tasolla.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Miten käyttäjien tai ryhmien käyttää Azure AD yhteyden kunto
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Vaiheet 1: Valitse asianmukaiset käyttöoikeudet-alue
Salli käyttöoikeus, *kaikki palveluesiintymiä* kuluessa Azure AD yhteyden kunto tasolla, Avaa tärkeimmät sivu Azure AD yhteyden kunto.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Vaihe 2: Lisää käyttäjiä, ryhmiä ja määrittää roolit
1. Napsauta määrittäminen-osassa "Käyttäjät"-osaa.<br>
![Azure AD Connect kunto RBAC pää-sivu](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Valitse "Lisää"
3. Valitse esimerkiksi "Omistaja" "rooli"<br>
![Azure AD Connect kunto RBAC käyttäjän lisääminen](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Kirjoita nimi tai tunnus kohdistettu käyttäjää tai ryhmää. Voit valita yhden tai useamman käyttäjien tai ryhmien samanaikaisesti. Valitse "Valitse".
![Azure AD Connect kunto RBAC Valitse käyttäjä](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Valitse, "Ok".<br>

6. Kun roolimääritys on valmis, käyttäjät tai ryhmät näkyvät luettelossa.<br>
![Azure AD Connect kunto RBAC käyttäjäluettelon](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Nämä vaiheet sallii luettelon käyttäjät ja ryhmän Accessin varatut roolien mukaan.
>[AZURE.NOTE]
- Yleiset järjestelmänvalvojat aina on täydet oikeudet kaikki toiminnot, mutta yleinen järjestelmänvalvojatilit eivät ole edellä olevassa luettelossa.
- "Käyttäjien kutsuminen" toiminto ei tue Azure AD yhteyden kunto.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Vaihe 3: Jakaa sivu sijainti käyttäjien tai ryhmien kanssa
1. Käyttäjä voi käyttää jälkeen oikeuksien määrittäminen Azure AD yhteyden kunto siirtymällä [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Kerran-sivu, käyttäjän voit kiinnittää eri osien koontinäyttö tai sivu napsauttamalla "Raporttinäkymät-ikkunan kiinnittäminen"<br>
![Azure AD yhteyden kunto RBAC PIN-tunnus-sivu](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Käyttäjä, jolla "Reader" rooli ei voi suorittaa "luominen-toiminto Azure AD yhteyden kunto tunniste saaminen Azure Marketplacesta. Tämän käyttäjän pääsevät yhä sivu valitsemalla edellä olevaa linkkiä. Seuraavien käytön käyttäjän voit kiinnittää koontinäyttö sivu.

### <a name="remove-users-andor-groups"></a>Käyttäjien tai ryhmien poistaminen
Voit poistaa käyttäjän tai ryhmän Azure AD yhteyden kunto rooli perusteella käytönvalvonta-osaan lisätty napsauttaminen hiiren kakkospainikkeella ja valitsemalla Poista.<br>
![Azure AD Connect kunto RBAC Poista käyttäjä](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Aiheeseen liittyvät linkit

* [Azure AD Connect kunto](active-directory-aadconnect-health.md)
* [Azure AD Connect kunto agentti asennus](active-directory-aadconnect-health-agent-install.md)
* [Käyttämällä Azure AD yhteyden kunto AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD yhteyden kunto käytön synkronointi](active-directory-aadconnect-health-sync.md)
* [Kuntotietojen käyttämällä Azure AD yhteydessä AD DS: ÄÄN](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect kunto usein kysytyt kysymykset](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect kunto versiohistoria](active-directory-aadconnect-health-version-history.md)
