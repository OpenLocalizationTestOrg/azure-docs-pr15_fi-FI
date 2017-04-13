
<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - access ohjausobjektin vaatimusten määrittäminen | Microsoft Azure"
    description="Kattaa pylväiden käyttäjätiedot ja tunnistaminen access yhdistelmäympäristössä käyttäjät resursseja koskevat vaatimukset."
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

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Access-ohjausobjektin vaatimusten hybrid identity-ratkaisun määrittäminen
Organisaation suunniteltaessa hybrid tunnistetietojen ratkaisun ne myös Tarkastele Accessin resurssit, jotka he ovat suunnittelu, jotta se käytettävissä käyttäjille mahdollisuuden avulla. Tietojen käyttö toimintojen kaikki neljä pylväiden perusteella, jotka ovat:

- Hallinta
- Todennus
- Todennus
- Valvonta

Osien jälkeen tulevaa kattavat todennus- ja lisää tiedot, hallinta ja valvonta on osa hybrid tunnistetietojen elinkaari. Lisätietoja näiden ominaisuuksien [määrittäminen hybrid tunnistetietojen hallintatehtäviä](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) lukeminen

>[AZURE.NOTE]
Lukea lisätietoja yksitellen näiden pylväiden [Neljä pylväiden, Identity - jäsenyyksien hallinta Hybrid se ikä](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) .

## <a name="authentication-and-authorization"></a>Todennus- ja
On eri tilanteita, joissa todennus- ja -tilanteista on erityiset vaatimukset, jotka hybrid identity-ratkaisun, joka yrityksen siirtymällä hyväksyy on täytettävä. Käsittelevissä tilanteissa, joissa Business-Business (B2B) viestintä lisätä ylimääräisen todennus IT-järjestelmänvalvojille jälkeen, kun tiedot on varmistaa, että todennus- ja tapa, jolla organisaation viestiä business niiden kumppaneiden kanssa. Todennus- ja vaatimukset suunnitteleminen, aikana varmistaa, että seuraaviin kysymyksiin vastataan:

- On organisaation todennusta ja sallia vain käyttäjät, jotka sijaitsevat niiden käyttäjätietojen hallintajärjestelmän?
 - Onko suunnitelmien B2B skenaarioissa?
 - Kyllä, jos jo tiedät protokollat (SAML, OAuth, Kerberos, tunnusten tai varmenteet) käytetään yhteyden molemmat yritykset?
- Onko hybrid identity-ratkaisun, joka aiot hyväksyy tuki protokollaa?

Huomioon otettavat toiseen kohtaan on mistä todennus säilöön, jonka avulla käyttäjät ja kumppanit on ja järjestelmänvalvojan mallin, jota käytetään. Ota huomioon seuraavat kaksi core asetukset:
- Keskitetty: tämän mallin käyttäjän tunnistetiedot, määrittäminen ja hallinta voi olla keskitetystä paikallisen tai pilveen.
- Hybrid: tämän mallin käyttäjän tunnistetiedot, määrittäminen ja hallinta on keskitetty paikallisen ja replikoitua pilveen.

Niiden business vaatimusten mukainen vaihtelevat organisaatiossa käytetään malli, jonka haluat tunnistavan missä käyttäjätietojen hallintajärjestelmän liittyville ja käyttämään järjestelmänvalvojan tilan seuraaviin kysymyksiin:

- Organisaation tällä hetkellä sisällä jäsenyyksien hallinta-ympäristöön?
 - Jos Kyllä, ne aiot pidä se?
 - Onko asetuksen tai yhteensopivuuden vaatimukset, että organisaatiosi on seurattava kyseisen mukaan määräytyy joissa käyttäjätietojen hallintajärjestelmän olisi sijaitsevat?
- Organisaatiosi käyttää kertakirjautumisen sovellukset, joka sijaitsee Paikalliset tai pilveen?
 - Jos Kyllä, hybrid tunnistetietojen mallin antamisen vaikuttaa yhteydessä?

## <a name="access-control"></a>Käyttöoikeuksien hallinta
Vaikka todennus- ja ovat core osat yrityksen tietojen kelpoisuuden käyttäjän käyttämiseksi, on myös tärkeää voit määrittää haluamasi käyttöoikeustaso, jonka he ja access-järjestelmänvalvojien taso kohdalla näkyy resurssit, joiden ne hallitaan. Hybrid tunnistetietojen ratkaisu on voitava muodostaa hajautetun pääsy resursseja, delegointi ja roolin perusosoitteen käyttöoikeuksien hallinta. Varmista seuraavaan kysymykseen vastataan koskevia käyttöoikeuksia:

- Onko yrityksesi useamman kuin yhden käyttäjän järjestelmänvalvojana käyttöoikeus identity-järjestelmän hallintaan?
 - Jos Kyllä, jokaisella käyttäjällä on sama käyttöoikeustaso?
- Käyttäjät voivat hallita resurssit edustajakäyttöoikeudet yrityksesi on?
 - Kyllä, kuinka usein tämä tapahtuu?
- Yrityksen on integroiminen ohjausobjektin käyttömahdollisuudet paikallisen ja cloud resursseja?
- Yrityksen resurssien mukaan jotkin ehdot käytön rajoittaminen on?
- Yrityksen on sovellus, jossa on mukautettu ohjausobjekti resurssien?
 - Jos Kyllä, jossa kyseisissä sovelluksissa sijaitsevat (paikallisen tai pilvipalvelussa)?
 - Jos Kyllä, jossa ovat ne kohde resursseja, joka sijaitsee (paikallisen tai pilvipalvelussa)?

>[AZURE.NOTE]
Varmista, että kunkin vastauksen kirjoittaa muistiinpanoja ja ymmärtää perusteet takana vastaus. [Määritä tietojen suojauksen strategia](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) siirtyvät vaihtoehtoja ja kunkin asetuksen eduista/Haittapuolia.  Vastaamalla näihin kysymyksiin valitsee sopii parhaiten sopivan vaihtoehdon yrityksesi tarpeisiin.

## <a name="next-steps"></a>Seuraavat vaiheet

[Tapauksen vastauksen vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
