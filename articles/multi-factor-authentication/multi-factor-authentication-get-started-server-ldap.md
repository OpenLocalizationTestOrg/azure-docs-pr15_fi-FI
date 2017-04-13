<properties 
    pageTitle="LDAP-todennusta ja Azure multi-factor Authentication-palvelin"
    description="Tämä on, jotka auttavat käyttöönotto LDAP-todennusta ja Azure multi-factor Authentication Server Azure multi-factor authentication-sivu."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-todennusta ja Azure multi-factor Authentication-palvelin


Oletusarvon mukaan Azure multi-factor Authentication palvelin on määritetty tuotavan tai Synkronoi käyttäjät Active Directorysta. Se voi kuitenkin määrittää sitominen eri LDAP-tiedostot, kuten ADAM hakemiston tai tietyn Active Directory-toimialueen ohjauskoneen. Kun määritetty muodostaa kautta LDAP-kansioon, Azure multi-factor Authentication Server voi määrittää LDAP-välityspalvelimen suorittamiseen välityspalvelimia edustajana. Sen avulla myös käytön LDAP sitominen RADIUS kohteeksi vanhat todennusta käyttäjien käytettäessä IIS-todennusta- tai ensisijainen käyttöoikeuden Azure multi-factor Authentication käyttäjä-portaalissa.

Käytettäessä Azure Monimenetelmäisen todentamisen LDAP-välityspalvelin Azure multi-factor Authentication-palvelin on lisätty lisääminen monimenetelmäisen todentamisen LDAP-asiakkaan (kuten VPN-laitteeseen, sovelluksen) ja LDAP-hakemistopalvelimen välillä. Azure multi-factor Authentication-funktion Azure multi-factor Authentication-palvelin on määritettävä client palvelimiin ja LDAP-hakemiston kanssa kommunikoimiseen. Tässä määrityksessä Azure multi-factor Authentication Server hyväksyy client palvelimiin ja sovellusten LDAP-pyynnöt ja välittää niitä vahvistamiseen ensisijainen tunnistetiedot kohde LDAP directory-palvelimeen. Jos LDAP-hakemiston vastaus näkyy, että ne ensisijainen tunnistetiedot ovat kelvollisia, Azure Monimenetelmäisen todentamisen suorittaa toisen monimenetelmäinen todentaminen ja lähettää vastauksen takaisin LDAP-asiakkaan. Koko todennus onnistuu vain, jos LDAP-palvelimeen todennus ja monimenetelmäisen todentamisen onnistu.





## <a name="ldap-authentication-configuration"></a>LDAP-todennuksen määrittäminen


Voit määrittää LDAP-todennus asentaminen Windows server Azure multi-factor Authentication-palvelimeen. Noudattamalla seuraavia vaiheita:

