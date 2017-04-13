<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - monimenetelmäisen todentamisen vaatimusten määrittäminen"
    description="Ohjausobjektin ehdollisen käyttöoikeuden kanssa Azure Active Directory tarkistaa tiettyjen ehtojen, voit valita, kun käyttäjän todentamista ja ennen kuin sallit sovelluksen käyttöä. Ehtojen täyttyessä, kun käyttäjä on todennus ja sovelluksen käyttöoikeus."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Monimenetelmäisen todentamisen vaatimusten hybrid identity-ratkaisun määrittäminen

Tämä maailman liikkuvuuden käyttäville tietoja ja sovelluksia pilveen ja mistä tahansa laitteesta ja suojaaminen nämä tiedot on tullut ensiarvoisen.  Päivittäin on tietoja suojauksen sopimusrikkomusta uusi otsikko.  Vaikka ei tällaisia tietomurroilta ei takaa, monimenetelmäisen todentamisen sisältää kerroksen estämistä nämä ilmeisesti suojaus.
Aloittaa arvioimisen monimenetelmäisen todentamisen organisaatiot edellytyksistä. Eli mikä organisaation yrittää suojaamiseen.  Arvioinnissa on tärkeää tekniset vaatimukset määrittäminen ja käyttöönottaminen organisaatioissa käyttäjien monimenetelmäisen todentamisen määrittäminen.

>[AZURE.NOTE]
Jos et ole tottunut MFA ja kuvaus, on suositeltavaa lukea artikkelin [Mikä Azure Monimenetelmäisen todentamisen?](../multi-factor-authentication/multi-factor-authentication.md) ennen kuin Jatka tämän osan lukemista.

Varmista, että vastaa seuraavasti:

- Yrittää yrityksesi suojaamiseen Microsoft-sovelluksia? 
- Miten nämä sovellukset julkaistaan?
- Sisällä yrityksesi etäkäyttöpalvelimen työntekijät voivat käyttää paikallisen sovelluksia?

Jos Kyllä, minkä tyyppisiä etäkäyttöpalvelimen? Haluat myös arvioida, missä kansiossa käyttäjät, jotka käyttävät sovellukset on. Arvioinnissa on toinen tärkeä vaihe ERISNIMI monimenetelmäisen todentamisen toimintamallin. Varmista, että seuraaviin kysymyksiin:

- Jos käyttäjien siirtymällä sijoitetaan?
- Ne voidaan sijoittaa mihin tahansa?
- Yrityksen haluat muodostaa rajoitukset käyttäjän sijainnin mukaan

Kun tiedät, että näitä vaatimuksia, on tärkeää myös arvioida monimenetelmäisen todentamisen käyttäjän koskevat vaatimukset. Arvioinnissa on tärkeää, koska se määrittää vaatimukset toteuttaa monimenetelmäisen todentamisen. Varmista, että seuraaviin kysymyksiin:

- Käyttäjät ovat tuttuja monimenetelmäisen todentamisen?
- Esimerkkejä käyttää vaaditaan antamaan todennus?  
 - Jos Kyllä, aina kun tulossa ulkoiset verkostot tai käytettäessä sovelluksia tai muiden ehtojen täyttyessä?
- Käyttäjien osalta koulutus asetukset ja toteuttaa monimenetelmäisen todentamisen?
- Mitkä ovat keskeiset skenaariot, jotka yrityksesi haluaa monimenetelmäisen todentamisen niiden käyttäjien käyttöön?

Kun kyselyihin Edellinen osaat ymmärtää, jos paikalliseen monimenetelmäisen todentamisen jo käytössä. Arvioinnissa on tärkeää tekniset vaatimukset määrittäminen ja käyttöönottaminen organisaatioissa käyttäjien monimenetelmäisen todentamisen määrittäminen. Varmista, että seuraaviin kysymyksiin:

- Tarvitseeko yrityksesi suojaaminen sellaisten tili ja MFA?
- Tarvitseeko yrityksesi MFA tietyille sovelluksen yhteensopivuuden syistä käyttöön?
- Tarvitseeko yrityksesi MFA käyttöön kaikille olevalla käyttäjille sovelluksen tai vain järjestelmänvalvoja?
- Sinulla on on aina käyttöön MFA tai vain, kun käyttäjät ovat kirjautuneet yrityksen verkon ulkopuolella?


## <a name="next-steps"></a>Seuraavat vaiheet
[Hybrid tunnistetietojen käyttöönoton toimintamallin](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
