<properties 
    pageTitle="Työpöydän yhdyskäytävän ja Azure multi-factor Authentication Server RADIUS käyttäminen"
    description="Tämä on Azure multi-factor authentication-sivu, jotka auttavat Remote työpöytä (RD) yhdyskäytävän ja Azure multi-factor Authentication Server käyttämällä RADIUS käyttöönotto."
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

# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>Työpöydän yhdyskäytävän ja Azure multi-factor Authentication Server RADIUS käyttäminen

Monissa tapauksissa Remote Desktop yhdyskäytävä käyttää paikallisen Network Policy käyttäjien todentamiseen. Tässä asiakirjassa kerrotaan, miten reitittämään RADIUS pyytää ulos Remote Desktop-yhdyskäytävän (kautta paikallisen Network Policy) multi-factor Authentication-palvelimeen.

Multi-factor Authentication-palvelimeen on asennettava muussa palvelimessa, joka sitten välityspalvelimen takaisin työpöydän yhdyskäytävän etäpalvelimeen Network Policy SÄDE-pyynnön. Kun NPS tarkistaa käyttäjänimi ja salasana, se palauttaa vastauksen, joka suorittaa todennusta ennen yhdyskäytävään, joka palauttaa tuloksen toinen tekijä multi-factor Authentication-palvelimeen.





## <a name="configure-the-rd-gateway"></a>RD-yhdyskäytävän asetusten määrittäminen

RD-yhdyskäytävä on oltava määritettynä RADIUS todennus lähettäminen Azure multi-factor Authentication-palvelimeen. Kun Etätyöpöydän yhdyskäytävä on asennettu, määritetty ja on käytät, valitse Etätyöpöydän yhdyskäytävä-ominaisuuksiin. RD pää Store-välilehti ja muuta sen asetukseksi Määritä sen sijaan, että paikallinen palvelimeen NPS NPS keskitetyn palvelimen. Lisää yksi tai useampi Azure multi-factor Authentication palvelin RADIUS-palvelimia ja määritä salaisuus kullekin palvelimelle.





## <a name="configure-nps"></a>Määritä NPS

RD-yhdyskäytävä käyttää NPS lähettää SÄDE-pyynnön Azure Monimenetelmäisen todentamisen. Voit estää RD Gatewayn ennen valmistumista monimenetelmäisen todentamisen aikakatkaistu on vaihdettava aikakatkaisu. Seuraavien ohjeiden avulla voit määrittää NPS.

1. NPS Laajenna vasemmanpuoleisessa sarakkeessa RADIUS asiakkaan ja palvelimen-valikko ja valitse Remote RADIUS palvelimen ryhmiä. Siirry TS YHDYSKÄYTÄVÄN SERVER-ryhmän ominaisuudet. Muokkaa näkyviin SÄDE-palvelimesta ja kuormituksen tasaamisen-välilehti. Muuta "sekunteina ilman vastausta ennen kuin pyyntö katsotaan pudotettu" ja "Sekuntimäärä välillä pyynnöt, kun palvelin on määritetty ei käytettävissä" 30 – 60 sekuntia. Todennus ja tili-välilehdessä ja varmista, että määritettyä SÄDETTÄ portit vastaavat portit, jotka multi-factor Authentication Server Kuuntele.
2. NPS on myös määritettävä vastaanottamaan RADIUS välityspalvelimia takaisin Azure multi-factor Authentication-palvelimeen. Valitse vasemmanpuoleisessa valikossa SÄDE-asiakassovelluksissa. Lisää RADIUS-asiakkaaksi Azure multi-factor Authentication-palvelimeen. Valitse nimi ja määritä salaisuus.
3. Laajenna käytännöt-osan vasemmassa siirtymisruudussa ja valitse sitten yhteyden pyytää käytännöt. Se on oltava yhteys pyytää käytännön nimeltä TS YHDYSKÄYTÄVÄN luvan käytännön, joka on luotu, kun etätyöpöydän yhdyskäytävä on määritetty. Tämän käytännön välittää RADIUS pyynnöt multi-factor Authentication-palvelimeen.
4. Kopioi tämän voit luoda uuden käytännön. Lisää ehto, joka vastaa asiakkaan kutsumanimi kutsumanimi ja määritä vaiheessa 2 RADIUS Azure multi-factor Authentication-palvelin-asiakasohjelman uuden käytännön. Muuta käyttöoikeustarkistuspalvelun paikalliseen tietokoneeseen. Tämän käytännön varmistaa, että kun SÄDE-pyyntö on vastaanotettu Azure multi-factor Authentication-palvelimesta, todennus tapahtuu paikallisesti lähettää RADIUS Azure multi-factor Authentication palvelimeen joka seuraa silmukka ehdon sijaan. Jotta Silmukkaehto, uusi käytäntö on tilattava YLÄPUOLELLA alkuperäisen käytännön, joka välittää multi-factor Authentication-palvelimeen.

## <a name="configure-azure-multi-factor-authentication"></a>Azure Monimenetelmäisen todentamisen määrittäminen


--------------------------------------------------------------------------------



Azure multi-factor Authentication palvelin on määritetty RD yhdyskäytävän ja NPS RADIUS-välityspalvelin.  Se on asennettava toimialueen liittyneet palvelimessa, jossa on erillinen Etätyöpöydän yhdyskäytävä-palvelimesta. Seuraavien ohjeiden avulla voit määrittää Azure multi-factor Authentication-palvelimeen.

1. Avaa Azure multi-factor Authentication-palvelin ja RADIUS todennus-kuvaketta. Valitse Ota käyttöön RADIUS todennus-valintaruutu.
2. Varmista portit vastaavat, mikä on määritetty NPS asiakkaat-välilehti ja lisää... painike. Lisää RD yhdyskäytävän palvelimen IP-osoite, sovelluksen nimi (valinnainen) ja salaisuus. Jaetun toiminta on sama Azure multi-factor Authentication Server ja etätyöpöydän yhdyskäytävä.
3. Valitse kohde-välilehti ja sitten RADIUS palvelimesta-valintanappi.
4. Napsauta Lisää... painike. Kirjoita IP-osoite, jaetun ja NPS palvelimen portit. Ellei käyttämällä keskitetyn NPS RADIUS asiakkaan ja RADIUS kohde on sama. Jaetun toiminta on vastattava NPS-palvelimen RADIUS asiakas-osassa yksi asetukset.

![Säde-todennus](./media/multi-factor-authentication-get-started-server-rdg/radius.png)
