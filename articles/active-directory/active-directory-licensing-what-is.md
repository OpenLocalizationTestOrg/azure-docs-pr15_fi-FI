<properties
    pageTitle="Mikä on Microsoft Azure Active Directory-käyttöoikeuksien myöntämisestä? | Microsoft Azure"
    description="Kuvaus Microsoft Azure Active Directory-käyttöoikeudet, miten se toimii, aloittamisesta ja parhaita käytäntöjä, esimerkiksi Office 365: ssä ja Microsoft Intune- ja Azure Active Directory-Premium Basic versiot"
    services="active-directory"
      keywords="Azure AD-käyttöoikeuksien myöntämisestä"
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

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Mikä on Microsoft Azure Active Directory-käyttöoikeuksien myöntämisestä?

##<a name="description"></a>Kuvaus
Azure Active Directory (Azure AD) on Microsoftin käyttäjätietojen Service (IDaaS) ratkaisu ja ympäristössä. Azure AD tarjotaan Azure AD-vapaa, joka on käytettävissä minkä tahansa Microsoft-palvelun, kuten Office 365: ssä, Dynamics, Microsoft Intune ja Azure vakiovalikoimasta toiminnalliset ja tekniset versioita (Azure AD Luo kulutus kulut tässä tilassa), Azure AD maksanut versiot, kuten yrityksen Mobility Suite (EMS), Azure AD Premium ja Basic sekä Azure multi-factor Authentication (MFA). Monia Microsoft online Services-palveluiden, kuten eniten Azure AD maksettu versiot toimitetaan käyttäjäkohtaisten oikeuksien kautta, kuin Office 365: ssä, Microsoft Intune ja Azure AD. Tällaisissa tapauksissa palvelun hankkiminen esitetään on vähintään yksi tilausta ja jokaisen tilauksen sisältää vuokraajan vanhat hankkiminen säilytettävien käyttöoikeuksien lukumäärä. Käyttäjäkohtaisten oikeuksien saavutetaan käyttöoikeusmääritykset käyttäjän ja tuotteen käyttäjän service-komponenttien ottaminen käyttöön ja muissa yhtä etukäteen maksettua välillä linkin kautta.

