<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - tunnistetietojen vaatimusten määrittäminen | Microsoft Azure"
    description="Tunnista yrityksen liiketoimintatarpeiden, joka johtaa voit määrittää hybrid käyttäjätiedot suunnittelun vaatimukset."
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

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Käyttäjätietojen vaatimusten hybrid identity-ratkaisun määrittäminen
Suunnitella hybrid identity-ratkaisun ensimmäiseksi on määrittämään business organisaatiossa, jossa hyödyntäminen tätä ratkaisua koskevat vaatimukset.  Hybrid tunnistetietojen nimellä (se tukee kaikkia ratkaisuja cloud antamalla todennusta) apuna käynnistyy, ja siirtyy, anna uusi ja kiinnostavimmat ominaisuuksia, joiden lukituksen poistaminen käyttäjien uusi työmääriä.  Näistä toiminnoista tai palveluja, jolle haluat antaa käyttäjien määräävät hybrid käyttäjätiedot suunnittelun vaatimukset.  Näiden palvelujen ja toiminnoista on hyödyntää hybrid tunnistetiedot sekä paikallisen ja pilveen.  

Vaatii päälle nämä keskeisiä ominaisuuksia business selvittääksesi, mikä se on nyt pyyntö ja yrityksen suunnitelmien tulevaisuudessa. Jos sinulla ei ole hybrid käyttäjätiedot suunnittelun pitkällä aikavälillä strategia näkyvyyden, on, että ratkaisu eivät ole skaalattava yrityksen tarpeiden suurennus ja muuttaminen.   T hän kaavio näyttää alla on esimerkki hybrid identity-arkkitehtuuri ja toiminnoista, jotka ovat parhaillaan lukitus käyttäjille. Tämä on vain esimerkki uusia mahdollisuuksia, jonka lukituksen ja tasainen hybrid tunnistetietojen strategia mukana toimitettu. 
 
