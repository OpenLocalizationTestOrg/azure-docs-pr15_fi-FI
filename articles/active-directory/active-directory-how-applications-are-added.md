<properties
   pageTitle="Miten sovellusten lisätään Azure Active Directory."
   description="Tässä artikkelissa kuvataan, miten sovellusten lisätään Azure Active Directory-esiintymä."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Miten ja miksi sovellukset lisätään Azure AD

Alun perin epäselviin toimea tarkasteltaessa sovellusten luettelo Azure Active Directory-esiintymässä on tietoja mistä sovellukset ovat peräisin ja miksi ne ovat olemassa.  Tässä artikkelissa antaa korkean tason yleiskatsaus siitä, miten sovellusten esitetään hakemiston ja antaa sinulle kontekstissa, joka auttaa sinua ymmärtää, kuinka sovelluksen oli on hakemistossa.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Mitä palveluja Azure AD antaa sovelluksia?

Sovellusten lisätään Azure AD hyödyntää vähintään yksi seuraavista se tarjoaa services.  Näistä palveluista ovat seuraavat:

* Sovelluksen todennus- ja
* Käyttäjien todentamiseen ja todennus
* Kertakirjautuminen (SSO) federation tai salasanan avulla
* Käyttäjän valmistelu ja synkronointi
* Roolipohjainen käyttöoikeuksien valvonta; Käytä hakemistoon sovelluksen roolit suorittamiseen roolit perusteella luvan tarkistaa-sovelluksessa.
* oAuth luvan services (käytetään Office 365: ssä ja muiden Microsoft-sovellusten sallivat ohjelmointirajapinnan/resurssien käyttöoikeuksien.)
* Sovellusten julkaiseminen ja välityspalvelimen; Julkaise sovelluksen yksityisverkon Internetiin

## <a name="how-are-applications-represented-in-the-directory"></a>Miten sovellusten esitetään hakemiston?

Sovellusten esitetään 2-objektien käyttäminen Azure AD: sovellusobjektin ja service Pääobjektin.  On vain yksi sovelluksen objekti rekisteröity "talojen" tai "omistaja" tai "julkaisun" directory ja vähintään yhden palvelun pääasiallista objekteja, jotka sovelluksen kunkin hakemiston, jossa se toimii.  

Sovellusobjektin kuvataan Azure AD sovellus (usean vuokraajan palvelu) ja voi olla jokin seuraavista palveluista: (*Huomautus*: Tämä ei ole kattava luettelo.)

* Nimi, Logo ja Publisherissa
* Tietoja (symmetrisen ja/tai julkiseen näppäimet todennetaan sovellus)
* API riippuvuudet (oAuth)
* API/resurssien/käyttöalueen julkaistu (oAuth)
* Sovelluksen roolit (RBAC)
* SSO metatiedot- ja määritystietoja (SSO)
* Käyttäjän metatiedot- ja valmistelu
* Välityspalvelimen metatietojen ja määrittäminen

Palvelun lyhennys on tietue kunkin hakemiston, jossa sovellus toimii myös sen pääkansion sovelluksen.  Palvelu: lyhennyksen.

* Viittaa takaisin sovellusobjektin kautta app id-ominaisuus
* Tietueiden paikallisen käyttäjän ja ryhmän app rooli määritykset
* Tietueiden paikallisen käyttäjän ja järjestelmänvalvojan oikeudet myönnetään-sovellukseen
    * Esimerkki: tiettyjen käyttäjien sähköpostin sovelluksen käyttöoikeuksia
* Tietueiden Paikalliset käytännöt lukien ehdollisen käyttöoikeuden ehdot
* Tietueiden paikallisen vaihtoehtoinen paikalliset asetukset sovelluksen
    * Vaateita, jotka muunnos säännöt
    * Määritteen yhdistämismääritysten (käyttäjän valmistelu)
    * Vuokraaja tietyn sovelluksen roolit (Jos sovellus tukee mukautettuja rooleja)
    * Nimi ja Logo

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Esimerkkejä joistain muista sovelluksen objektit ja palvelun ansaitun kansioiden välillä

![Kaavio, jossa kuvataan, miten sovelluksen objektit ja palvelun ansaitun aiemmin Azure AD-esiintymät.][apps_service_principals_directory]

