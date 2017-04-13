<properties
   pageTitle="Ulkoisten käyttäjätietojen Azure Active Directoryn avulla voidaan verrata | Microsoft Azure"
   description="Vertaa Azure Active Directory-B2B yhteistyön B2C ja usean vuokraajan App tukemiseen todennus- ja ulkoisen käyttäjätietoja"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Vertailu voidaan hallita ulkoisten käyttäjätietojen Azure Active Directoryn avulla

Lisäksi mobiilikäyttö SaaS sovellusten käyttöoikeuksien hallinta, Azure Active Directory (Azure AD) voivat auttaa organisaatiota resurssien jakaminen liikekumppaneita ja yritykset ja kuluttajille sovelluksia.

## <a name="developing-applications-for-businesses"></a>Kehityssovellusten yrityksille

Haluatko tarjota palvelun tai sovelluksesta, palkanlaskennan palvelun, kuten yritysten? Azure AD on identity-ympäristö, jonka avulla voit integroida saumattomasti miljoonia organisaatioissa, joissa on jo määritetty Azure AD osana käyttöönottaminen Office 365- tai enterprise-muiden sovellusten rakentamiseen.

**Esimerkki:** Pharmaceutical jälleenmyyjä on lääketieteellinen tarvikkeiden ja terveydenhuollon alan järjestelmien tiedot. Ne tarvittavat kentän analytics sovellus lääketieteellinen käytännöt ja osoittava asiakkaillesi hallita omia käyttäjätietoja. Yrityksen on Azure AD identity-ympäristö kuin niiden käytäntöjen hallinta sovelluksen tarjoavat asiakkaidensa ät-merkin Azure AD-käyttäjätietojen määrittäminen tarvittaessa. Lisätietoja on artikkelissa [Azure Active Directory-Sovelluskehittäjän opas](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Yrityksen resurssien business kumppanien käyttöoikeus ottaminen käyttöön

Sinulla on liikekumppanien tai muut käyttäjät, jotka tarvitsevat yritysresurssien, kuten SharePoint-sivustoon tai ERP-järjestelmän yrityksen ulkopuolisten? Azure AD avulla järjestelmänvalvojat myöntäminen ulkoisille käyttäjille (joka voi tai ei ehkä ole Azure AD) yrityksen sovellukset kertakirjautuminen. Tämä parantaa käyttäjät menettävät access, kun he jättävät kumppaniorganisaation samalla kun sinä hallinnoit organisaation käytäntöjen suojaus. Tämä myös yksinkertaistaa hallintaa, kun sinun ei tarvitse hallita ulkoisten kumppani, kansion tai kohti kumppanin federation yhteydet.

**Esimerkki:** Imaging yrityksen jälleenmyyjät sisältää valokuva imaging services, ja se toimii tulostaminen kioskien suurin jälleenmyynti-verkossa. Ne on Azure AD käyttöön tuhansia niiden jälleenmyynti liikekumppanien omia tunnistetietojen avulla Lataa uusin kumppanin markkinoinnin materiaalien ja Järjestä valokuvien käsittely tarvikkeiden yrityksen toimittajalta ekstranet-käyttäjille. Lisätietoja on artikkelissa [Azure AD B2B yhteistyön](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Kehityssovellusten kuluttajille:

Tarvitseeko suojatusti ja eteenpäin Julkaise online-sovellusten, kuten jälleenmyynti kaupan edessä, miljoonia kuluttajille? Azure AD tarjoaa ottaminen käyttöön sosiaalisen sekä käyttäjänimi ja salasana-kirjautuminen yrityskuvaa tukevan itsepalvelu Rekisteröi ja Omatoiminen salasanan vaihtamista koskeva kuluttajille sovelluksesi. Tämä suurentaa helppokäyttöisyys lukijoille, vähentää kehittäjille kuormituksen aikana.

**Esimerkki:** \#1 urheilua konsessio maailmanlaajuisesti tarvitsee suoraan osallistuminen sen 450 miljoonaa tuulettimista kanssa. Voit tehdä tämän ne suunniteltu Azure AD käytön käyttäjän todennusta ja profiilin tallennuspaikan mobiilisovelluksessa. Hae yksinkertaistettu rekisteröinti ja kirjaudu sisään käyttämällä yhteisöpalvelujen, kuten Facebook tai he voivat käyttää perinteinen käyttäjänimi ja salasana saumaton takaamiseksi iOS-, Android-ja Windows puhelimet. Perustuva muodostettu Azure AD-ympäristö pienentää merkittävästi mukautettua koodia, kun antamalla luvaketoiminnan mukautettu ominaispiirteet ja lievittämiseksi epävarma suojausasetukset, tietojen ilmeisesti ja skaalattavuus. Lisätietoja on artikkelissa [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Azure AD-ominaisuuksien vertailu

Jokainen tässä artikkelissa kuvatut käyttötavoista on osoitettu ominaisuuksien mukaan Azure AD. Tässä taulukossa pitäisi selkeyttää mitkä ominaisuudet ovat tärkeimpiä:

| **Harkitse tämän tuotteen...**       | [Azure AD-vuokraajan usean SaaS app](active-directory-developers-guide.md)    | [Azure AD-B2B yhteistyö](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD-B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Jos haluat säätää...** | palvelun yrityksille | Omat sovellukset kumppanien käyttöoikeus  | palvelun kuluttajille |
| **Ja olen samalla...**  | Pharma jälleenmyyjä      | Yrityksen Imaging            | Urheilu konsessio       |
| **Otetaan käyttöön apusovellus...**  | Käytäntöjen hallinta     | Toimittajan ekstranet          | Jalkapallon tuulettimista            |
| **Kohdistamisen...**        | Lääkärin toimistot        | Hyväksytyt liikekumppanien | Kaikki, joilla on sähköposti      |
| **Helppokäyttöisten milloin...**      | Asiakkaan järjestelmänvalvojan suostuu | Järjestelmänvalvoja-kutsut           | Kuluttaja Rekisteröi      |
