<properties
    pageTitle="Azure Mobile välitys vertailu muiden samalla Azure-palvelujen kanssa"
    description="Azure Mobile välitys vertailu muiden samalla Azure-palvelujen kanssa - HockeyApp, AppInsights, ilmoitus keskittimet"
    services="mobile-engagement"
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Azure Mobile välitys vertailu muiden samalla Azure-palvelujen kanssa

Microsoft Azure tarjoamia palveluluetteloon koskaan laajentaminen ja joskus voi pohdit miten eroaa Azure Mobile välitys palvelusta juuri lukea tai kuunnella tietoja. Tässä artikkelissa yrittää Poista sekaannusta ja ohjaa sopivat Azure Mobile välitys ollessa sopii parhaiten käyttö. 
 
Azure Mobile välitys on palvelu kohdistettu **digitaalisen markkinoijat/CMOs varten** , mutta se voidaan käyttää **mobiilisovelluksen omistaja tai publisher** joka haluaa parantaa käyttö, säilytystä ja niiden mobiilisovellukset monetization. 

*Se on ohjelmiston nimellä--palvelun (SaaS) käyttäjän välitys ympäristö, joka sisältää tietoihin perustuvien tiedot sovelluksen käytön, reaaliaikainen käyttäjän Segmentointi kyselyjä ja mahdollistaa contextually huomioon push-ilmoitukset ja sovelluskohtaisesta messaging.* 

Jakamiseen Tämä määritelmä on seuraavia keskeisiä ominaisuuksia, jotka korostaa myös sen yksilöllinen arvo-ehdotus:

1.  **Contextually Ota push-ilmoitukset ja sovelluskohtaisesta viestintä**
        
    Tämä on tuotteen core kohdistus - suorittaa kohdennettujen ja mukautettuja push-ilmoitukset. Ja tämä voi johtua Microsoft kerää monipuolisia käytöspiirteen analytics-tiedot. 

2.  **Tietoihin perustuvien havainnollistamisen kyselyjä sovelluksen käytön**

    Annamme cross platform SDK: T voi kerätä tietoja sovelluksen käyttäjille käytöspiirteen analytics. Huomautus termin käytöspiirteen analytics (vastaan suorituskyvyn analytics), koska olemme keskittyä miten sovelluksen käyttäjät käyttävät sovellus. Microsoft kerää analytics basic suorituskyvyn virheitä, kaatuu jne mutta ei ole core kohdistuksen tuotteen tietoja. 

3.  **Reaaliaikainen käyttäjän Segmentointi**

    Kun olet kerännyt sovelluksen käyttäjille käytöspiirteen analytics-tiedot, emme Salli segmenttiin yleisön erilaisten parametrien perusteella ja kerättyjä tietoja, jotta voit suorittaa kohdennettujen push kampanjat. 

4.  **Ohjelmiston nimellä--palvelun (SaaS):**

    On erillään Azure hallinta-portaalin, joka on optimoitu vuorovaikutuksessa ja tarkastella monipuolisia käytöspiirteen analytics app-käyttäjien ja suorita markkinointi push Kampanjat portaali. Tuotteen on tarkoitettu auttaa sinua siirtymällä hetkessä!   
 
Tämä omaavien käsi erottelutarkkuus näin miten olemme verrataan muiden näennäisesti samalla Azure tarjouksia - artikkelin ei toimi ominaisuuksien tason vertailu mutta kestää useamman tilanne Huomautus perusteella voit määrittää, mikä tuote toimii parhaiten:
 
1.  [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) on on Microsoft mobile DevOps ratkaisu. Voit jakaa versiot beetatestaajien, kaatumisen tietojen kerääminen ja käyttäjän palautteen saaminen. Se myös integroitu Visual Studio Team Services helposti muodosta ominaisuuksissa ja työn kohteen integrointi. 
    
    Tähän keskitytään DevOps sekä keräävän suorituskyvyn analytics tiedot mobile-sovellukset. Voit ehkä lopputulos integrointi sekä HockeyApps ja Mobile välitys sovelluksen ja jotka eivät ole epätavallisia, koska vaikka on kerättyjen tietojen joitakin limitys-tuotteiden core kohdistus on eri ja niiden avulla eri tavoitteiden saavuttamiseen puolestasi.  

