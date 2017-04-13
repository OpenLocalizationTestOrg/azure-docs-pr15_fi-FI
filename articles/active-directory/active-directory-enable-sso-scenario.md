<properties
    pageTitle="Azure Active Directory sovellusten hallinta | Microsoft Azure"
    description="Tässä artikkelissa Azure Active Directory-integrointi paikallisen, cloud ja SaaS sovellusten etuja."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Azure Active Directory sovellusten hallinta

Todellinen työnkulun tai sisällön lisäksi yritysten on kaikissa sovelluksissa kaksi edellytykset:

1. Voit parantaa tuottavuutta, sovellukset on helppo tutkia ja käyttää

2. Suojaus- ja seurantatyökaluilla käyttöön organisaatiossa on ohjausobjekti ja kuka voi ja käyttää todella kunkin sovelluksen valvonta

Cloud sovellusten maailmanlaajuisesti tämä parhaiten onnistuu käyttämällä tunnusta ",*KENELLÄ on oikeus tehdä mitä*" ohjausobjektiin.

Valitse tietojenkäsittely termejä:

- *Kuka* on nimeltään *identity* - käyttäjien ja ryhmien hallinta

- *Mitä* kutsutaan *käyttöoikeushallinta* – suojatun resurssien käyttöoikeuksien hallinta

Molemmat osat yhdessä kutsutaan *käyttäjätieto- ja käytön hallinta (IAM)*, joka on määritetty [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) ryhmän "*suojauksen laji, jonka avulla oikealle käyttäjät voivat käyttää oikealla oikeiden resurssien varaaminen kertaa oikealle syistä*".

OK, eli mikä on ongelma? Jos IAM *ei hallita* yhdessä paikassa integroitu-ratkaisuun:

- Käyttäjätietojen järjestelmänvalvojilla on yksitellen Luo ja Päivitä kaikki sovellukset-käyttäjätilit erikseen aikaa ja tarpeettomat tehtävän.

- Käyttäjillä on memorize sovelluksia, ne pitää käsitellä useita käyttöoikeudet. Tämän vuoksi käyttäjien yleensä salasanansa muistiin tai käyttää muita salasana-ratkaisuja, jossa esitellään muiden tietojen tietoturvauhkia.

- Tarpeettomien aikaa toiminnoista vähentää ajan käyttäjien ja järjestelmänvalvojien työskentelevät business toimintoja, jotka Suurenna yrityksesi alimman rivin.

Mitä, yleensä estää organisaatiot-antamista integroitu IAM ratkaisuja?

- Useimmat teknisiä ratkaisuja perustuvat ohjelmiston ympäristöissä, jotka on otettu käyttöön ja mukauttaa kunkin organisaation omia sovellusten mukaan.

- Cloud sovellukset ovat usein hyväksytyt siirtonopeuden kuin IT-organisaatio voi integroida IAM ratkaisuja.

- Turvallisuus- ja sillä edellyttää muita mukauttaminen ja integrointi saavuttamiseksi täydellinen E2E skenaarioita.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directoryn integroitu-sovellusten kanssa

Azure Active Directory on Microsoftin täydellinen jäsenyys Service (IDaaS)-ratkaisun, joka:

- Mahdollistaa IAM kuin pilvipalveluun 

- Tarjoaa keskitetyn käyttöoikeushallinta, single-merkki (SSO)- ja raportointi 

