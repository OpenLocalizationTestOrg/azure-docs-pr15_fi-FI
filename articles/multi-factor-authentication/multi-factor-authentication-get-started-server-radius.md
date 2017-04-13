<properties 
    pageTitle="SÄDE-todennus ja Azure multi-factor Authentication-palvelin"
    description="Tämä on, jotka auttavat käyttöönotto RADIUS-todennus ja Azure multi-factor Authentication Server Azure multi-factor authentication-sivu."
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
    ms.date="08/15/2016"
    ms.author="kgremban"/>



# <a name="radius-authentication-and-azure-multi-factor-authentication-server"></a>SÄDE-todennus ja Azure multi-factor Authentication-palvelin

SÄDE todentaminen ‑osassa voit ottaminen käyttöön ja määrittäminen Azure multi-factor Authentication-palvelimen todennuksen RADIUS. SÄDE on vakio protokolla Hyväksy käyttöoikeuksien ja käsitellä nämä pyynnöt. Azure multi-factor Authentication Server toimii RADIUS-palvelin ja RADIUS asiakkaan (kuten VPN-laitteen) ja todennus-kohteen, jotka voivat olla Active Directory (AD), LDAP-hakemiston tai jokin toinen SÄDE-palvelin, jotta voit lisätä Azure Monimenetelmäisen todentamisen väliin. Sinun on määritettävä Azure multi-factor Authentication-funktion, Azure multi-factor Authentication Server niin, että se voi olla yhteydessä client palvelimiin ja käyttöoikeuksien kohde. Azure multi-factor Authentication Server RADIUS-asiakas hyväksyy, tarkistaa käyttöoikeudet todennus tavoitteesta, Lisää Azure Monimenetelmäisen todentamisen ja lähettää vastauksen takaisin RADIUS-asiakas. Koko todennus onnistuu vain, jos ensisijainen käyttöoikeuden ja Azure Monimenetelmäisen todentamisen onnistuu.

>[AZURE.NOTE]
>MFA-palvelin tukee vain PAP (salasanan todennusta protocol)-MSCHAPv2 (Microsoftin todennus Handshake Authentication Protocol) RADIUS protokollat toimiva RADIUS-palvelin.  Muita protokollia, kuten EAP (extensible authentication protocol) voidaan, kun MFA palvelimen toimii RADIUS-välityspalvelimen toiseen SÄDE-palvelimeen, joka tukee, kuten Microsoft NPS-protokollaa.
></br>
>Käytettäessä muita protokollia Tässä määrityksessä yksisuuntainen SMS ja sijasta tunnusten ei toimi, koska MFA palvelimeen ei voi aloittaa onnistuneen RADIUS todennus vastausta, protokollan avulla.


![Säde-todennus](./media/multi-factor-authentication-get-started-server-rdg/radius.png)

## <a name="radius-authentication-configuration"></a>SÄDE-todennuksen määrittäminen

SÄDE-käyttöoikeuksien määrittämiseen asentaminen Windows server Azure multi-factor Authentication-palvelimeen. Jos sinulla on Active Directory-ympäristössä, palvelimen liitetty toimialueen sisällä verkkoon. Seuraavien ohjeiden avulla voit määrittää Azure multi-factor Authentication-palvelimeen:

