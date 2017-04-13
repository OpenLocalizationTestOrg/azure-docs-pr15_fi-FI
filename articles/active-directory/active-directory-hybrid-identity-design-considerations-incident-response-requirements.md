
<properties
    pageTitle="Azure Active Directoryn hybrid tunnistetietojen tyyliseikat - tapauksen rResponse vaatimusten määrittäminen | Microsoft Azure-vaatimukset "
    description="Määritä valvonta- ja ominaisuudet, jotka voivat olla hyödyntää hybrid identity-ratkaisun se tunnistaa ja pienentämään mahdollisten uhkien toimintoja"
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Hybrid identity-ratkaisun tapauksen vastauksen vaatimusten määrittäminen

Suuri tai Normaali organisaatiot todennäköisesti on [tapauksen vastauksen suojaus](https://technet.microsoft.com/library/cc700825.aspx) auttaa IT toimia vastaavasti tapahtumasta tasolle. Käyttäjätietojen hallintajärjestelmän on tärkeä osa tapauksen vastauksen prosessin, koska se voidaan käyttää apuna tunnistaminen suorittanut tietyn toiminnon kohdearvoon. Hybrid identity-ratkaisun voivat antaa valvonta- ja ominaisuuksista, joita voidaan hyödyntää mukaan se tunnistaa ja pienentämään mahdollisten uhkien toimintoja. Tyypillinen tapauksen vastaus-suunnitelmassa on seuraavat vaiheet suunnitelman osana:

1.  Alustava arviointi.
2.  Tapauksen viestintä.
3.  Vioittuneet ohjausobjekti ja riskien vähentämistä.
4.  Mitä tunnus on haavoittuvan ja vakavuus.
5.  Näyttöä säilyttämistä.
6.  Asianmukaiset osapuolet-ilmoituksen.
7.  Järjestelmän palautus.
8.  Asiakirjat.
9.  Vioittuneet ja kustannusten arviointi.
10. Prosessin ja sopimus-versio.

Aikana tunnus sen, mitä se oli haavoittuvan ja vakavuus vaihe, se on tarpeen tunnistaa järjestelmät, käsiin, tiedostot, jotka on käytetty ja määrittää kyseiset tiedostot suojaustasoa. Hybrid tunnistetietojen järjestelmän pitäisi täyttävät kaikki seuraavat vaatimukset, voit helpottaa tunnistaminen käyttäjälle, joka on tehnyt muutokset. 

## <a name="monitoring-and-reporting"></a>Seuranta ja raportointi
Monta kertaa tunnistetietojen järjestelmä auttaa myös alustava arviointi vaiheen pääasiassa, jos järjestelmä on luonut tarkistaminen ja raportoinnissa ominaisuuksia. Alkuperäinen arvioinnin aikana IT-järjestelmänvalvoja on voitava muodostaa epäilyttävistä tehtävän määritys tai järjestelmän pitäisi onnistua käynnistimen pohjalta valmiiksi määritetyn tehtävän automaattisesti. Aktiviteetteja saattaa johtua mahdollinen hyökkäyksen, mutta muissa tapauksissa väärin määritetty järjestelmän saattaa johtaa tunnistettujen useita tunkeutumisen tunnistus järjestelmän. 

Käyttäjätietojen hallintajärjestelmän olisi auttaa tunnistamaan ja raportin kyseisiin epäilyttävistä toimintoihin IT-järjestelmänvalvojat. Yleensä kyseiset tekniset vaatimukset on täytettävä seuraamalla kaikki järjestelmät ja ottaa raportoinnin ominaisuus, joka korostaa mahdollisten uhkien. Alla olevia kysymyksiä avulla voit suunnitella hybrid identity-ratkaisun ottaen huomioon tapauksen vastauksen vaatimukset:

- Yritykselläsi on tapauksen vastauksen suojaus paikassa?
 - Kyllä, jos nykyinen käyttäjätietojen hallintajärjestelmän käytetään yhteydessä?
- Tarvitseeko yrityksesi tunnistavan epäilyttävistä Sign yritykset käyttäjiltä yli eri laitteissa?
- Yrityksen tarvitse tunnistaa mahdolliset vaarantuneen käyttäjän tunnistetiedot
- Tarvitseeko yrityksesi voit valvoa käyttäjän access ja toiminto?
- Tarvitseeko yrityksesi tietää, kun käyttäjän salasanan hänen?

## <a name="policy-enforcement"></a>Käytännön käyttäminen

Vioittuneet ohjausobjekti ja riskien vähentämistä-vaiheen aikana on tärkeää vähentää nopeasti hyökkäys todellisia ja mahdolliset vaikutukset. Toiminto, joka otetaan tehdä tässä vaiheessa pienet ja suuret yksi ero. Tarkka vastaus riippuu organisaation ja laatu, kohtaat hyökkäyksen. Jos alustava arviointi tehneet, että tili on käsiin, sinun on voidaan valvoa käytännön, joka estää tähän tiliin. Tämä on vain yksi esimerkki missä käyttäjätietojen hallintajärjestelmän hyödyntää. Alla olevia kysymyksiä avulla voit suunnitella hybrid identity-ratkaisun ottaen huomioon, kuinka käytännöt pakotetaan suoritettaviksi vastata meneillään oleviin tapahtuma:

- Yrityksesi on käytännöt paikassa käyttäjien estäminen Accessista verkon tarvittaessa?
 - Jos Kyllä, nykyistä ratkaisua integroida hybrid käyttäjätietojen hallintajärjestelmän, jotka aiot hyväksyy?
- Tarvitseeko yrityksesi voidaan valvoa ehdollisen käyttöoikeuden käyttäjille, joilla on karanteenissa? 
 
>[AZURE.NOTE]
Varmista, että kunkin vastauksen kirjoittaa muistiinpanoja ja ymmärtää perusteet takana vastaus. [Määritä tietojen suojauksen strategia](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) siirtyvät vaihtoehtoja ja kunkin asetuksen eduista ja haittoja.  Mukaan on vastannut näihin kysymyksiin valitaan sopii parhaiten sopivan vaihtoehdon yrityksesi on.

## <a name="next-steps"></a>Seuraavat vaiheet
[Tietojen suojauksen toimintamallin](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Katso myös
[Rakenteen huomioon otettavia seikkoja yleiskatsaus](active-directory-hybrid-identity-design-considerations-overview.md)
