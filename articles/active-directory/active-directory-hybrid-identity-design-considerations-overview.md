<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat – yleiskatsaus | Microsoft Azure"
    description="Yleiskatsaus ja sisällön kartan Hybrid käyttäjätiedot suunnittelun huomioon otettavia seikkoja opas"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure Active Directory Hybrid käyttäjätiedot suunnittelun huomioon otettavia seikkoja

Kuluttaja-pohjaisten laitteiden ovat proliferating yrityksen maailman ja pilvipohjainen ohjelmiston nimellä--palvelun (SaaS)-sovellukset on helppo toteuttaa. Tämän vuoksi ylläpito käyttäjien sovelluksen käytön valvonta sisäinen palvelinkeskusten ja pilvi ympäristöjen välillä on vaikeaa.  Microsoftin käyttäjätietojen ratkaisuja kattavat paikallisesti että pilvipohjaisia ominaisuuksia, todennus- ja kaikkien resurssien paikasta riippumatta yksittäisen käyttäjätieto luodaan. Olemme Soita Hybrid-tunnistetiedot. On eri rakenne ja asetuksia hybrid jäsenyys käyttäen Microsoft-ratkaisuja ja joissakin tapauksissa voi olla vaikea määrittää käytetystä parhaat täyttävät organisaation tarpeiden mukaan. Hybrid käyttäjätiedot suunnittelun huomioon otettavia seikkoja oppaan avulla voit ymmärtää, kuinka suunnitteluun hybrid identity-ratkaisun, joka sopii parhaiten liiketoiminnan ja tekniikka on organisaatiollesi.  Tämän oppaan tiedot vaiheet sekä tehtävät, voit seurata avulla voit suunnitella hybrid identity-ratkaisun, joka vastaa organisaation yksilöllisiä sarjaa. Koko vaiheet ja tehtävät-opas esittelee asianmukaisia tekniikoita ja ominaisuus-asetukset saatavilla organisaatioita toimintojen kokoukset ja palvelun laatua (kuten käytettävyyden, skaalattavuus, suorituskyvyn, hallinta ja suojaus) tason tarpeita. Hybrid käyttäjätiedot suunnittelun huomioon otettavia seikkoja opas tavoitteet ovat erityisesti seuraaviin kysymyksiin: 

- Mitä kysymyksiä tarvitsen Kysy ja vastaus levyasemassa hybrid identity-kohtaisia rakenteeltaan tekniikka tai ongelma toimialueen, parhaiten vastaa Omat vaatimuksia?
- Mitä toimintoja järjestyksen suunnitteluun hybrid identity-ratkaisun tekniikka tai ongelma toimialueen olisi suorittaminen? 
- Hybrid tunnistetietojen tekniikka- ja määritystietoja asetuksista on saatavilla ongelmatilanteiden Omat ehtojen mukaan? Mitkä ovat valinnat välillä asetuksia niin, että voit valita paras vaihtoehto omaa liiketoimintaani varten?


## <a name="who-is-this-guide-intended-for"></a>Kuka on tämän oppaan tarkoitettu?
 CIO, CITO, johtava tunnistetietojen Architects, Enterprise Architects ja IT-Architects vastuussa suunnitteleminen hybrid identity-ratkaisun, Normaali tai suuri organisaatioissa.

## <a name="how-can-this-guide-help-you"></a>Miten tässä oppaassa voi auttaa sinua? 
Tämän oppaan avulla voit suunnitella hybrid tunnistetietojen ratkaisu, joka on voi integroida nykyisen paikallisen tunnistetietojen ratkaisu pilvipohjainen käyttäjätietojen hallintajärjestelmän, kuinka. Kuvan seuraavassa esimerkissä hybrid identity-ratkaisu, jonka avulla IT-järjestelmänvalvojat voivat hallita integroiminen niiden nykyinen Windows Server Active Directory-ratkaisun sijaitsevat paikalliseen Microsoft Azure Active Directory, jotta käyttäjät voivat käyttää Single Sign-On (SSO) sovellusten sijaitsevat pilvessä ja paikallisen.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


Edellä olevassa kuvassa on esimerkki hybrid identity-ratkaisun, joka on hyödyntäminen pilvipalveluihin, jos haluat liittää paikallisen ominaisuuksia haluat tarjota yksittäisen käyttökokemuksen käyttäjän todentaminen-prosessi ja helpottaa IT näiden resurssien hallinta. Tämä voi olla hyvin käytetty vaihtoehto jokaisen organisaation hybrid käyttäjätiedot suunnittelun on todennäköisesti on erilainen kuin Esimerkki esitelty kuva 1 vuoksi erilaisia vaatimuksia. Tässä oppaassa on vaiheet sekä tehtävät, voit seurata hybrid identity-ratkaisun, joka vastaa organisaation yksilöllisen suunnitteluun sarjaa. Koko seuraavat vaiheet ja tehtävät-oppaan näkyy asianmukaisia tekniikoita ja ominaisuus vaihtoehdoista sinulle vastaa toimintojen ja palvelun laatua tason tarpeita organisaatiollesi.

**Perustiedot**: joissakin Windows Server, Active Directory-toimialueen palveluista ja Azure Active Directory kokemusta on. Tässä asiakirjassa on oletetaan hakua siitä, miten näitä ratkaisuja vastaa business tarpeitasi yhteiskäyttöä tai integroitu ratkaisu.

## <a name="design-considerations-overview"></a>Rakenteen huomioon otettavia seikkoja yleiskatsaus
Tässä tiedostossa on tietyt vaiheet ja tehtävien vaiheittaisten ohjeiden avulla voit suunnitella hybrid identity-ratkaisun, joka vastaa parhaiten tarpeitasi. Järjestetty esitetään vaiheet. Myöhemmissä vaiheissa kerrotaan tyyliseikat saattaa edellyttää, että voit muuttaa päätöksiä tekemäsi aiemmissa vaiheissa kuitenkin vuoksi ristiriitaiset ulkoasu-valinnat. Jokaisen yritetään varoituksen rakenne ristiriitoja koko asiakirjassa. 

Olet päässyt, parhaiten vastaa tarpeitasi vasta ohjeiden mukaisesti monta kertaa tahansa toteuttamaan kaikki asiakirjassa huomioitavista läpikäyminen rakenne. 

| Hybrid tunnistetietojen vaihe                                             | Aihe-luettelo                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Käyttäjätietojen vaatimusten määrittäminen                                   | [Määrittää liiketoimintatarpeiden](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Kansion synkronoinnin vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Monimenetelmäisen todentamisen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Hybrid tunnistetietojen käyttöönoton toimintamallin](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Suunnitelman vahva identity-ratkaisussa tietojen suojauksen parantaminen | [Tietojen suojauksen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Sisällön hallinta vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Access-ohjausobjektin vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Tapauksen vastauksen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Tietojen suojauksen toimintamallin](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Hybrid tunnistetietojen elinkaari suunnitteleminen                                | [Määrittää hybrid käyttäjätietojen hallintatehtävät](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Synkronoinnin hallinta](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Hybrid käyttäjätietojen hallinta käyttöönoton määrittäminen](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Lataa opas
Voit ladata pdf-version Hybrid tunnistetietojen Tyyliseikat oppaan [Technet-valikoimassa](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
