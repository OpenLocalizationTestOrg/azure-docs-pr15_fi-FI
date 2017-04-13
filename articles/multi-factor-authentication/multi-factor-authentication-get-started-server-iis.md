<properties 
    pageTitle="IIS-todennus ja Azure multi-factor Authentication-palvelin"
    description="Tämä on Azure multi-factor authentication-sivu, jotka auttavat IIS-todennus ja Azure multi-factor Authentication Server käyttöönotto."
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

# <a name="iis-authentication"></a>IIS-todennus

Azure multi-factor Authentication-palvelimen IIS-todennus-osan avulla voit ottaa käyttöön ja määrittää IIS-todennus integrointi Microsoft IIS-web-sovellusten kanssa. Azure multi-factor Authentication Server asentaa laajennuksen joka suodattaa pyynnöt IIS-verkkopalvelin lisääminen Azure Monimenetelmäisen todentamisen. Laajennuksen IIS tukee Lomakepohjainen todennus- ja Windows HTTP todennusta. Luotetut IP-osoitteet voidaan määrittää myös vapauttaa sisäisiä IP-osoitteita kaksisuuntainen todennusta.


![IIS-todennus](./media/multi-factor-authentication-get-started-server-iis/iis.png)


## <a name="using-form-based-iis-authentication-with-azure-multi-factor-authentication-server"></a>Lomakepohjaisia IIS-todennusta käyttämällä Azure multi-factor Authentication-palvelimen kanssa

Suojaamiseen IIS-web-sovelluksen lomakepohjaisia todennusta käyttävä, asenna Azure multi-factor Authentication Server IIS-WWW-palvelimessa ja ‑palvelin kohti seuraavasti.