- Tukee Integroitu sovellus-valikoimassa, mukaan lukien Salesforce, Google Apps-ruutuun, Concur ja muut [sovellukset tuhansia](https://azure.microsoft.com/marketplace/active-directory/) käyttöoikeushallinta. 


Azure Active Directory-hakemistosta kaikki sovellukset julkaiset kumppanien ja asiakkaiden (yrityksen tai kuluttaja) on samat tunnistetiedot ja ominaisuuksien hallinta.<br> Näin voit vähentää huomattavasti toiminnallisia kustannuksia.

Entä jos tarvitset Toteuta sovellus, joka ei ole vielä luettelossa sovelluksen valikoiman? Vaikka tämä on hieman enemmän aikaa kuin määrittäminen sovelluksen valikoimasta sovellusten SSO-Azure AD avulla voit ohjatun toiminnon, joka auttaa määritykset.

Azure AD arvon ulottuvat "vain" pilven sovellukset. Voit käyttää sitä paikallisen sovellusten mukaan turvallinen etäkäyttö. Suojatun etäkäyttöpalvelimen, jossa voit poistaa edellyttää VPN-yhteydet tai muiden perinteinen etäkäyttöpalvelimen hallinta käyttöotot.

Antamalla käytön keskitetyn hallinnan ja kertakirjautuminen (SSO) kaikkien sovellusten Azure AD sisältää tärkeimmät suojaus ja tuottavuutta ongelmien ratkaisun.

- Käyttäjät voivat käyttää useita sovelluksia yksi merkki, jossa enemmän aikaa tulot luotaessa tai yrityksen valmis toiminnot-toimintoja.

- Käyttäjätietojen järjestelmänvalvojat voivat hallita sovellukset yhdessä paikassa.

Etu käyttäjän ja yrityksesi on selvää. Järjestelmänvalvojan tunnistetiedot ja organisaation voit hyötyä tarkemmin.

## <a name="integrated-application-benefits"></a>Integroitu sovellus edut

SSO-prosessi on kaksi vaihetta:

- Todennus-prosessi, jossa vahvistetaan käyttäjän tunnistetiedot.

- Salli tai estä pääsy käyttöoikeuskäytäntö resurssin päätös luvan.

Kun käytät Azure AD SSO ottaminen käyttöön ja sovellusten hallinta:

- Käyttäjän paikallisen (esimerkiksi AD) tai Azure AD-tilin todennus on valmis.

- Luvan suorittaa Azure AD varauskentät ja suojaus-käytännössä varmistaa, että yhdenmukainen käyttökokemuksesta ja voit lisätä tehtävän, sijainnit ja MFA ehdot jokin sovellus, riippumatta siitä, sen sisäinen ominaisuuksia.

Se on tärkeää ymmärtää, että tapaa, jolla luvan on säädetty kohdesovelluksen vaihtelee sen mukaan, miten sovellus on integroitu Azure AD.

- **Sovellusten integroitu valmiiksi tarjoaja** Office 365: ssä ja Azure, kuten nämä ovat sovellusten rakennettu Azure AD ja käyttäisit niiden täydellinen tunnistetietojen ja käytön hallintatoiminnot varten. Nämä sovellukset on otettu käyttöön luettelotiedot ja vahvistustunnuksen liittoutumispalvelujen.

- **Sovellusten integroitu valmiiksi Microsoftin ja mukautetut sovellukset.** Nämä ovat itsenäisten cloud-sovellukset, jotka luottavat sisäinen sovelluksen-kansio ja voi toimia ulkopuolisista Azure AD. Nämä sovellukset on käytössä sovelluksen tietyn tunnistetiedot sovelluksen tiliin yhdistetty lähettämällä. Sovelluksen ominaisuudet, mukaan tunnistetieto voi olla federation tunnuksen tai käyttäjänimi ja salasana tilille, jolla on aiemmin valmisteltu-sovelluksessa.

- **Paikallisen sovellukset** Sovellusten julkaista käyttöönottoon ensisijaisesti paikallisen sovellukset Azure AD-sovelluksen välityspalvelimen kautta. Nämä sovellukset ovat riippuvaisia Keski paikalliseen-kansioon, kuten Windows Server Active Directory. Nämä sovellukset on käytössä, käynnistävä välityspalvelimen pitää sovelluksen sisällön peruskäyttäjän aikana honoring paikallisen Sign vaatimus.

Esimerkiksi käyttäjä yhdistää organisaatiossa, jos haluat luoda tili käyttäjän ensisijaisen Sign toimille Azure AD. Jos tämän käyttäjän edellyttää hallitun sovelluksen, kuten Salesforce-myös haluat luoda tili kyseisen käyttäjän Salesforce ja linkittää sen Azure-tili, jotta Kertakirjautumista toimi. Kun käyttäjä jättää organisaation, on suositeltavaa poistaa Azure AD-tilin ja kaikki IAM vastaavaan tilin tallentaa käyttäjällä on pääsy sovellukset.

## <a name="access-detection"></a>Access-tunnistus

Nykyaikainen yrityksissä IT-osastoille eivät usein tietoinen kaikki cloud-sovelluksia, jotka ovat käytössä. Yhdessä Cloud App etsiminen Azure AD avulla voit ratkaista esiintyvien nämä sovellukset.

## <a name="account-management"></a>Tilinhallinta

Perinteisesti eri sovelluksiin tilien hallinta on manuaalinen prosessi maksettavan korvauksen IT tai tue oman organisaation. Azure AD täysin automaattinen tilinhallinta kaikkien palvelusovellusten integroitu tarjoaja ja valmiiksi integroitu Microsoft tukevat automaattinen käyttäjän valmistelu tai SAML JIT sovellusten välillä.

## <a name="automated-user-provisioning"></a>Automaattinen käyttäjän valmistelu

Jotkin sovellukset avulla automation liittymät luominen ja poistaminen tai käytöstä poistamisen tilit. Jos palveluntarjoaja tarjoaa tällaisten käyttöliittymään, se on hyödyntää Azure AD. Tämä pienentää toiminnallisia kustannukset, koska hallintatehtäviä tapahtuu automaattisesti ja parantaa ympäristön suojaus, koska se pienenee luvattoman käytön mahdollisuutta.

## <a name="access-management"></a>Käyttöoikeuksien hallinnan

Azure AD avulla voit hallita sovellusten yksittäisiä tai varausten perustuva sääntö. Voit myös delegoida käyttää hallinta voit varmistaa parhaan valvonta ja vähentää olevien tukipalvelu organisaation käyttäjillä.

## <a name="on-premises-applications"></a>Paikallisen sovellukset

Myös sisäinen sovelluksen välityspalvelimen avulla voit julkaista paikallisen-sovelluksia käyttäjille tuloksena on molemmat yhdenmukaisia käyttää kokemusta Moderni cloud-sovelluksen ja mitä etuja Azure AD-ominaisuuksien seurannassa, raportoinnissa ja suojaus.

## <a name="reporting-and-monitoring"></a>Ilmoittaminen ja seuranta

Azure AD voi valmiiksi integroitu ilmoittaminen ja seuranta ominaisuuksia, joilla voit tietää, kuka on access-sovellukset ja niitä todella käyttää niitä.

## <a name="related-capabilities"></a>Aiheeseen liittyvät ominaisuudet

Voit suojata sovellustesi hajautetun käytäntöjen ja valmiiksi integroitu MFA kanssa Azure AD. Saat lisätietoja Azure MFA on artikkelissa [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Käytön aloittaminen

Aloita Azure AD-sovellusten integraation, tutustu [integrointi Azure Active Directory-sovellusten käytön kanssa aloitusopas](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Katso myös

[Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