1. Siirtää Azure multi-factor Authentication-palvelimen Valitse vasemmanpuoleisessa valikossa LDAP-todennus-kuvake.
2. LDAP-todennus käyttöön-valintaruutu.![LDAP-todennus](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)
3. Asiakkaat-välilehdessä muuta porttinumeroa ja SSL-portti, jos Azure multi-factor Authentication LDAP-palvelu pitää sitoa normaalista portit kuunteleminen asiakkaat, jotka on määritetty LDAP-pyynnöt.
4. Jos aiot käyttää LDAPS asiakaskoneesta Azure multi-factor Authentication-palvelimeen, SSL-varmenne on asennettava palvelimeen suorittava palvelin. Valitse Selaa... SSL-varmenne-ruutuun seuraava-painiketta ja valitse asennettujen varmenne, jota käytetään suojatun yhteyden.
5. Valitse Lisää... painike.
6. Kirjoita LDAP-asiakkaan lisääminen-valintaikkunassa laitteen, palvelimeen tai sovellus, joka todentaa IP-osoite palvelimen ja sovelluksen nimi (valinnainen). Sovelluksen nimen Azure Monimenetelmäisen todentamisen raporteissa ja saa näkyviin SMS tai Mobile-sovelluksen käyttöoikeuksien viesteissä.
7. Valitse edellytä Azure Monimenetelmäisen todentamisen käyttäjän vastine-valintaruutu, jos kaikki käyttäjät on poistettu tai tuodaan tietoja palvelimeen ja mutli monimenetelmäinen todentaminen. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai tulee tehdä mutli kerroin todennusta, jätä ruutu ole valittuna. Katso lisätietoja ohjetiedoston tämän toiminnon.
8. Ehkä toista vaiheet 5 – 7, jos haluat lisätä muita LDAP-asiakkaat.
9. Kun Azure Monimenetelmäisen todentamisen on määritetty vastaanottamaan LDAP välityspalvelimia, se täytyy välityspalvelimen näiden välityspalvelimia LDAP-hakemiston. Tämän vuoksi Target näyttää välilehti käyttää yhden, vain harmaana asetus, kun LDAP-kohteen. LDAP-hakemisto-yhteyden määrittäminen, napsauta Hakemistointegrointi-kuvaketta.
10. Asetukset-välilehdessä Valitse Käytä tiettyä LDAP määritys-valintanappi.
11. Valitse Muokkaa. painike.
12. Muokkaa LDAP-määritys-valintaikkunassa lisätä kenttien yhdistäminen LDAP-hakemiston tarvittavat tiedot. Kenttien kuvaukset sisältyvät alla olevassa taulukossa. Huomautus: Nämä tiedot sisältyy myös Azure multi-factor Authentication Server ohjetiedosto.![Yhteystietojen integrointi](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)
13. Testaa LDAP-yhteys valitsemalla Testaa-painike.
14. Jos LDAP-Yhteystesti onnistui, valitse OK-painiketta.
15. Valitse suodattimet-välilehti. Palvelin on valmiiksi säilöjen, käyttöoikeusryhmät ja käyttäjien ladata Active Directorysta. Jos sitominen eri LDAP-hakemiston, tarvitset todennäköisesti näkyvissä suodattimien muokkaaminen. Napsauta Lisätietoja suodattimien Ohje-linkkiä.
16. Valitse määritteet-välilehti. Palvelin on valmiiksi yhdistämään määritteet Active Directorysta.
17. Jos sidonta eri LDAP-hakemiston tai muuttaa valmiiksi määritteiden yhdistämismääritykset, valitse Muokkaa. painike.
18. Muokkaa määritteiden-valintaikkunassa Muokkaa LDAP-määritteen yhdistämismääritykset hakemistossa. Määritteiden nimet on kirjoitettu- tai valittu napsauttamalla... kunkin kentän vieressä-painiketta.
19. Napsauta määritteet lisätietoja Ohje-linkkiä.
20. Napsauta OK-painiketta.
21. Yrityksen asetukset-kuvaketta ja valitse käyttäjänimi tarkkuus-välilehti.
22.CN Jos yhteyden muodostaminen Active Directoryyn toimialueeseen liittyneet palvelimesta, sinun pitäisi poistua Windowsin suojauksen tunnukset (SID) valitsemiseen käyttäjänimet valintanappi valittuna. Muussa tapauksessa valitse Käytä LDAP-yksilöllinen määritteen vastaavia käyttäjänimet-valintanappi. Kun asetus on valittu, Azure multi-factor Authentication Server yrittää ratkaista jokaisen käyttäjänimi yksilöllinen LDAP-hakemiston. LDAP-haku suoritetaan, valitse käyttäjänimi ja määritteitä, jotka on määritelty Directory-integrointi sitten määritteet-välilehti. Kun käyttäjä todentaa käyttäjänimi ratkaistava LDAP-hakemiston yksilöllinen ja yksilöllinen käytetään valitsemiseen käyttäjän Azure multi-factor Authentication-datatiedoston. Näin asennustavan vertailuissa, joita hyvin niin pitkän ja lyhyen käyttäjänimi muotoilut tämä suorittaa Azure multi-factor Authentication Server-määritys. Palvelin on nyt Kuuntele määritetyn porttien LDAP käyttöoikeuspyyntöjen määritetty asiakkaiden ja Määritä välityspalvelimen nämä pyynnöt LDAP-hakemiston todennusta varten.


## <a name="ldap-client-configuration"></a>LDAP-asiakkaan määritys

Jos haluat määrittää LDAP-asiakas, ohjeiden avulla:

- Määritä laitteen, palvelimen tai sovelluksen tarkistamiseen LDAP Azure multi-factor Authentication-palvelimen kautta, ikään kuin se olisi LDAP-hakemisto. Käytä samoja asetuksia, joka tavallisesti käyttäisit yhteyden muodostamiseen LDAP-hakemiston lukuun ottamatta palvelimen nimen tai jotka ovat Azure multi-factor Authentication-palvelimen IP-osoite.
- Määrittää LDAP-aikakatkaisu 30 – 60 sekuntia, jotta ole aikaa Vahvista käyttäjän tunnistetiedot LDAP-hakemiston kanssa, toinen tekijä-todennuksella, saat vastauksensa ja vastata LDAP-Käyttöoikeuspyyntöä.
- Jos käytät LDAPS, laitteen tai palvelimen LDAP-kyselyjen tekeminen on luotettava SSL-varmenne asennettuna Azure multi-factor Authentication-palvelimeen.