1. Siirtää Azure multi-factor Authentication-palvelimen Valitse vasemmanpuoleisessa valikossa IIS-todennus-kuvake.
2. Valitse lomakepohjaisia-välilehti.
3. Napsauta Lisää... painike.
4. Tunnistaa käyttäjänimi, salasana ja toimialueen muuttujat automaattisesti, kirjoita Login URL-osoite (esimerkiksi https://localhost/contoso/auth/login.aspx) Auto-Configure Form-Based sivusto-valintaikkunaan ja valitse OK.
5. Valitse edellytä Monimenetelmäisen todentamisen käyttäjän vastine-valintaruutu, jos kaikki käyttäjät on poistettu tai palvelimen ja veloittaa monimenetelmäisen todentamisen tuodaan. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai päivitetään Vapauta monimenetelmäisen todentamisen, jätä ruutu ole valittuna.
6. Jos sivun muuttujat ei tunnisteta automaattisesti, valitse Määritä manuaalisesti... painike Auto-Configure Form-Based sivuston-valintaikkunassa.
7. Kirjoita Add Form-Based julkinen verkkosivusto-valintaikkunaan kirjautumissivulle Lähetä URL-osoite-kenttään URL-osoite ja kirjoita sovelluksen nimi (valinnainen). Sovelluksen nimen Azure Monimenetelmäisen todentamisen raporteissa ja saa näkyviin SMS tai Mobile-sovelluksen käyttöoikeuksien viesteissä. Katso lisätietoja ohjetiedoston Lähetä URL-osoite.
8. Valitse oikea pyynnön muoto. Tämä on määritetty "VIESTIIN tai saat" useimmat verkkosovelluksissa.
9. Kirjoita käyttäjänimi-muuttuja, salasana muuttujan ja toimialueen muuttujan (jos se näkyy sivulla Kirjaudu sisään). Joudut ehkä siirtyminen selaimessa-kirjautumissivulle, napsauta sivua hiiren kakkospainikkeella ja valitsemalla "Näytä lähdekoodi" Etsi sivun syötteen ruutujen nimet.
10. Valitse edellytä Azure Monimenetelmäisen todentamisen käyttäjän vastine-valintaruutu, jos kaikki käyttäjät on poistettu tai palvelimen ja veloittaa monimenetelmäisen todentamisen tuodaan. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai päivitetään Vapauta monimenetelmäisen todentamisen, jätä ruutu ole valittuna. Katso lisätietoja ohjetiedoston tämän toiminnon.
11.  Valitse Lisäasetukset... Tarkista esiin Lisäasetukset, kuten mahdollisuus valita mukautetun eston sivun, tiedoston välimuistiin onnistuneen välityspalvelimia sivustoon ajan kuluessa evästeet ja valitse, haluatko todennetaan ensisijainen tunnistetiedot Windows-toimialue, LDAP-hakemiston tai RADIUS-palvelimen-painiketta. Kun olet valmis napsauta OK-painiketta voit palata Add Form-Based julkinen verkkosivusto-valintaikkunaan. Katso lisätietoja ohjetiedoston Lisäasetukset.
12. Napsauta OK-painiketta.
13. Kun URL-Osoitteen ja sivun muuttujat on havainnut tai kirjoitettu-sivustotiedot näy lomakepohjaisia-paneeli.
14. Katso käyttöön IIS laajennukset Azure multi-factor Authentication Server osan suoraan alla suorittamiseen IIS-todennuksen määrittäminen.

## <a name="using-integrated-windows-authentication-with-azure-multi-factor-authentication-server"></a>Integroitu Windows-todennusta käyttämällä Azure multi-factor Authentication-palvelimen kanssa

Suojaamiseen IIS-web-sovelluksen integroitu Windows HTTP todennusta käyttävä, asenna Azure multi-factor Authentication Server IIS-WWW-palvelimessa ja ‑palvelin kohti seuraavasti.

1. Siirtää Azure multi-factor Authentication-palvelimen Valitse vasemmanpuoleisessa valikossa IIS-todennus-kuvake.
2. Valitse HTTP-välilehti. Valitse lomakepohjaisia-välilehti.
3. Napsauta Lisää... painike.
4. Kirjoita URL-osoite, jossa HTTP-todennus on sivuston suoritetaan (esimerkiksi http://localhost/owa) Base URL-osoite-kenttään ja kirjoita sovelluksen nimi (valinnainen) Lisää Base URL-osoite-valintaikkuna. Sovelluksen nimen Azure Monimenetelmäisen todentamisen raporteissa ja saa näkyviin SMS tai Mobile-sovelluksen käyttöoikeuksien viesteissä.
5. Muuta aikakatkaisun ja suurin-ruutuun istunto jos oletusarvo ei ole tarpeeksi.
6. Valitse edellytä Monimenetelmäisen todentamisen käyttäjän vastine-valintaruutu, jos kaikki käyttäjät on poistettu tai palvelimen ja veloittaa monimenetelmäisen todentamisen tuodaan. Jos merkitsevän käyttäjämäärä ei ole vielä tuotu tietoja palvelimeen ja/tai päivitetään Vapauta monimenetelmäisen todentamisen, jätä ruutu ole valittuna. Katso lisätietoja ohjetiedoston tämän toiminnon.
7. Valitse eväste välimuisti-valintaruutu, jos toivottuja.
8. Napsauta OK-painiketta.
9. Kohdassa [Azure multi-factor Authentication Server käyttöön IIS-laajennukset](#enable-iis-plug-ins-for-azure-multi-factor-authentication-server) on kuvattu suoraan suorittamiseen IIS-todennuksen määrittäminen.


## <a name="enable-iis-plug-ins-for-azure-multi-factor-authentication-server"></a>IIS-laajennusten ottaminen käyttöön Azure multi-factor Authentication-palvelin

Kun olet määrittänyt lomakepohjaisia tai HTTP todennus URL-osoitteet ja asetuksia, sinun on valittava sijainnit, joissa Azure multi-factor Authentication IIS-laajennusten ladattu ja käytössä IIS. Noudattamalla seuraavia vaiheita:

1. Jos suoritat IIS 6-ISAPI-välilehti ja valitse sivusto, joka web-sovellus on käynnissä-kohdassa (kuten oletussivuston) käyttöön laajennuksen Azure multi-factor Authentication ISAPI suodatin, sivuston.
2. Jos käynnissä IIS 7 tai uudempi versio, napsauta alkuperäisen moduuli-välilehteä ja valitse palvelimeen, Web-sivustoista tai sovellukset käyttöön haluamasi siirtokorkeuteen laajennuksen IIS.
3. Valitse näytön yläreunassa käyttöön IIS-todennusta-vaihtoehto. Valittu IIS-sovellus on nyt suojaaminen Azure Monimenetelmäisen todentamisen. Varmista, että käyttäjien on tuotu palvelimeen. Kohdassa luotettujen IP-osoitteet on kuvattu, jos haluat whitelist sisäinen IP-osoitteita siten, kaksiosainen todentamismenetelmä ei tarvita, kyselyjä näistä sijainneista sivuston kirjauduttaessa.


## <a name="trusted-ips"></a>Luotettu IP-osoitteet

Luotetut IP-osoitteet avulla käyttäjät voivat ohittaa Azure Monimenetelmäisen todentamisen sivuston pyyntöjen peräisin IP-osoitteet tai aliverkosta. Esimerkiksi haluat ehkä vapautettujen käyttäjille from Azure multi-factor Authentication samalla, kun Office-kirjautuminen. Tämän ominaisuuden tuettavaksi määrität office-aliverkon Luotetut IP-osoitteet tekstiksi. Voit määrittää Luotetut IP-osoitteet noudattamalla seuraavia vaiheita:

1. Valitse IIS-todennus-osassa Luotetut IP-osoitteet-välilehti.
2. Napsauta Lisää... painike.
3. Lisää Luotetut IP-osoitteet-valintaikkuna tulee näkyviin, kun valitse yksittäinen IP, IP-alueen tai aliverkon valintanappi.
4. IP-osoitteiden alue tai aliverkon, jotka kuuluvat whitelisted Valvontakeskus IP-osoite. Jos aliverkon, valitse haluamasi verkkopeite ja napsauta OK-painiketta. Whitelist on nyt lisätty.