1. Siirtää Azure multi-factor Authentication-palvelimen Valitse vasemmanpuoleisessa valikossa RADIUS todennus-kuvake.
2. Valitse Ota käyttöön RADIUS todennus-valintaruutu.
3. Asiakkaat-välilehdessä muuta todennus portit ja laskenta-portit jos RADIUS Azure multi-factor Authentication-palvelun pitää sitoa normaalista portit kuunteleminen asiakkaat, jotka on määritetty SÄDE-pyynnöt.
4. Valitse Lisää... painike.
5. Kirjoita Lisää RADIUS-asiakas-valintaikkunassa Azure multi-factor Authentication-palvelimeen, sovelluksen nimi (valinnainen) ja Shared salaisuus, joka todentaa laitteen/palvelimen IP-osoite. Jaetun toiminta on sama sekä laitteen/server Azure multi-factor Authentication Server. Sovelluksen nimen Azure Monimenetelmäisen todentamisen raporteissa ja saa näkyviin SMS tai Mobile-sovelluksen käyttöoikeuksien viesteissä.
6. Valitse edellytä Monimenetelmäisen todentamisen käyttäjän vastine-valintaruutu, jos kaikki käyttäjät on poistettu tai palvelimen ja veloittaa monimenetelmäisen todentamisen tuodaan. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai päivitetään Vapauta monimenetelmäisen todentamisen, jätä ruutu ole valittuna. Katso lisätietoja ohjetiedoston tämän toiminnon.
7. Valitse Ota käyttöön varakyselyjen sijasta suojaustunnuksen valintaruutu, jos käyttäjät käyttävät Azure Monimenetelmäisen todentamisen mobiilisovelluksen todennus ja haluat käyttää sijasta salasanat Luokittele puhelin puhelu, SMS tai push ilmoitukseen varakyselyjen todennusta.
8. Napsauta OK-painiketta.
9. Ehkä toista vaiheet 4 – voit lisätä muita RADIUS-asiakkaat 8.
10. Valitse kohde-välilehti.
11. Jos Azure multi-factor Authentication-palvelimeen on asennettu toimialueen liittyneet palvelimessa Active Directory-ympäristössä, valitse Windows-toimialue.
12. Jos käyttäjät todennetaan LDAP-hakemiston vastaan, valitse LDAP sitominen. LDAP-sitominen käytettäessä Hakemistointegrointi-kuvaketta ja Muokkaa asetukset-välilehdessä LDAP-määritys, niin, että palvelin sitoa hakemistossa. Ohjeet määrittämisestä LDAP löytyy LDAP-välityspalvelimen määritys-oppaassa.
13. Jos käyttäjät todennetaan toiseen SÄDE-palvelinta, valitse SÄDE-palvelimesta.
14. Määrittää, että palvelin on välityspalvelimen SÄDE-pyyntöjä valitsemalla Lisää palvelimen... painike.
15. Kirjoita Lisää RADIUS-palvelin-valintaikkunassa RADIUS-palvelin ja jaettu salaisuus IP-osoite. Jaetun toiminta on sama Azure multi-factor Authentication Server ja SÄDE-palvelimeen. Muuttaa käyttöoikeuksien portti- ja laskenta-portti, jos eri portit RADIUS-palvelimen avulla.
16. Napsauta OK-painiketta.
17. Sinun on lisättävä Azure multi-factor Authentication Server SÄDE-asiakkaaksi RADIUS-palvelin, niin, että se käsittelee käyttöoikeuspyyntöjen lähettää Azure multi-factor Authentication-palvelimesta. Sinun on käytettävä samaa jaetun salaisuus määritetty Azure multi-factor Authentication Server.
18. Ehkä toista tämä vaihe muut SÄDE-palvelimet lisättävä ja määritettävä siinä järjestyksessä, jossa palvelimen olisi puhelu ne ja Siirrä ylös- ja Siirrä alas-painikkeita. Tämä suorittaa Azure multi-factor Authentication Server-määritys. Palvelin on nyt Kuuntele RADIUS käyttöoikeuspyyntöjen määritetty asiakkaiden määritetty porttiin.   


## <a name="radius-client-configuration"></a>SÄDE asiakkaan määritys

Voit määrittää RADIUS-asiakas, ohjeiden avulla:

- Määritä laitteen/palvelin todentaa Azure multi-factor Authentication palvelimen IP-osoite, joka toimii RADIUS-palvelimen kautta RADIUS.
- Käytä samaa jaettu salaisuus, joka on määritetty yläpuolella.
- Määritä 30 – 60 sekuntia SÄDE-aikakatkaisu, niin, että ole aikaa Vahvista käyttäjän tunnistetiedot, suorittaa monimenetelmäisen todentamisen, saat vastauksensa ja vastata RADIUS Käyttöoikeuspyyntöä.
