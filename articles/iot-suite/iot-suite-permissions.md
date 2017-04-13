<properties
  pageTitle="Azure IoT ohjelmistopaketissa ja Azure Active Directory | Microsoft Azure"
  description="Tässä artikkelissa kuvataan, miten Azure IoT Suite käyttää Azure Active Directory käyttöoikeuksien hallinta."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Azureiotsuite.com oikeudet

## <a name="what-happens-when-you-sign-in"></a>Mitä tapahtuu, kun kirjaudut sisään

Kun kirjaudut osoitteessa [azureiotsuite.com][lnk-azureiotsuite], sivuston määrittää perustuu valittuna Azure Active Directory (AAD) vuokraajan ja Azure tilauksen käyttöoikeustasot.

1.  Sivuston ensin löytää Azure mitä AAD alihallinnat, jotka haluat täyttää alihallinnat kirjautuneena-käyttäjänimesi vieressä näkyy luettelo kuulut. Tällä hetkellä sivuston vain hankkia yksi vuokraajan tunnusten käyttäjä kerrallaan. Tuloksena vaihdettaessa oikean yläkulman avattavan luettelon käyttäminen eri vuokraajalle sivusto uudelleen kirjautuu you kyseisen alihallinnan, voit hankkia tunnukset alihallintaan.

2.  Seuraavaksi sivuston löytää Azure päättyneet tilaukset liittämäsi valitun vuokraajan. Näet käytettävissä tilaukset, kun luot uuden valmiiksi määritettyjä ratkaisun.

3.  Lopuksi sivuston hakee kaikki resurssit tilaukset ja resurssiryhmistä, merkityt esimääritetyt ratkaisut kuin ja lisää aloitussivulla ruudut.

Seuraavissa kohdissa kuvataan roolit, joilla on hallita esimääritettyjä ratkaisuihin.

## <a name="aad-roles"></a>AAD roolit

AAD roolit hallita voi säätää valmiiksi ratkaisuja ja esimääritettyjä ratkaista käyttäjien hallinta.

Löydät järjestelmänvalvojan rooleja haluat lisätietoja [artikkelissa järjestelmänvalvojan]roolien määrittäminen Azure AD-AAD[lnk-aad-admin], mutta tässä artikkelissa kerrotaan ensisijaisesti **Yleinen järjestelmänvalvoja** ja **Toimialueen käyttäjien ja käyttöoikeuksien** määrittäminen jäsenroolin sillä käyttävät esimääritetyt ratkaisut.

**Yleinen järjestelmänvalvoja:** Voi olla useita Yleiset järjestelmänvalvojat AAD kohden. Kun luot AAD vuokraajan, olet oletusarvoisesti kyseisen alihallinnan Yleinen järjestelmänvalvoja. Yleinen järjestelmänvalvoja voi valmistella esimääritettyjä ratkaisu ja määritetään sovelluksen sisältyy AAD käyttäjän vuokraajan **järjestelmänvalvoja** -roolista. Kuitenkin jos toisen käyttäjän AAD samassa alihallinnassa Luo sovellus, Yleinen järjestelmänvalvoja on myöntänyt oletusarvo-roolin on **Vain IMPLISIITTINEN lukea**. Yleiset järjestelmänvalvojat voivat määrittää roolit [Azure perinteinen portal]-sovellusten[lnk-classic-portal].

**Käyttäjä/toimialueeseen:** Voi olla useita toimialueen käyttäjät/jäseniä AAD kohden. Toimialuekäyttäjä voi valmistella esimääritettyjä ratkaisu – [azureiotsuite.com] [ lnk-azureiotsuite] sivuston. **Järjestelmänvalvoja**on ne myönnetään ne valmistella sovelluksen oletusarvon rooli. He voivat luoda [azure-iot-remote-seuranta] build.cmd komentosarja-sovellus[ lnk-rm-github-repo] tai [azure iot – ennakoivan-ylläpito -] [ lnk-pm-github-repo] säilöön, mutta ne myönnetään oletusarvon rooli on **IMPLISIITTINEN vain luku**-, ole roolin oikeudet. Jos toisen käyttäjän AAD vuokraajan Luo sovellus, ne on varattu **IMPLISIITTINEN vain luku** -roolin sovelluksen oletusarvoisesti. Heillä ei ole mahdollisuus määrittää roolit sovellusten; Tämän vuoksi voi lisätä käyttäjät tai roolit sovelluksen käyttäjille, vaikka ne valmistelun yhteydessä.