Jotkin osat, jotka ovat osa hybrid identity-arkkitehtuuri![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Määrittää liiketoimintatarpeiden
Jokaisen yrityksen on erilaisia vaatimuksia, vaikka näiden yritysten saman alan vaatimukset saattavat vaihdella sen reaali liiketoiminnan osana. Parhaat käytännöt-teollisuuden voidaan hyödyntää edelleen, mutta kädessä yrityksen liiketoimintatarpeiden, joka johtaa voit määrittää hybrid käyttäjätiedot suunnittelun vaatimukset on. 

Varmista, että tunnistavan yrityksesi tarpeisiin seuraaviin kysymyksiin:

- Leikkaa IT toiminnallisia kustannukset yrityksesi Tarvitsetko?
- Yrityksen suojaamiseen cloud kalusto (SaaS sovellukset, infrastruktuuri) Tarvitsetko?
- Ajanmukaistaa IT yrityksesi Tarvitsetko?
  - Käyttäjät ovat nuolet ja vaativia IT-luoda poikkeuksia kyselyjä oman DMZ sallimaan erityyppisen liikenne eri resurssien käytön?
  - Onko yrityksesi vanhat sovellukset, joita tarvitaan Moderni käyttäjät julkaiseminen ei ole helppo kirjoittaa?
  - Täytyykö yrityksesi suorittaa seuraavia tehtäviä ja noutaa sen valvonnassa samalla kertaa?
- Yrityksen suojatun käyttäjien käyttäjätietojen ja vähentää riskin tuomalla uusia työkaluja, joilla hyödyntää Microsoft Azure suojauksen osaamisalueet paikallisen osaamisalueet Tarvitsetko?
- Yrittää yrityksesi vahingossa paikallinen dreaded "ulkoiset" tilit ja siirrä ne eivät enää sisällä paikallisen ympäristön passiiviset uhkien pilvessä?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analysoi paikallisen tunnistetietojen infrastruktuuri
Nyt kun olet luonut verrata koskeva yrityksen yrityksen tarpeiden, haluat arvioida paikallisen identity-infrastruktuuria. Arvioinnissa on tärkeää tekniset vaatimukset integroiminen nykyiseen käyttäjätiedot-ratkaisuun, cloud käyttäjätietojen hallintajärjestelmän määrittämistä varten. Varmista, että seuraaviin kysymyksiin:

- Mitä todennus- ja ratkaisu onko yrityksesi käyttää paikallisen? 
- Onko yrityksesi tällä hetkellä kaikki paikallisen synkronointipalvelut?
- Yrityksesi käyttää minkä tahansa kolmannen osapuolen tunnistetietojen toimittajat (IdP)?

Sinun on myös mahdolliset yrityksesi pilvipalveluihin huomioon. Suorittamiseen arvio ymmärtää SaaS, IaaS tai PaaS mallien ympäristössäsi nykyisen integrointi on erittäin tärkeää. Varmista, että seuraaviin kysymyksiin arvioinnissa aikana:
- Onko yrityksesi cloud-palveluntarjoajan integraatio?
- Jos Kyllä, mitkä palvelut käytetään?
- Integrointi parhaillaan tuotannon tai onko se koekäytön?


>[AZURE.NOTE]
Jos et on tarkkoja vastaavuuden kaikki sovellukset ja cloud services, voit Cloud App etsintä-työkalu. Tämä työkalu voi antaa IT-osaston näkyvyys kaikki organisaation business- ja kuluttaja cloud sovellukset. Jotka helpottavat varjostus IT organisaatiossa, mukaan lukien tiedot käyttötavat ja sellaisten käyttäjien cloud-sovellusten käyttäminen Tutustu. Voit käyttää tätä työkalua siirtymällä [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Käyttäjätietojen integrointi arvioiminen
Seuraavaksi sinun täytyy arvioiminen käyttäjätietojen integrointi. Arvioinnissa on tärkeää määrittää, kuinka käyttäjät todentaa, organisaation tavoitettavuuden ulkoasun pilveen, miten organisaatio Salli luvan ja mitä käyttäjäkokemuksen käyttäjästä tulee tekniset vaatimukset. Varmista, että seuraaviin kysymyksiin:

- Käyttävät organisaation federaatio, vakio todennuksen tai molemmat?
- Liitetty viestintä on tarpeen?  Koska seuraavasti:
 - Kerberos-pohjainen SSO
 - Yritykselläsi on paikallisen sovellukset, joka käyttää SAML tai samanlaisia federation ominaisuuksia (joko sisältyy osapuolen sisäiset tai 3).
 - MFA älykortit kautta. RSA SecurID jne.
 - Asiakkaan käyttösäännöt, jotka osoite seuraavia kysymyksiä:
     1. Voit estää kaikki ulkoisen käytön perustuu asiakkaan IP-osoite Office 365: een?
     1. Office 365: een, paitsi Exchange ActiveSync kaikki ulkoisen käytön estäminen
     1. Kaikki Office 365: n ulkoisen käytön lukuun ottamatta selainpohjaisia sovelluksia (OWA, SharePoint Onlinen) estäminen
     1. Office 365: een kaikki ulkoisen käytön estäminen nimettyjen AD-ryhmissä
- Koskee suojaus ja valvonta
- Aiemmin luodusta sijoitus liitetyssä todennuksessa
- Mitä nimi organisaation käyttää Microsoftin toimialueen pilvessä?
- Onko organisaation mukautettu toimialue?
    1. On toimialueella julkinen ja helposti oikeiden kautta DNS?
    1. Jos se ei ole, valitse sinulla on julkinen toimialue, jonka avulla voidaan rekisteröidä vaihtoehtoinen UPN AD?
- Yhdenmukaisuuden käyttäjän tunnukset cloud esitystä varten? 
- Onko organisaation sovellukset, jotka edellyttävät pilvipalveluihin integrointi?
- Organisaation on useita toimialueita, ja ne kaikki käyttää vakio tai liitetyt todennus?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Ympäristön suoritettavien sovellusten arvioiminen
On kuitenkin verrata koskeva paikallisen oman ja cloud infrastruktuurin, haluat arvioida näitä ympäristöissä suoritettavien sovellusten. Arvioinnissa on tärkeää tekniset vaatimukset integroiminen cloud käyttäjätietojen hallintajärjestelmän näiden sovellusten määrittäminen. Varmista, että seuraaviin kysymyksiin:

- Missä Microsoftin sovellukset sijaitsevat?
- Käyttäjät pääsevät paikallisen sovelluksia?  Pilvessä? Tai molemmat?
- Onko suunnitelmien kestää olemassa olevan sovelluksen toiminnoista ja siirrä ne pilveen?
- Onko aikoo kehittää uusia sovelluksia, jotka sisällytetään sijaitsevat joko paikallisessa tai pilvipalvelussa, jotka käyttävät cloud todennusta?

## <a name="evaluate-user-requirements"></a>Käyttäjän arvioiminen
Sinun on myös arvioiminen käyttäjä. Arvioinnissa on tärkeää määrittää vaiheet, jotka tarvitaan lennolle-ja avustaminen käyttäjät, kun ne siirtyminen pilveen. Varmista, että seuraaviin kysymyksiin:

- Käyttäjät pääsevät sovellusten paikallisen?
- Käyttäjät pääsevät sovellusten pilvessä?
- Miten käyttäjät tavallisesti kirjautuessasi niiden paikalliseen ympäristöön?
- Miten käyttäjät kirjautua sisään pilvessä?

>[AZURE.NOTE]
Varmista, että kunkin vastauksen kirjoittaa muistiinpanoja ja ymmärtää perusteet takana vastaus. [Selvittäminen tapauksen vastauksen vaatimukset](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) siirtyvät vaihtoehtoja ja kunkin asetuksen ammattilaisille ja haittoja.  Mukaan on vastannut näihin kysymyksiin valitaan sopii parhaiten sopivan vaihtoehdon yrityksesi on.

## <a name="next-steps"></a>Seuraavat vaiheet
[Kansion synkronoinnin vaatimusten määrittäminen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