[Kokeile nyt Azure AD-premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Azure AD-hallintaportaaliin kuuluu Azure perinteinen portaalin. Kun käyttämällä Azure AD ei edellytä mitään Azure hankintoja, portaalin käyttäminen edellyttää aktiivinen Azure-tilaus tai [Azure-kokeiluversioon](https://azure.microsoft.com/pricing/free-trial/).

Katso Azure AD-palvelun ominaisuuksien laaja yleiskatsaus, [Mikä on Azure AD](active-directory-whatis.md).
[Lisätietoja Azure AD-palvelutasot](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Azure käytön mukaan tilaukset ovat eri: kun esittää myös hakemistossa, nämä tilaukset Azure resurssien luomisen ja yhdistäminen maksutapa. Tässä tapauksessa on ei käyttöoikeutta-arvo tilaukseen liittyvää. Käyttäjien liitos käyttäjien pääsyn tilauksen resurssien hallinta-tilauksen saavutetaan myönnät käyttöoikeudet toimivat Azure resursseja yhdistää tilaukseen.


##<a name="how-does-azure-ad-licensing-work"></a>Miten Azure AD-käyttöoikeuksien myöntämisestä toimii?

Käyttöoikeus-pohjainen (oikeuden-pohjainen) Azure AD services työn aktivoimalla Azure Active directory-palvelun vuokraajan tilauksen. Kun tilaus on aktiivinen palvelun-ominaisuuksia voi hallita directory-palvelun Järjestelmänvalvojat ja käyttöoikeus käyttäjät käyttävät.

Osta tai ottaa käyttöön yrityksen Mobility Suite, Azure AD Premium tai Azure AD Basic, hakemistossa päivitetään tilaukseen, mukaan lukien sen voimassaoloaika ja maksetun käyttöoikeudet. Tilauksen tietoja, kuten tila, seuraava lifecycle-tapahtuma ja määritetty, tai se on käytettävissä käyttöoikeuksien määrä on saatavana Azure perinteinen portaalin tietyn kansion käyttöoikeudet-välilehdessä. Tämä on myös hyvä hallita oman käyttöoikeusmääritykset.

Yhden tai useamman palvelusopimusten vaihtoehdot, koostuu jokaisen tilauksen kunkin palvelutyyppi; mukana toiminnan tasoa yhdistäminen esimerkiksi Azure AD-Azure MFA, Microsoft Intune, Exchange Online tai SharePoint online-tilassa. Azure AD-käyttöoikeuksien hallinta ei edellytä suunnitelman tason hallinta. Tämä on sama kuin Office 365: n, jossa voit hallita niiden käyttöä järjestelmään sisältyvien palveluiden lisämäärityksiä tässä tilassa on riippuvainen. Azure AD on riippuvainen palvelun määrityksessä ominaisuuksien käyttöönotto ja yksittäisten käyttöoikeuksien hallinta.

Yleensä Azure AD-Tilaustiedot hallitaan Azure perinteinen portal tietyn kansion käyttöoikeudet-välilehdessä. Azure AD-tilaukset, lukuun ottamatta Azure AD Premium ei näy Office-portaalissa.

> [AZURE.IMPORTANT] Azure AD-Premium ja Basic-sekä yrityksen Mobility Suite tilaukset on ainoastaan niiden valmistellun kansio/vuokraajan. Tilauksia ei voi jakaa kansioiden välillä tai käytettävä oikeuta käyttäjien muihin kansioihin. Tilauksen siirtäminen kansioiden välillä on mahdollista, mutta vaatii lähettämistä tuki lippu tai peruutus- ja ostaa kyseessä suoraan.

> Kun olet ostanut Azure AD- tai Enterprise Mobility Suite kautta Volume Licensing-tilauksen aktivointi tapahtuu automaattisesti kun sopimus sisältää muita Microsoft Online-palveluja, kuten Office 365.

Maksettu Azure AD-ominaisuuksien kattavat hakemiston kaikki. Esimerkkejä:
- Ryhmän perustuva liitos-sovellukset, jossa on käytössä hallinnoit tietylle sovellukselle.
- Erikoissuodatus ja Omatoiminen ryhmän hallinta-ominaisuuksia on käytettävissä, valitse kansio-määritysten tai tietyn ryhmän.
- Premium suojauksen raportit ovat raportointi-välilehdessä
- Cloud sovelluksen etsinnän näyttää käyttäjätiedot-kohdassa Azure-portaalissa.

###<a name="assigning-licenses"></a>Käyttöoikeuksien määrittämisestä
Samalla, kun tilaus on kaikki sinun on määritettävä maksettu ominaisuuksia, oman Azure AD maksettu ominaisuuksien käyttäminen edellyttää jakaminen käyttöoikeuksien oikean henkilöille. Yleensä kuka tahansa käyttäjä, kuka saa käyttöoikeuden tai joilla hallitaan Azure AD maksettu ominaisuus on määritettävä käyttöoikeus. Käyttöoikeuden määritys on käyttäjän ja ostettujen palvelun, kuten Azure AD Premium, Basic tai yrityksen Mobility Suite yhdistämisen.

Käyttäjät, jotka hakemistossa pitäisi olla käyttöoikeuden hallinta on helppoa. Se onnistuu, määrittämällä ryhmälle tehtävän luominen Azure AD-hallintaportaaliin kautta tai määrittämällä käyttöoikeudet portal, PowerShell tai ohjelmointirajapinnan oikean henkilöille. Kun määrität ryhmän käyttöoikeuksia, kaikille ryhmän jäsenille määritetään käyttöoikeuden. Jos käyttäjät lisätään tai poistetaan ryhmästä määritetään tai poistaa käyttöoikeutta. Ryhmämääritys voit hyödyntää käytettävissä minkä tahansa ryhmän hallinta ja vastaa sovellusten ryhmän perustuva liitos. Käytä tätä tapaa, voit säännöt määritetään siten, että kaikki käyttäjät hakemistossa määritetään automaattisesti, varmista, että kaikki, joilla on tarvittavat tehtävänimike on käyttöoikeuden tai delegoida edes päätös organisaation muille valvojille.

Kuka tahansa käyttäjä, puuttuu käyttö sijainti perivät ryhmän perustuva käyttöoikeusmääritykset kanssa kuormituksen aikana kansion sijainti. Järjestelmänvalvoja voi muuttaa tätä sijaintia milloin tahansa. Mihin automaattinen määritys ei onnistunut virheen vuoksi tapauksissa käyttäjätiedot-kohdassa, että käyttöoikeustyyppi vaikuttavat valtion.

##<a name="getting-started-with-azure-ad-licensing"></a>Aloittaminen kanssa Azure AD-käyttöoikeuksien myöntämisestä

Azure AD käytön aloittaminen on helppoa; Voit luoda hakemiston rekisteröimässä vapaa Azure, kokeiluversion osana. [Lue lisää siitä organisaationa rekisteröimässä](sign-up-organization.md). Seuraavat auttavat Varmista, että hakemistossa tasataan parhaiten muiden Microsoftin palvelujen saattaa olla muissa tai tarjoaman suunnittelu ja saamiseksi palvelun tavoitteet.

Tässä on muutama parhaat käytännöt:
- Jos sinulla on jo käytössä jokin Microsoftin organisaation palveluista, sinulla on jo Azure AD-kansio. Tässä tapauksessa voit jatkaa samassa kansiossa muista palveluista niin, että core jäsenyyksien hallinta, mukaan lukien valmistelu ja hybrid SSO-voidaan käyttää palveluita yli. Käyttäjien on yksittäinen go.microsoft.com/fwlink/?LinkId=525059 ja hyötyvät tehokkaan ominaisuuksia palveluita yli. Tuloksena Jos ostaminen Azure AD, joka on maksettu palvelun käytettävyyttä, on suositeltavaa, että käytät samassa kansiossa toiminto.
- Jos aiot käyttää Azure AD samat käyttäjien (esimerkiksi kumppanit ja asiakkaat) tai jos haluat arvioida Azure AD-palvelujen ja haluat tehdä yksinään tuotannon-palvelun tai jos etsit määrittäminen palvelujen hallitussa ympäristössä, on suositeltavaa, että sinun on luotava uusi kansio palvelun perinteinen Azure Azure-portaalissa. [Lisätietoja uusien Azure AD-kansion Azure perinteinen portaalissa](active-directory-licensing-directory-independence.md). Uusi kansio luodaan tilisi ulkoisena käyttäjänä, jolla on yleisen järjestelmänvalvojan oikeudet. Kun kirjaudut sisään tällä tilillä Azure perinteinen-portaaliin, osaat tähän kansioon ja käyttää hakemiston hallintatoimia. On suositeltavaa luoda paikallisen tilin ja haluamasi oikeudet hallita muiden Microsoftin palvelujen (niitä ei voi käyttää Azure perinteinen yritysportaalin kautta). [Lue lisää Azure AD-käyttäjätilit luomisesta](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD tukee "ulkoisille käyttäjille," on käyttäjätilit, jotka on luotu käyttämällä Microsoft tili (MSA) tai toisesta kansiosta Azure AD-jäsenyyden Azure AD-esiintymän. Vaikka emme varattu laajentaminen tämän vain, jos kaikkiin Microsoftin organisaation palveluja, tällä hetkellä nämä tilit ei tueta joitakin palveluiden kokemukset; esimerkiksi Office 365-hallintaportaaliin ei tällä hetkellä tue nämä käyttäjät. Tuloksena ulkoisten käyttäjien Microsoft-tilien kanssa ei voi käyttää Office 365-hallintaportaaliin lainkaan, kun Ulkoiset käyttäjät muiden Azure AD-kansioista ohitetaan. Jälkimmäisessä, vain käyttäjän paikallisen tilin, Azure AD- tai Office 365: ssä kansio, jossa käyttäjä on alun perin luotu, on nämä kokemukset kautta.

Ilmoitettuja, Azure AD on maksettu versiosta. Seuraavia versioita on sisään Osta käytettävyyden pieniä eroja:


| Tuotteen   | EA/VOLYYMIKÄYTTÖOIKEUDEN     | Avaa  |   CSP |   MPN käyttöoikeudet  |   Suora hankkiminen | Kokeiluversio |
|---|---|---|---|---|---|---|
| Yrityksen Mobility Suite |   X | X | X | X |  |      X |
| Azure AD-Premium  | X | X | X |   | X | X |
| Azure AD-Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Valitse vähintään yksi käyttöoikeus-kokeiluversiot
 Kaikissa tapauksissa voit ottaa Azure AD Premium- tai Enterprise Mobility Suite kokeilutilauksen valitsemalla haluamasi käyttöoikeudet-välilehdessä hakemistossa tietyn kokeiluversio. Joko kokeiluversio sisältää 30 päivää tilauksen 100 käyttöoikeudet.

![Azure Active Directory-kokeiluversion käyttöoikeus-Palvelupaketit](./media/active-directory-licensing-what-is/trial_plans.png)

![Yrityksen Mobility Suite kokeiluversion käyttöoikeus-Palvelupaketit](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktiivinen kokeiluversion käyttöoikeus-Palvelupaketit](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Käyttöoikeuksien määrittäminen
Kun tilaus on valittuna, voit määrittää itsellesi käyttöoikeudet ja Päivitä selain varmistaakseen, näyttöön tulee kaikkia ominaisuuksia. Seuraava vaihe on käyttöoikeudet määrittää käyttäjät, jotka on sekä lisätä maksettu Azure AD-ominaisuuksia. Olemme edellä mainittua "käyttöoikeuksien määrittäminen-kohdassa, voit tehdä tämän helpoiten edustava haluamasi yleisön ryhmän ja sen käyttöoikeutta; Tällä tavalla käyttäjät, jotka lisätään tai poistetaan ryhmästä päälle sen elinkaaren aikana on määritetty tai poistaa käyttöoikeuden.

Jos haluat määrittää käyttöoikeuden ryhmän tai yksittäisille käyttäjille, valitse haluat liittää, ja napsauta komentopalkista **määrittää** käyttöoikeuden suunnitelma.

![Aktiivinen kokeiluversion käyttöoikeus-Palvelupaketit](./media/active-directory-licensing-what-is/assign_licenses.png)

Kerran valitun suunnitelman määritys-valintaikkunassa voit valita käyttäjät ja lisäämällä ne **määrittää** sarakkeen oikealla. Voit selata käyttäjä-luetteloon tai etsiä tietyille käyttäjille avulla Haluatko lasi valitsemalla rakennenäkymäalueen oikeassa yläkulmassa käyttäjän ruudukko. Ryhmien määrittäminen "Ryhmät" Valitse **Näytä** -valikko ja valitse sitten Päivitä tehtävät, jotka näkyvät oikealla tarkistus-painiketta.

![Käyttöoikeuksien määrittämisestä ryhmille](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Voit etsiä tai Selaa ryhmiä ja lisätä ne **määrittää** sarakkeen samalla tavalla. Nämä avulla voit määrittää käyttäjät ja ryhmät yhdellä kertaa yhdistelmä. Viimeistele määritys, napsauta sivun oikeassa alakulmassa tarkistus-painiketta.

![Käyttöoikeuden tehtävän edistymisen viesti](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Jos ryhmä on määritetty, jäsenten perivät käyttöoikeudet 30 minuutin kuluessa, mutta yleensä 1 – 2 minuutin kuluessa.

Tehtävän virheet Azure AD-käyttöoikeusmääritykset aikana voi tapahtua, mutta se on suhteellisen harvoin. Tehtävän mahdolliset virheet on rajoitettu:
- Tehtävän ristiriita - kun käyttäjä on aiemmin määritetty käyttöoikeus, joka ei ole yhteensopiva nykyinen käyttöoikeus. Tässä tapauksessa määrittäminen uusilla käyttöoikeuksilla edellyttävät sitä edeltävän poistaminen.
- Ylitti saatavilla olevia käyttöoikeuksia - kun-varattujen ryhmien määrä ylittää saatavilla olevia käyttöoikeuksia käyttäjien tehtävän tilan vastaavat vuoksi puuttuu käyttöoikeuksien määrittäminen epäonnistui.

###<a name="view-assigned-licenses"></a>Määritetty olevien käyttöoikeuksien tarkistaminen

Yhteenvetonäkymä määritetyt käyttöoikeudet, mukaan lukien käytettävissä, määritetty ja seuraavan tilauksen elinkaari tapahtuman näkyvät **käyttöoikeudet** -välilehdessä.

![Tarkastele voimassa olevia määritetyt käyttöoikeudet](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Yksityiskohtainen luettelo määritetty käyttäjille ja ryhmille sekä tehtävän tilan ja polku (suoraan tai perittyjä vähintään yksi ryhmistä) on käytettävissä, kun siirtyminen kyselyjä käyttöoikeuden suunnitelma.

![Määritetty käyttöoikeus-palvelupaketin käyttöoikeudet yksityiskohtien näyttäminen](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Käyttöoikeuksien poistaminen on yhtä helppoa kuin varataan. Jos käyttäjä on määritetty suoraan tai varatut ryhmän, voit poistaa käyttöoikeuden käyttöoikeustyyppi valitsemalla, valitsemalla **Poista**, Poista luettelosta käyttäjän tai ryhmän lisääminen ja toiminnon vahvistaminen. Voit myös avata käyttöoikeuden lajin, tietyn käyttäjän tai ryhmän valitseminen ja valitsemalla **Poista** komentopalkista. Voit lopettaa käyttäjän ryhmästä käyttöoikeuden periytymisen, poistaa käyttäjän yksinkertaisesti ryhmästä.

###<a name="extending-trials"></a>Yritykset laajentaminen

Kokeiluversion tunnisteet asiakkaat ovat käytettävissä kuin Omatoiminen palvelun Office 365-portaalissa. Asiakas-järjestelmänvalvoja voi siirtyä [Office-portaali](https://portal.office.com/#Billing) (access määräytyy Office portaalissa käyttöoikeudet) ja valitse Azure AD Premium-kokeiluversio. Napsauta **Jatka kokeiluversio** -linkkiä ja noudata ohjeita. Sinun on annettava luottokortin tiedot, mutta se ei ole ladattu.

![Office-portaalissa käyttöoikeuden kokeiluversion laajentaminen](./media/active-directory-licensing-what-is/extend_license_trial.png)

Asiakkaiden myös pyytää kokeiluversion käyttöajan lähettämällä tukipyynnön. Asiakkaan järjestelmänvalvoja voivat siirtyä Office 365: n portaalin [tuen sivun](http://aka.ms/extendAADtrial) (access määräytyy Office-tuen sivun käyttöoikeudet). Valitse tällä sivulla "Tilaukset ja yritykset" ominaisuudet-kohdasta ja valitse Oire "kokeiluversion kysymykset". Tietojen syöttäminen lopuksi olosuhteissa

![Laajentaminen käyttämällä tukipyynnön käyttöoikeus-kokeiluversio](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt voit ehkä haluat määrittää ja käyttää joidenkin Azure AD Premium-ominaisuuksia.

- [Omatoiminen salasanan palauttaminen](active-directory-manage-passwords.md)
- [Omatoiminen ryhmän hallinta](active-directory-accessmanagement-self-service-group-management.md)
- [Azure AD Connect nummet](active-directory-aadconnect-health.md)
- [Ryhmämääritys-sovellukset](active-directory-manage-groups.md)
- [Azure Monimenetelmäisen todentamisen](../multi-factor-authentication/multi-factor-authentication.md)
- [Suora Azure AD Premium käyttöoikeuksien hankkiminen](http://aka.ms/buyaadp)