**Vieras-käyttäjän tai Vieras:** Voi olla useita Vieras käyttäjät/vieraiden AAD kohden. Vierailevien käyttäjien on rajattu oikeuksien AAD alihallintaan. Tuloksena vierailijat ei voi valmistella esimääritettyjä ratkaista AAD alihallintaan.

Lisätietoja on seuraavissa resursseissa:

- [Luo tai Muokkaa käyttäjiä Azure AD][lnk-create-edit-users]
- [Sovelluksen roolien AAD][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure tilauksen järjestelmänvalvojan roolit

Azure järjestelmänvalvoja-roolien hallinta voi yhdistää tilauksen Azure AD-vuokraajan.

Voit tarkistaa, Lisätietoja on artikkelissa [miten voit lisätä tai muuttaa Azure työtovereiden järjestelmänvalvojan, palvelun järjestelmänvalvoja ja tilin järjestelmänvalvojaan]Azure työtovereiden järjestelmänvalvojan, palvelun järjestelmänvalvoja ja tilin järjestelmänvalvojan rooleista[lnk-admin-roles].

## <a name="application-roles"></a>Sovelluksen roolit

Sovelluksen roolit hallita esimääritettyjä ratkaisu laitteisiin.

On kaksi määrittämät ja sovelluksessa, jossa luodaan, kun esimääritettyjä ratkaisu valmistella yhden implisiittinen roolia.

-   **Järjestelmänvalvoja:** Täydet oikeudet, hallinta ja poista laitteet

-   **Vain luku-tilassa:** Mahdollisuus tarkastella laitteiden on

-   **IMPLISIITTINEN luku vain:** Tämä on sama kuin vain luku-tilassa, mutta myönnetään kaikille käyttäjille AAD vuokraajan. Tämä on tehty asiasta kehityksen aikana. Voit poistaa tämän roolin muokkaamalla [RolePermissions.cs] [ lnk-resource-cs] lähdetiedosto.

### <a name="changing-application-roles-for-a-user"></a>Sovelluksen käyttäjän roolien muuttaminen

Seuraavien ohjeiden avulla voit tehdä käyttäjän Active Directory, että esimääritettyjä järjestelmänvalvoja.

Sinun on oltava AAD yleisen järjestelmänvalvojan roolien käyttäjän muuttaminen:

1. Siirry [Azure perinteinen portal][lnk-classic-portal].

2. Valitse **Active Directorysta**.

3. Napsauta AAD alihallintaan (tämä on valitsemaasi azureiotsuite.com kun ratkaisu valmisteltu) nimeä.

4. Valitse **sovellukset**.

5. Valitse sama esimääritettyjä ratkaisun nimi sovelluksen nimi. Jos et näe sovellus-luettelossa, Vaihda **Näytä** avattavassa alaspäin **yritykseni omistaa sovellukset** ja napsauttaa valintamerkkiä.

7. Valitse **käyttäjiä**.

8. Valitse käyttäjä, johon haluat vaihtaa roolit.

9. Valitse **Määritä** ja valitse rooli (kuten **järjestelmänvalvoja**) haluat määrittää käyttäjälle, valitse valintamerkkiä.

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Olen palvelun järjestelmänvalvoja ja haluan muuttaa tilaukseni ja tietyn AAD palvelutili hakemiston yhdistämisen. Miten tämän voi tehdä?

1. Siirry [Azure perinteinen portal][lnk-classic-portal], Valitse palveluluettelosta vasemmassa reunassa **asetukset** .

2. Valitse tilaus haluat muuttaa hakemisto-määrityksiä.

3. Valitse **Muokkaa hakemisto**.

4. Valitse **kansio** haluat avattavan luettelon käyttäminen. Valitse oikeaa nuolta.

5. Vahvista hakemisto-määritys ja vaikuttaa apuyhteyshenkilöiden. Huomaa, että jos siirrät toisen kansion, kaikki apuyhteyshenkilöiden alkuperäisen hakemiston poistetaan.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Olen toimialueen käyttäjä/jäsen AAD vuokraajan ja luomani esimääritettyjä ratkaisu. Kuinka voin Hae määritetty rooli oma sovelluksen?

Pyytää määrittämään sinulle Yleinen järjestelmänvalvoja voi määrittää roolit käyttäjille itsellesi oikeuksien saaminen AAD-vuokraajan Yleinen järjestelmänvalvoja tai pyydä yleisen järjestelmänvalvojan roolin määritteleminen. Jos haluat muuttaa AAD vuokraajan esimääritettyjä ratkaisu on otettu käyttöön, katso seuraava kysymys.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Miten vaihdan remote seurantaa esimääritettyjä ratkaisu-ja sovelluksen AAD vuokraajan?

Voit suorittaa cloud käyttöönoton <https://github.com/Azure/azure-iot-remote-monitoring> ja ota uudelleen juuri luomasi AAD palvelutili kanssa. Koska olet Yleinen järjestelmänvalvoja oletusarvoisesti kun luot uuden AAD vuokraajan, sinulla on oikeudet käyttäjien lisääminen ja roolien kyseisille käyttäjille.

1. Luo uuden AAD kansion [Azure hallinta-portaalin][lnk-classic-portal].

2. Siirry <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Suorita `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (esimerkiksi `build.cmd cloud debug myRMSolution`)

4. Kun sinulta kysytään, Määritä **tenantid** on sen sijaan, että edellisen vuokraajan juuri luotuun alihallintaan.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Haluan muuttaa palvelun järjestelmänvalvoja tai muiden järjestelmänvalvojan kirjautuessa sisään organisaation tilille

Katso lisätietoja tukiartikkelista [muuttaminen palvelun järjestelmänvalvoja ja muiden järjestelmänvalvojan kirjautuessa sisään organisaation tilille][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Miksi tämä virhe? "Tili ei ole oikeuksia luot ratkaisun. Tarkista järjestelmänvalvojalta tai kokeile eri tilillä."

Tutustu seuraavassa kuvassa:

![][img-flowchart]

> [AZURE.NOTE] Jos näet edelleen tarkistettuaan voit virhe on yleinen järjestelmänvalvoja AAD vuokraajan ja muiden järjestelmänvalvojan tilauksen, on tili järjestelmänvalvoja poistaa käyttäjän ja määritä uudelleen tarvittavat oikeudet tässä järjestyksessä: Lisää käyttäjä Yleinen järjestelmänvalvoja ja lisää käyttäjän kanssasi järjestelmänvalvojana Azure-tilauksessa. Jos ongelmat jatkuvat, ota yhteyttä [Ohje ja tuki][lnk-help-support].

**Miksi näen tämän virheen, jos minulla on Azure tilauksen?** *Valmiiksi määritetyt ratkaisuja luomiseen tarvitaan Azure tilauksen. Voit luoda ilmainen kokeiluversio tili vain muutaman minuutin.*

Jos olet varma ole Azure-tilausta, tarkista tilauksen yhdistäminen vuokraajan ja varmistaa oikean vuokraajan on valittuna olevaa avattavaa valikkoa. Jos olet vahvistaa haluamasi vuokraajan on oikein, toimi yllä olevassa kaaviossa ja Vahvista tilaus ja AAD tämän vuokraajan yhdistäminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisää IoT Suite, katso kuinka voit [mukauttaa valmiiksi määritettyjä ratkaisu][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