Kuten näet yllä olevassa kaaviossa.  Microsoft ylläpitää kaksi kansioiden sisäisesti (vasemmalla) yhteydessä käyttämien ohjelmistotuotteiden valinnaiseksi.

* Yksi Microsoft Apps (Microsoft services-hakemisto)
* Toinen valmiiksi integroitu 3 osapuolten sovelluksissa (sovelluksen valikoima hakemisto)

Sovelluksen julkaisijat/toimittajia, jotka Azure AD integroida tarvitaan julkaisun hakemisto.  (Jotkin SAAS kansio).

Sovellukset, jotka olet lisännyt itse ovat seuraavat:

* Asiakas on kehittänyt Apps (integroitu AAD)
* Sovellukset, jotka on muodostettu, single-sign-on
* Sovellusten julkaista Azure AD-sovelluksen välityspalvelimen.

### <a name="a-couple-of-notes-and-exceptions"></a>Muistiinpanojen ja poikkeukset pari

* Kaikki service ansaitun osoittamalla takaisin sovelluksen objektit.  HUH? Kun Azure AD on alun perin luotu sovellusten palveluja on paljon rajoitettu ja palvelun lyhennys on riittävä app jäsenyyden vahvistamiseksi.  Lainan alkuperäisen palvelu ei tarkempi muodon Windows Server Active Directory-palvelutilin.  Tästä syystä on edelleen voi luoda käyttämällä PowerShellin Azure AD ilman, että luot sovellusobjektin palvelun ansaitun.  Graph-Ohjelmointirajapinnan edellyttää sovelluksen objektin ennen kuin luot palvelu pääasiallista.
* Kaikkia edellä kuvattuja tietoja ei ole tällä hetkellä näkyvät ohjelmallisesti.  Seuraavassa ovat käytettävissä vain käyttöliittymässä:
    * Vaateita, jotka muunnos säännöt
    * Määritteen yhdistämismääritysten (käyttäjän valmistelu)