2.  [Hakemuksen tiedot](../application-insights/app-insights-overview.md) Jos mobile-sovellus on palvelinpuolen, valitse käytät hakemuksen tiedot seurannassa web palvelinpuolen sovelluksesi mutta asiakkaan puoli sovelluksen mobiiliversion, sinun täytyy käyttää HockeyApp. 

3.  [Ilmoitus keskittimet](https://azure.microsoft.com/services/notification-hubs/) tarjoaa kevyt palvelun siten, että sinun ei tarvitse integroitu mobiilisovelluksessa SDK-paketissa ja sinulla on täydet oikeudet mobile-sovellus ja se sallii lähettäminen push-ilmoitukset tunnisteiden perusominaisuudet. Tämä on hyvä minkä tahansa sovelluksen omistajan kuka arvostaa pienempi tietoja kohdistamisen ja lisää lähettäminen/yhteyden tietoja heti niiden sovelluksen käyttäjille (suuri populaation lähetys). 

    Huomautus kohdistus tähän lähettäminen blazing nopea ilmoitukset basic Segmentointi ominaisuuksien kanssa. 

Joissakin tilanteissa voit:

1.  TIM on osa digitaalisen markkinoinnin ryhmän Adventure Works-kaupasta, joka julkaisee mobiilisovellukset käyttäjille. TIM's lähinnä varmistaa, että käyttäjät, jotka Lataa mobile-sovellukset edelleen käyttää sitä ja usein. Tämä ei pelkästään kasvaa brändiä Liitä sovelluksen käyttäjille, mutta myös kasvaa monetization sovelluksen käyttäjille tehtyä ostot-Mobiilisovelluksella. Tämän ominaisuuden Tim on kohdistettu ilmoitukset lähetetään sovelluksen käyttäjille, jotka he hyödyllisiä, voit pyytää heitä Avaa sovellus ja palaat siihen usein. Tämä on missä Tim voi olla hyötyä Mobile välitys. 

2.  Joann kuuluu kehittäminen ryhmän jäsenille mobiilisovellukset Adventure Works-palvelussa ja kirjaudu kaatuu, poikkeuksia lukuun ottamatta tietoja ja pyydä suorituskyvyn telemetriatietojen sovelluksen tuotteen etsii. Tämä on missä Joann voi olla hyötyä HockeyApp. Joann voi integroida sekä HockeyApp hänen developer, jotka koskivat skenaarioissa ja Azure Mobile välitys Tim, saat parhaan molemmat samaan-sovelluksessa. 

3.  Mikko on kehittäminen ryhmän jäsenille mobiilisovellukset uutisia Contoso-verkon ja kaikki haluaa lähettää purkaa uutisia ilmoitusten kaikille käyttäjille ilman paljon Segmentointi heti, kun uutisia hälytyksen on osa. Tämä on missä Mikko voi olla hyötyä ilmoituksen keskittimet. Tässä skenaariossa voi kuitenkin, että kun mobiilisovelluksessa kehittyy, on tarpeen paljon tehokkaan Segmentointi ja sovelluksen käyttäjän toiminnan tietoja. Tällä hetkellä Mikko on arvioitava Azure Mobile välitys. 
 
Recap, Mobile välitys tarkoitus ei ole juuri kerätä analytics - ei "muuhunkin Analytics tuote Microsoftin". On lähettämisestä kohdennettujen push-ilmoitukset ja saat tämän kohdistamisen Microsoft kerää käytöspiirteen analytics-tiedot mutta kohdistus pysyy lähettäminen push-ilmoitukset, joka korostaa itsellesi sovelluksen käyttäjille, niin, että se ei tulla roskapostiksi. 

Lisätietoja – tutustu tämän [lyhyen videon](mobile-engagement-overview.md) Mobile välitys lyhyesti sanottuna. 

