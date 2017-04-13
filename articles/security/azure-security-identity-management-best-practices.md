<properties
   pageTitle="Azure käyttäjätietojen hallinta ja käyttöoikeudet määrittää suojauksen parhaiden käytäntöjen | Microsoft Azure"
   description="Tässä artikkelissa on joukko parhaat käytännöt jäsenyyksien hallinta ja access-ohjausobjektin avulla luotu Azure ominaisuuksia."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="yurid"/>

# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure käyttäjätietojen hallinta ja käyttöoikeudet määrittää suojauksen parhaat käytännöt

Monet harkitse tunnistetiedot on uusi reunan kerros, tietoturvasyistä ottaen perinteinen verkon keskitettyä näkökulmasta roolin päälle. Tämä kehitys ensisijainen pivot suojauksen huomiota ja sijoitukset peräisin verkon perimeters on tullut yhä porous ja ympäröivän kyseisen linnaa ei voi olla siirretyt kerran ennen hajotus [BYOD](http://aka.ms/byodcg) laitteet ja cloud sovelluksia tehokkaasti.

Tässä artikkelissa käsittelemme kokoelma Azure jäsenyyksien hallinta ja Accessin ohjausobjektin suojauksen parhaista käytännöistä. Näiden vinkkien johdettuja Microsoftin [Azure AD](../active-directory/active-directory-whatis.md) -käyttökokemuksen ja asiakkaiden, kuten itse kokemukset.

Parhaita käytäntöjä, kerrotaan:

- Mikä on paras käytäntö
- Miksi haluat ottaa käyttöön kyseisen parhaat käytännöt
- Mitä voi olla tulos, jos et ota parhaita käytäntöjä
- Paras käytäntö mahdollista vaihtoehtoja
- Miten voit lukea käyttöön parhaat käytännöt

Tässä artikkelissa on kirjoitettu tämän Azure jäsenyyksien hallinta ja parhaita käytäntöjä artikkelissa perustuu yksimielisyyttä mielestä ja Azure ympäristö ominaisuudet ja määrittää ominaisuus milloin olemassa käyttöoikeudet. Lausuntoja ja -tekniikoiden muuttuvat ajan kuluessa ja sisältö päivitetään säännöllisesti muutosten mukaisiksi.

Azure käyttäjätietojen hallinta ja käyttöoikeudet ohjausobjektin suojauksen parhaiden käytäntöjen tässä artikkelissa kuvatut ovat seuraavat:

- Keskittää jäsenyyksien hallinta
- Ottaa käyttöön kertakirjautumisen (SSO)
- Ottaa käyttöön salasanojen hallinta
- Pakottaa monimenetelmäisen todentamisen (MFA) käyttäjille
- Käytä Roolipohjainen käyttöoikeuksien valvonta (RBAC)
- Ohjausobjektin sijainnit missä resurssien luodaan Resurssinhallinnan avulla
- Oppaan kehittäjät voivat hyödyntää SaaS-sovellusten ominaisuuksia
- Epäilyttävien toimintojen aktiivisesti valvonta

## <a name="centralize-your-identity-management"></a>Keskittää jäsenyyksien hallinta

Yksi tärkeä askel suojaaminen henkilöllisyytesi on varmistaa, että se voi hallita sijainnista yksittäisen koskeva missä tämä tili on luotu. Kun yritysten IT-organisaatioille useimpia on hänen ensisijaisen tilin kansion paikallisen, cloud yhdistelmäympäristöä ole nousu ja on tärkeää ymmärtää integroida paikallisen ja cloud kansioiden ja tarjota saumattomasti käyttökokemuksen peruskäyttäjän.

Suoritettava [hybrid identity](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) -artikkelista Suosittelemme kahdella eri tavalla:

- Synkronointi pilveen hakemistossa käyttämällä Azure AD Connect paikallisesta hakemistosta
- Järjestäjäorganisaatiota henkilöllisyytesi paikallisen [Active Directory Federation Services](https://msdn.microsoft.com/library/bb897402.aspx) (AD FS) käyttämällä cloud hakemistossa kanssa

Organisaatioissa, joissa ei onnistu paikallisen jäsenyys integroida cloud jäsenyys kohdata parantavat hallinnan tarvetta hallintaa tilit, mikä suurentaa virheet ja suojauksen ilmeisesti todennäköisyys.

Lisätietoja Azure AD-synkronointi Lue artikkeli, [integroiminen paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Ottaa käyttöön kertakirjautumisen (SSO)

Jos sinulla on useita kansioita, voit hallita, tämä on järjestelmänvalvojan ongelma, ei vain IT, mutta myös käyttäjille, jotka on useita salasanojen varten. Käyttämällä [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) tarjoaa mahdollisuutta käyttää samoja tunnistetietoja Kirjaudu sisään ja käsitellä resurssit, joiden ne, riippumatta resurssi on sijoitettu paikallisen tai pilveen käyttäjille.

Käytä tätä, jotta käyttäjät voivat käyttää niiden [SaaS sovellusten](../active-directory/active-directory-appssoaccess-whatis.md) perusteella niiden organisaatiotili Azure AD SSO. Tämä on käytettävissä, ei ainoastaan Microsoft SaaS sovellukset, mutta muista sovelluksista, kuten [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) ja [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Sovelluksen voi määrittää käyttämään Azure AD [SAML-pohjaisen tunnistetietojen](../active-directory/fundamentals-identity.md) toimittaja. Azure AD kuin suojaus-ohjausobjekti ei myöntäjän tunnuksen, he voivat kirjautua sovellusta paitsi, jos ne on myönnetty Azure AD-käytön. Voit myöntää access suoraan tai ryhmän kautta, että ne ovat jäsen.

> [AZURE.NOTE] käyttää SSO päätös vaikuttaa miten paikallisesta hakemistosta integroida cloud hakemistossa. SSO halutessasi sinun on käytettävä federaatio, koska hakemistosynkronoinnin antaa vain [saman Sign-ominaisuus](../active-directory/active-directory-aadconnect.md).

Organisaatioissa, joissa Älä pakota niiden käyttäjien ja sovellusten SSO niitä julkaista Lisää skenaarioita, joihin käyttäjiä on useita salasanoja, mikä suurentaa suoraan käyttäjien salasanat uudelleen tai heikko salasanojen todennäköisyys.

Voit lukea lisää Azure AD SSO lukemalla artikkelin [AD FS hallinta ja mukauttaminen Azure AD Connect kanssa](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Ottaa käyttöön salasanojen hallinta

Skenaarioita, joissa on useita omistajien tai haluat antaa käyttäjien [Oman salasanan](../active-directory/active-directory-passwords-update-your-own-password.md)on tärkeää, että käytät tarvittavat suojauskäytäntöjä estää väärinkäyttö. Azure-tietokannassa voit hyödyntää käyttäjien Omatoiminen salasanan palauttaminen ominaisuuksien ja mukauttaa business vaatimukset täyttävän suojausasetukset.

On erittäin tärkeää saada palautetta näiltä käyttäjiltä ja opi kokemuksensa, kun käyttäjä yrittää suorittaa nämä vaiheet. Nämä kokemukset mukaan laadittava suunnitelma pienentämään mahdolliset ongelmat, joita voi ilmetä suuremman ryhmän käyttöönoton aikana. On myös suositeltavaa, että käytät [salasanan palauttaminen rekisteröinti Tehtäväraportti](../active-directory/active-directory-passwords-get-insights.md) käyttäjät, jotka rekisteröityvät seurannassa.

Organisaatioissa, joissa haluat välttää salasanan muuttaminen tuotetukea koskevia puheluita, mutta antaa käyttäjien salasanojen omia on helpompi suurempi puhelun aseman tukipalvelu salasanaongelmien vuoksi. Organisaatioissa, joissa on useita alihallinnat, jotka on välttämätöntä toteuttaa tällaista ominaisuuksien että käyttäjät voivat suorittaa suojauksen rajojen, joka on luotu suojauskäytäntö salasanan.

Voit lukea lisää salasanan lukemalla artikkelin [käyttöönotto salasanojen hallinta ja koulutus käyttäjät voivat käyttää sitä](../active-directory/active-directory-passwords-best-practices.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Pakottaa monimenetelmäisen todentamisen (MFA) käyttäjille

Organisaatioissa, joissa on oltava yleisesti käytettyjen, esimerkiksi [PCI DSS versiota 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32)standardien kanssa monimenetelmäisen todentamisen on valittava on ominaisuuksien varten todentaa käyttäjät. Lisäksi, että yleisesti käytettyjen standardien kanssa, pakotetut MFA todentaa käyttäjät voivat myös auttaa organisaatioita pienentämään tunnistetiedon varkaus muodossa, kuten [kerralla--Hash (PtH)](http://aka.ms/PtHPaper).

Ottamalla Azure MFA käyttäjiä lisätään toisen layer Security käyttäjän kirjautumiset ja tapahtumia. Tässä tapauksessa tapahtuman voi käyttää asiakirjan, joka sijaitsee tiedostopalvelimessa tai SharePoint-Online. Azure MFA avulla voit pienentää todennäköisyys, että vaarantuneen tunnistetieto on organisaation tietojen käytön IT.

Esimerkki: Pakota Azure MFA käyttäjien ja määrittää sen käyttämään tekstiviesti tai puhelu vahvistus. Jos käyttäjän tunnistetiedot ovat käsiin, hän ei voi käyttää mitä tahansa resursseja, koska hän ei voi käyttää käyttäjän puhelimeen. Organisaatioissa, joissa ei lisätä ylimääräisen kerrokset tunnistetiedot on helpompi tunnistetiedon varkaus hyökätä, mikä voi aiheuttaa tietojen haavoittuvan.

Organisaatioissa, joissa haluat säilyttää koko käyttöoikeuksien hallinta paikallisen yksi vaihtoehto on käyttää [Azure multi-factor Authentication Server](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), kutsutaan myös MFA paikallisen. Käyttämällä tätä menetelmää voit edelleen saa oikeuden pakottaa monimenetelmäisen todentamisen ja säilyttää MFA palvelimen paikallisen.

Lisätietoja Azure MFA Lue artikkeli, [Azure Monimenetelmäisen todentamisen pilveen käytön aloittaminen](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Käytä Roolipohjainen käyttöoikeuksien valvonta (RBAC)

[Hyvä tietää](https://en.wikipedia.org/wiki/Need_to_know) ja [käyttöoikeuksien myöntäminen](https://en.wikipedia.org/wiki/Principle_of_least_privilege) suojauksen periaatteiden perusteella käytön rajoittaminen on organisaatioissa, joissa haluat käyttää tietojen käytön suojauskäytäntöjä. Azure Roolipohjainen käytön hallinta (RBAC) voidaan määrittää käyttäjät, ryhmät ja tiettyjen käyttöalue sovellusten käyttöoikeudet. Roolien osoitus laajuus voi olla tilauksen, resurssiryhmä tai yksittäinen resurssi.

Määritä oikeuksia käyttäjille Azure [RBAC sisäisten](../active-directory/role-based-access-built-in-roles.md) roolien avulla voidaan hyödyntää. Kannattaa käyttää *Tallennustilan tilin avustaja* cloud operaattoreita, jotka on hallittava tallennustilan asiakkaat ja *Perinteinen tallennustilan tilin osallistujan* rooli perinteinen tallennustilan tilien hallinta. Harkitse cloud operaattoreita, joka on hallittava VMs ja tallennustilaa tilin lisääminen *Virtuaalikoneen osallistujan* rooli.

Organisaatioissa, joissa Älä pakota tietojen käyttöoikeuksien valvonta mukaan hyödyntäminen ominaisuudet, kuten RBAC saattaa antaa enemmän kuin on tarpeen niiden käyttäjien oikeuksia. Tämä voi aiheuttaa tietojen haavoittuvan mukaan käyttäjät tietyntyyppisiä tietoja (esimerkiksi suuren business vaikutus), joihin heillä ei kannata ensiksi on pääsy.

Voit lukea lisää Azure RBAC lukemalla artikkelin [Azure Role-Based käyttöoikeuksien hallinta](../active-directory/role-based-access-control-configure.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Ohjausobjektin sijainnit missä resurssien luodaan Resurssinhallinnan avulla

Cloud operaattoreita, mutta estää niiden katkaisemisen käytäntöjä, joita tarvitaan organisaation resurssien tehtävien suorittamiseen ottaminen käyttöön on erittäin tärkeää. Organisaatiot, jotka haluat määrittää, missä resurssit luodaan sijainnit olisi kiintolevyn koodin seuraavista sijainneista.

Tätä organisaatiot voivat luoda suojauskäytäntöjä, joissa on määritteitä, jotka kuvaavat toiminnot tai resurssit, jotka on erikseen estetty. Voit määrittää kyseiset käytännön määritykset haluamasi alueessa, kuten tilaus, resurssiryhmä tai yksittäinen resurssi.

> [AZURE.NOTE] Tämä ei ole sama kuin RBAC, se hyödyntää todella RBAC tarkistamiseen käyttäjät, joilla on oikeus luoda resurssit.

Voit luoda mukautettuja käytäntöjä myös skenaariot, jossa organisaation haluaa antaa toiminnot vain, jos tarvittavat kustannuspaikka liitetään; [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) hyödyntää Muussa tapauksessa ne hylätä pyynnön.

Organisaatioissa, joissa on ei hallinta miten resurssit luodaan on helpompi käyttäjät, joilla voi käyttää palvelua väärin luomalla lisää resursseja kuin he tarvitsevat. Resurssin luontiprosessi Hardening on tärkeä vaihe suojaamiseen usean vuokraajan tilanne.

Voit lukea lisää luomisesta käytännöt Azure resurssien hallinnan lukemalla artikkelin [Käyttöä koskevasta käytännöstä resurssien hallintaa ja hallita](../resource-manager-policy.md).

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>Oppaan kehittäjät voivat hyödyntää SaaS-sovellusten ominaisuuksia

Käyttäjätietojen hyödyntää monissa tilanteissa, kun käyttäjät käyttävät [SaaS sovellukset](https://azure.microsoft.com/marketplace/active-directory/all/) , jotka voidaan integroida paikallisen tai cloud hakemisto. Keskustelupalstoja on suositeltavaa, että kehittäjät avulla voit tehdä nämä sovellukset, kuten [Microsoft Security Development Lifecycle SDL](https://www.microsoft.com/sdl/default.aspx)turvallinen menetelmä. Azure AD yksinkertaistaa todennus kehittäjille tarjoamalla tunnistetietojen palveluna yleisesti protokollia, kuten [OAuth 2.0](http://oauth.net/2/) ja [OpenID yhteyden](http://openid.net/connect/)tukeen sekä Avaa lähde-kirjastoissa eri ympäristöissä.

Varmista, että voit rekisteröidä sovellus, jossa outsources Azure AD-todennus, tämä on pakollinen ohjeiden mukaisesti. Syy takana Tämä johtuu Azure AD tarvitsee järjestämiseen sovelluksen tietoliikenteen, kun käsittely kertakirjautumisportaaliin (SSO) tai vaihtaa tunnukset. Käyttäjän istunto päättyy, kun Azure AD myöntämä tunnuksen käyttöaika päättyy. Arvioi aina, jos sovellus kannattaa käyttää tällä kertaa tai voit pienentää tällä hetkellä. Vähentää elinkaaren voi toimia suojauksen mitta, jossa pakottaa käyttäjät voivat kirjautua ulos perustuvat ajan, valitse.

Organisaatioissa, joissa Älä pakota tunnistetietojen ohjausobjektin access-sovelluksia ja niiden kehittäjät ei apuviivan suojatusti integroida niiden käyttäjätietojen hallintajärjestelmän sovellusten käyttämisestä voi olla johtuu usein tunnistetietojen varkaus muodossa, kuten [Avaa Web Application Security projektin (OWASP) Ylin 10 kuvattu heikko todennus ja istunnon hallinta](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

Voit lukea lisää SaaS sovellusten käyttöoikeuksien tilanteita, joissa tietoja lukemalla [Todennus tilanteita, joissa Azure AD](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Epäilyttävien toimintojen aktiivisesti valvonta

[Verizon 2016 tietojen sopimusrikkomusta raportin](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/)mukaan tunnistetiedon haavoittuvan on edelleen nousu ja jatkossa jokin voitollisimpien yritysten cyber siihen. Tästä syystä on tärkeää on aktiivinen identity-valvonta järjestelmän paikkaa, jossa voit nopeasti tunnistaa epäilyttävistä toiminta-toimintojen ja lisää hälytys. Azure AD on kaksi tärkeimmistä ominaisuuksista, joita voi auttaa organisaatioita valvoa henkilöllisyyttä: Azure AD Premium [poikkeavuuksista raportteja](../active-directory/active-directory-view-access-usage-reports.md) ja Azure AD- [tunnistetiedot](../active-directory/active-directory-identityprotection.md) -ominaisuutta.

Varmista, että poikkeavuuksista raporttien avulla voit selvittää yrittää kirjautua sisään [ilman, että jäljittää](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md) [palvelinten](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) kalastelu vastaan tietyn tilin yrittää kirjautua sisään monista eri paikoista, kirjaudu [tartunnan laitteet](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md) ja epäilyttävistä IP-osoitteet. Ota huomioon, että ne ovat raportit. Toisin sanoen tarvitset prosessit ja toimintojen paikoillaan IT-järjestelmänvalvojat voivat suorittaa raportteja päivittäin tai pyydettäessä (yleensä-vastauksen tapaus-skenaario).

Sen sijaan Azure AD tunnistetietojen suojaus on aktiivinen valvonta järjestelmän ja se Merkitse nykyisen riskien omassa koontinäytössä. Lisäksi, että saat myös päivittäinen yhteenveto ilmoitukset sähköpostitse. On suositeltavaa Säädä riskin taso business tarpeen mukaan. Riskin riskin tapahtuman on riski tapahtuman vakavuus tieto (suuri, Normaali tai pieni). Priorisoida ne on otettava riskien vähentämistä organisaation toiminnot tunnistetiedot-käyttäjät voivat käyttää riskin taso.

Organisaatioissa, joissa ei aktiivisesti Valvo niiden identity-järjestelmiä ovat riski, käyttäjän tunnistetietoja käsiin. Tiedot, jotka epäilyttävistä toimintoja aloittamasi ilman Aseta käyttämällä näitä tunnistetietoja-organisaatiot voi rajoittaa tällaista uhkien.
Voit lukea lisää Azure tunnistetiedot lukemalla [Azure Active Directory tunnistetietojen suojaus](../active-directory/active-directory-identityprotection.md).