* Saat lisätietoja yksityiskohtaiset tiedot palvelun lyhennyksen ja sovelluksen objektit tutustumaan Azure AD Graph REST API-oppaat.  *Vihje*: Azure AD Graph API dokumentaatio on lähimpänä Huomaa, että rakenteeseen viittauksen Azure AD, joka on tällä hetkellä käytettävissä.  
    * [Sovelluksen](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Palvelun lyhennyksen.](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Miten sovellusten lisätään Azure AD-esiintymän?
Sovelluksen voidaan lisätä Azure AD monella tavalla:

* Lisää sovellus [Azure Active Directory App valikoima](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Kirjaudu ylös/kyselyjä 3rd osapuolen sovelluksen integroitu Azure Active Directory (esimerkiksi: [Smartsheet](https://app.smartsheet.com/b/home) tai [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Ylös/käyttäjät rekisteröitymisen yhteydessä pyydetään antamaan muiden oikeuksien käyttöoikeus-sovellukseen profiiliinsa.  Ensimmäisen henkilön suostumus aiheuttaa palvelun lyhennys edustava sovelluksen haluat lisätä hakemiston.
* Kirjaudu ylös/tuominen Microsoft online services kuten [Office 365: ssä](http://products.office.com/)
    * Kun tilaat Office 365: ssä tai Aloita yhden tai usean palvelun ansaitun luodaan kansio, joka edustaa eri palvelut, joita käytetään aikana kaikkia toimintoja Office 365: een liitetty kokeiluversion.
    * Jotkin Office 365-palveluja, kuten SharePoint Luo palvelun ansaitun jatkuvaa säännöllisesti, jotta tietosuojaa, mukaan lukien työnkulut osien välillä.
* Lisää sovellus kehität Azure hallinta-portaalin Katso: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Lisäämisestä sovelluksen kehität Visual Studiossa on seuraavissa artikkeleissa:
    * [ASP.Net-todennustavat](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Yhdistetyt palvelut](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Lisää sovellus [Välityspalvelimen Azure AD-sovelluksen](https://msdn.microsoft.com/library/azure/dn768219.aspx) avulla
* Yhdistä kertakirjautuminen käyttämällä SAML tai salasana SSO apusovellus
* Monista muista mukaan lukien Azure eri developer kokemukset ja tai-Ohjelmointirajapinnan Explorerissa ilmenee yli developer keskikohdan mukaan

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Kenellä on oikeus lisätä Azure AD-esiintymässä sovelluksia?

Ainoastaan Yleiset järjestelmänvalvojat voivat:

* Lisää sovelluksia Azure AD-sovelluksen valikoimasta (valmiiksi integroidut 3 osapuolten sovellukset)
* Sovelluksen Azure AD-sovelluksen välityspalvelimen julkaiseminen

Kaikkien käyttäjien hakemistossa mainitut oikeudet lisää sovelluksia, jotka he kehittävät ja Ota vastaanottaja huomioon päälle sovellukset ne Jaa ja antaa access niiden organisaation tiedot.  *Muista käyttäjän kirjautuminen ylös/sovelluksen ja käyttöoikeuksien myöntämistä saattaa johtaa luomisen service-lyhennyksen.*

Tämän voi aluksi ääntä koskevat, mutta pitää mielessä seuraavat:

* Sovellukset ovat voineet hyödyntää Windows Server Active Directory on rekisteröity ja tallentanut hakemiston sovelluksen tarvitsematta monta vuotta käyttäjän todennusta varten.  Nyt organisaation on parannettu näkyvyyden täsmälleen montako sovellukset käyttävät hakemistosta ja tehtävät for.
* Sovelluksen julkaiseminen-/ rekisteröitymisen perustuva järjestelmänvalvoja ei tarvita.  Active Directory Federation Services on todennäköistä, että voit lisätä sovelluksen varmenteen käyttäjän osapuolen puolesta kehittäjät oli järjestelmänvalvoja.  Kehittäjät voivat nyt itsepalvelu.
* Kirjautuminen/ylöspäin niiden organisaation tilin käyttäminen business tarkoituksiin sovellusten käyttäjien on hyvä asian.  Jos ne myöhemmin jättää organisaation ne menettävät käyttöoikeutensa tilissä he käyttäisivät-sovelluksessa.
* Ottaa kirjaa siitä, mitkä tiedot on jaettu, johon sovellus on hyvä asian.  Tiedot on enemmän kuin koskaan kyseisen ja ottaa Tyhjennä tietue, joka jakaa tietoja sovellukset on hyötyä.
* Sovellukset, jotka käyttävät Azure AD oAuth päättää täsmälleen tarvittavat käyttöoikeudet, jotka käyttäjät voivat myöntää sovellukset ja käyttöoikeudet edellyttävät Hyväksy järjestelmänvalvoja.  Se lähetetään selvää, että vain järjestelmänvalvojat voivat suostumus laajempia luetteloita ja lisää merkittäviä käyttöoikeudet.
* Käyttäjien lisääminen ja salliminen tietoihinsa sovellukset ovat valvottavien tapahtumien, voit selvittää, kuinka sovellus on lisätty hakemiston Azure Management-portaalin valvonta-raportteja voi tarkastella.

**Huomautus:** *Microsoft itse toiminut oletusarvo-määritysten käyttäminen nyt monta kuukautta.*

Kaikki kyseisen said ei estä käyttäjiä hakemistossa sovellusten lisäämisestä ja -toteuttavien Ota vastaanottaja huomioon kautta mitä tietoja niiden jakaminen-sovellusten kanssa muokkaamalla Directory kokoonpanon Azure hallinta-portaalissa.  Seuraavat määritykset niitä voi käyttää oman kansion "Määritä-välilehdessä Azure hallinta-portaalissa.

![Näyttökuva Käyttöliittymän Integroitu sovellus asetusten määrittäminen][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisää siitä, miten voit lisätä Azure AD-sovellusten ja palveluiden-sovellusten määrittäminen.

* Kehittäjät: [Katso, miten voit integroida hakemus AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Kehittäjät: [Tarkista otoksen koodin sovellusten integroitu Azure Active Directory-Github](https://github.com/AzureADSamples)
* Kehittäjille ja IT-ammattilaisille: [Tarkista REST API ohjeissa Azure Active Directory Graph-Ohjelmointirajapinta](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* IT-ammattilaisille: [Opi käyttämään Azure Active Directory valmiiksi integroitujen sovellusten App valikoimasta](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* IT-ammattilaisille: [Etsi opetusohjelmat tietyn valmiiksi integroitujen sovellusten määrittäminen](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* IT-ammattilaisille: [opit julkaisemaan sovelluksen Azure Active Directory sovelluksen välityspalvelimen](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Katso myös

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
